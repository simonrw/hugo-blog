---
title: An experiment with trustfall
date: 2023-11-20
tags:
- rust
- graphql
summary: My thoughts on implementing a toy trustfall project
---

[Trustfall][trustfall] is a GraphQL query engine, but without the web stuff meaning it is really designed for performing queries on data.
An example application is given in ["How to Query (Almost) Everything"](https://github.com/obi1kenobi/trustfall?tab=readme-ov-file#10min-tech-talk--demo) which shows combining multiple data sources:

* Hacker News posts
* GitHub projects
* GitHub actions jobs

I wanted to learn how difficult implementing something like this would be.
I have only scratched the surface, but I think I have a vague understanding that is worth writing down.

## API design

To start off, define the types of your API.
This is likely directly related to the data that you want to query.
For example, with these types:

```rust
struct Bar {
    quux: String,
    baz: Vec<Baz>,
}

struct Baz {
    value: String,
}
```

there needs to be a "top level" type which defines the top level query, and this _must_ borrow the data it represents.

```rust
struct Foo<'a> {
    bar: &'a [Bar],
}
```

in this case the GraphQL schema looks like:

```graphql
schema {
  query: RootSchemaQuery
}

type RootSchemaQuery {
  foo: Bar!
}

type Bar {
  quux: String!
  baz: Baz,
}

type Baz {
  value: String!
}
```

You must match the `Bar` and `Baz` types in this case to your data.

## The Adapter

[Trustfall][trustfall] depends on implementing a trait for your data.
This implementation is known as an _adapter_.

A [simplified adapter trait](https://docs.rs/trustfall/latest/trustfall/provider/trait.BasicAdapter.html) is available to simplify the process, with the following methods:

* `resolve_starting_vertices`
* `resolve_property`
* `resolve_neighbors`
* `resolve_coercion`

Let's annotate these on an schema:

```graphql
schema {
  query: RootSchemaQuery
}

type RootSchemaQuery {
  foo: Bar!
}

type Bar {
  quux: String!
  baz: Iface,
}

type Baz implements Iface {
  value: String!
}

interface Iface {
  value: String!
}
```

and query:

```graphql
{
  foo {                 # <- resolve_starting_vertices
    quux                # <- resolve_property
    baz {               # <- resolve_neighbors
      ... on Baz {      # <- resolve_coercion
        value           # <- resolve_property
      }
    }
  }
}
```

## Creating a Vertex

The final piece of the puzzle is to define the `Vertex` type.
This type is effectively returned by all trait methods, and must therefore represent all return types from the query engine.
Because of this it is common to define an `enum` to represent the `Vertex`.
This type stores references to inner data types, for example:

```rust
#[derive(Clone, Debug, TrustfallEnumVertex)]
enum Vertex<'a> {
    Bar(&'a Bar),
    Baz(&'a Baz),
}
```

Note the deriving of `TrustfallEnumVertex`: this is important later on.
Any type that is not a "primitive" type (e.g. `String`, number etc.) must be representable by the `Vertex` type.

## Implementing the adapter

Now we understand what the trait methods do, let's implement a stub adapter for the schema above:

```rust
// Implement a method to resolve the top level query into a `Vertex` type. This
fn resolve_starting_vertices(
    &self,
    edge_name: &str,
    _parameters: &trustfall::provider::EdgeParameters,
) -> trustfall::provider::VertexIterator<'a, Self::Vertex> {
    match edge_name {
        "foo" => {
            // We are interested in the `foo` field on the top level query.
            // Return a boxed iterator over `Vertex` instances
            Box::new(self.bars.iter().map(|b| Vertex::Bar(b)))
        },
        _ => unreachable!(),
    }
}
```

Resolve a property on a type.
Note how we use the methods added by the `TrustfallEnumVertex` derive macro.

```rust
// Given a vertex, resolve the property requested by `type_name`
fn resolve_property<V: trustfall::provider::AsVertex<Self::Vertex> + 'a>(
    &self,
    contexts: trustfall::provider::ContextIterator<'a, V>,
    type_name: &str,
    property_name: &str,
) -> trustfall::provider::ContextOutcomeIterator<'a, V, trustfall::FieldValue> {
    match type_name {
        "Bar" => match property_name {
            // resolve_property_with is a helper function that looks up the
            // property in the `contexts` variable
            //
            // field_property relies on conversion functions from the `TrustfallEnumVertex`
            // trait, and `quux` is the name of the property
            "quux" => resolve_property_with(contexts, field_property!(as_bar, quux)),
            _ => unreachable!(),
        },
        // ...
    }
}
```

Resolve any "links" i.e. properties on parent types that resolve to child types.

```rust
fn resolve_neighbors<V: trustfall::provider::AsVertex<Self::Vertex> + 'a>(
    &self,
    contexts: trustfall::provider::ContextIterator<'a, V>,
    type_name: &str,
    edge_name: &str,
    parameters: &trustfall::provider::EdgeParameters,
) -> trustfall::provider::ContextOutcomeIterator<
    'a,
    V,
    trustfall::provider::VertexIterator<'a, Self::Vertex>,
> {
    match (type_name, edge_name) {
        ("Bar", "baz") => {
            // resolve_neighbors_with is a helper function provided by trustfall.
            // Again it uses the `TrustfallEnumVertex` trait.
            resolve_neighbors_with(contexts, |vertex| {
                let neighbors = vertex.as_bar().unwrap().baz.iter().map(Vertex::Baz);
                Box::new(neighbors)
            })
        },
        _ => unreachable!(),
    }
}
```

## Things I have not yet looked into

* `Adapter` vs `BasicAdapter`
* If we can do away with the stringified API for matching type/property names
* Property/link arguments
* directives
* type coercions


[trustfall]: https://github.com/obi1kenobi/trustfall

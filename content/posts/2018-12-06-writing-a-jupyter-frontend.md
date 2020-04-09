---
title: Jupyter client library
Authors: Simon Walker
date: 2018-12-06 19:00:20
category: tech
tags:
- rust
---

**TL;DR I have been thinking about alternative frontends to the Jupyter notebook. The first step is to abstract communications with Jupyter kernels. I have started writing one of these.**

* [Project repository](https://gitlab.com/srwalker101/rust-jupyter-client)
* [docs.rs](https://docs.rs/jupyter-client/0.1.0/jupyter_client/)
* [crates.io](https://crates.io/crates/jupyter-client)

I am a huge fan of the Jupyter notebook environment. It is a fantastic way to explore a new data analysis approach, and keep results together with the implementation and explanation.

I wanted to understand how the Jupyter notebook/lab frontend interacts with the Kernel backends, and as such have started writing a _client_ library.

## Architecture

The Jupyter suite consists of two parts:

* Frontend
* Kernel

Within the frontend is typically a kernel communication abstraction component, what I call the "client":

![Jupyter schematic]({filename}images/jupyter_schematic.svg)

The frontend handles nicely presenting the execution results and collecting input from the user (i.e. the _notebook_). The _kernel_ is the execution engine that can be written in many languages. The _client_ handles communicating with a kernel. It communicates over 4[^1] ZeroMQ sockets:

* `shell`: for client -> kernel requests (and responses)
* `iopub`: for kernel -> client messages
* `heartbeat`: for health checks
* `stdin`: for kernel -> client requests (and responses from the frontend)

[^1]: Technically 5: a "control" socket is a duplicate of the "shell" socket but for high-priority messages.

This abstraction is what allows the frontend to communicate with different kernels.

## Writing a client library

The [documentation on the Jupyter wire protocol][wire-protocol] is pretty good at describing how messages are sent to and from the kernel over the "wire" (the various ZeroMQ sockets). This documentation was used to build a [`WireMessage`](https://gitlab.com/srwalker101/rust-jupyter-client/blob/b8c27ea2ca8b5a546fb59ed06c1d9a4e50575552/src/wire.rs#L17) abstraction, which:

* is created from a [`Command`](https://docs.rs/jupyter-client/0.1.0/jupyter_client/commands/enum.Command.html) enum variant ([conversion method](https://gitlab.com/srwalker101/rust-jupyter-client/blob/b8c27ea2ca8b5a546fb59ed06c1d9a4e50575552/src/commands.rs#L135)),
* gets passed to a [`Socket`](https://gitlab.com/srwalker101/rust-jupyter-client/blob/b8c27ea2ca8b5a546fb59ed06c1d9a4e50575552/src/socket.rs#L15); an abstraction around a ZeroMQ socket,
* gets sent over the wire to the kernel,
* a response is sent back and [converted to a `WireMessage`](https://gitlab.com/srwalker101/rust-jupyter-client/blob/b8c27ea2ca8b5a546fb59ed06c1d9a4e50575552/src/socket.rs#L58) ([conversion performed here](https://gitlab.com/srwalker101/rust-jupyter-client/blob/b8c27ea2ca8b5a546fb59ed06c1d9a4e50575552/src/wire.rs#L26)),
* which is then [turned back into a `Response` object](https://gitlab.com/srwalker101/rust-jupyter-client/blob/b8c27ea2ca8b5a546fb59ed06c1d9a4e50575552/src/wire.rs#L59) and generally passed back to the user.

I hope that this library will form the beginning of a new Rust-based Jupyter frontend. I have some ideas but that's for another time.

## Links

* [Project repository](https://gitlab.com/srwalker101/rust-jupyter-client)
* [docs.rs](https://docs.rs/jupyter-client/0.1.0/jupyter_client/)
* [crates.io](https://crates.io/crates/jupyter-client)
* [Description of the Jupyter wire protocol][wire-protocol]
* [Creating language kernels for IPython](http://andrew.gibiansky.com/blog/ipython/ipython-kernels/)


[wire-protocol]: https://jupyter-client.readthedocs.io/en/stable/messaging.html#the-wire-protocol

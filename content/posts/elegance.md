---
title: "Elegance"
date: 2020-04-27T10:30:16+01:00
draft: true
---

Elegance is a tough concept to define. Each person may think different things are elegant.

As an ex-physicist I consider elegance to mean when different phenomina can be treated in a similar way, typically operating within a particular parameter space. For example how General Relativity can be simplified down to Newton's laws when masses are (relatively) low.

Other examples are how some situations can follow the same laws as others. Again gravity is an inverse square law:

{{< katex display >}}
F = \frac{G m_1 m_2}{r^2}
{{< /katex >}}

where {{< katex >}}F{{< /katex >}} is force, {{< katex >}}G{{< /katex >}} is the universal gravitation constant, {{< katex >}}m_1{{< /katex >}} and {{< katex >}}m_2{{< /katex >}} are the masses of the two bodies involved, and {{< katex >}}r{{< /katex >}} is the distance between the two bodies.

Similarly electromagnetic radiation is an inverse square law:

{{<katex display>}}
I = \frac{P}{A} = \frac{P}{4 \pi r^2}
{{</katex>}}

where {{<katex>}}I{{</katex>}} is the intensity at a point and {{<katex>}}r{{</katex>}} is the distance between the point and the source.

Both of these have similar solutions, but they apply under different conditions (mass vs electromagnetic radiation). *Elegance*.

---

This situation comes up in software development as well. For example the way the Unicode specification simplifies down to the ASCII alphabet when considering only `a-zA-Z0-9` and special characters.

*Elegance*.

This is also the case for regular expressions. By default, `grep` uses regular expressions to search for text in a string. I thought this morning that I should write a version of `grep` that searches for literal strings[^1]. I realised that for simple `a-zA-Z` strings (which is usually what I'm searching for) that regular expressions _simplify_ down to literal strings.

*Elegance*.

[^1]: I know that this exists in `fgrep` and `grep -F` but I can never remember what it's called.

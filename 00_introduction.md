# Introduction

Estimated reading time: 5 minutes

## A love-hate relationship

I never studied functional programming (FP) at university and until few years ago it was just a
fancy word that appeared on some random blogs. It's only after a long while that I discovered the
Scala language. I was quickly captured by it and I wanted to know more and be fluent. So I hit
"functional programming" and I discovered it was not just a fancy word at all! As quick as my
interest grew also my frustration did because I found out that, for me at least, information is
very spread around (blogs, videos, books, libraries etc.) and that I could find only extremes: very
basic stuff or extremely complicated mathy contents "way beyond human comprehension".

What I couldn't find was a place that told me what I was supposed to learn, in what order and that
could explain concepts focusing on their intuition and usability rather than tasters or graduate
papers.

To put it short, this project is born from a personal struggle where I had to find my way to learn
functional programming. I thought it would be very useful to share the knowledge I built about
**learning** functional programming.

## Targets

What I want to achieve with this project is to put together **in a structured way** the information
that is already publicly available and to top it up with my personal comments, notes and ideas and
with examples and exercises to make the life of people that want to get into FP easier.

I want to focus on **functional programming applied to Scala** and Scala only, although there will
be multiple references to materials in Haskell.

I also want to dig deep and go as far as possible, including **advanced concepts** like references
to mathematics and category theory. I will connect to other branches of computer science and I will
talk about OOP too.

At the end I want the reader to be able to **understand** most of the material that can be found
online so that one can be independent in his or her own research.

Also, this project will focus on **giving concepts and information** that are useful to understand
libraries like Cats, scalaz and Shapeless that, if you've never done FP before, can be really
obscure and not understandable. Although, now, some of them have very good documentation focused on
usage.

To get to this point I want to do my best to **provide intuition** of the concepts and to connect
them to real world problems that they solve. The more you will progress with FP, the more the
concept will become abstract and the more it will become increasingly difficult to keep in touch
with "the real world". A very common question I asked myself several times is "what do I even need
this for?". I want this kind of question to be fully answered and to provide sound foundations of
concepts and tools.

I want to use **a step-by-step approach** where I guide the reader from no prior knowledge of FP to
get the grasp of the most advanced topics. I will introduce concepts little by little, providing
understanding and intuition behind each of them and also what real world problems they solve. I will
only assume that the reader is already well grounded in other programming paradigms.

On the other hand I will assume that you area already a developer and know how to do your job, that
you aware and apply good coding principle and that you strive for high quality. I will assume you
are familiar with Scala and that the syntax is not new to you but I will explain the syntax and
mechanics of what is needed for functional programming.

Overall, what I want the reader to achieve is:

* to be familiar with most of the FP concepts and know their purpose, from the simplest to the more
  advanced ones;
* understand and be able to use functional design patterns;
* become autonomous and be able to use online material;
* write purely functional code in Scala;
* be able to use the most common FP libraries.

## Why Scala

Why Scala.... For a few reasons.

I like the JVM, it's a solid and tested platform that's been around for several years, I worked with
it in the past years and I ended up appreciating its features, it has great adoption on the market
and it will keep this position for many years to come thanks to Java and its ecosystem.

Scala and Java can exchange libraries and this is a big kickstart for anyone coming from Java that
want to build applications in Scala because you can start right off with what you already know.

I like the community around it, there is a great bunch of people that are really good and motivated
and produce a lot of great material and provide great support.

I mostly love its syntax, its terseness and expressive power, I like the language features. Scala is
also implementing advanced and modern FP techniques and type level programming features that make it
very expressive, powerful and super fun to use and explore.

However, with Scala, I found it really really annoying to learn FP because the vast majority of the
content found around is for Haskell. Haskell is *the* authority on FP and for good reasons, I'm not
denying that. But I see no reason why I should learn one language to learn another language. It
would be a bit like going from London to Edinburgh via Paris. Sure you can do it but... I don't want
to learn Haskell! I want to learn Scala! I have to say this is changing in the past years because
the community is realizing that Scala is a world on its own and we're gradually moving away from
Haskell and more quality material is present on the internet (I hope this guide to be one).

So this is also to have good, complete materials in Scala.

With time I also hope that more languages will be added by other people. Languages like Haskell
itself or F# too, why not!

## Running the code examples

Except otherwise stated I tried to make all code snippets working on Ammonite, a Scala REPL, so that
you can just copy-paste the code and see the result. Alternatively you can save the snippets in a
file `*.sc` and run it with Ammonite:

```shell
amm <file>.sc
```

You can download Ammonite at <http://ammonite.io/>

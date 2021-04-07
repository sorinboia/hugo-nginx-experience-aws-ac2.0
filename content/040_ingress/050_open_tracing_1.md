+++
title = "What is open tracing"
date = 2020-07-08T14:37:59+03:00
weight = 40
+++

So, what is OpenTracing? It’s a vendor-agnostic API to help developers easily instrument tracing into their code base. It’s open because no one company owns it. In fact, many tracing tooling companies are getting behind OpenTracing as a standardized way to instrument distributed tracing.

OpenTracing wants to form a common language around what a trace is and how to instrument them in our applications. In OpenTracing, a trace is a directed acyclic graph of Spans with References that may look like this :
```
[Span A]  ←←←(the root span)
            |
     +------+------+
     |             |
 [Span B]      [Span C] ←←←(Span C is a `ChildOf` Span A)
     |             |
 [Span D]      +---+-------+
               |           |
           [Span E]    [Span F] >>> [Span G] >>> [Span H]
                                       ↑
                                       ↑
                                       ↑
                         (Span G `FollowsFrom` Span F)
```
This allows us to model how our application calls out to other applications, internal functions, asynchronous jobs, etc. All of these can be modeled as Spans, as we’ll see below.

For example, if I have a consumer website where a customer places orders, I make a call to my payment system and my inventory system before asynchronously acknowledging the order. I can trace the entire order process through every system with an OpenTracing library and can render it like this:
<pre>
––|–––––––|–––––––|–––––––|–––––––|–––––––|–––––––|–––––––|–> time
 [Place Order···················································]
   [Receive Payment·····] [Fulfill Order··] [Email Order...]
</pre>

Open Tracing tracing is becoming more and more important because software systems are becoming more and more distributed and complex. We need ways to correlate them so that we can understand what is happening inside them. When we know what’s happening inside them, we can quickly hunt down defects and other incidents. Good distributed tracing tooling can save you hours or days of frustration.
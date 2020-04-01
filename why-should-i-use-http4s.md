# Why should I use http4s

[Original source](https://www.reddit.com/r/scala/comments/5f5049/http4s_vs_finch_vs_akkahttp_vs/)

> I've heard good things about http4s in the past. But at the same time, I can't find anything noticably larger than "hello world" or "todo app" built with it.

Yeah, unfortunately, I don't think we (Verizon Labs) have open-sourced anything we've built with http4s yet. But rest assured, we're building a lot with http4s. (Insert joke about "we liked it so much we bought the developer" here, but now I'm dating myself by joking about a commercial from my childhood, not to mention insulting Ross Baker.)

> I also have a very hard time with the documentation. It's like 3 pages of "getting started", and then the rest you have to trial and error and read the source code to even know what the library has to offer.

The docs are definitely incomplete. Beyond that, there's good news and bad news.

Bad news: I don't think we're giving Ross any more bandwidth to focus on the docs than he has anytime soon.

Good news: http4s is very well architected, very principled, and very easy to understand, if you're familiar with the scalaz(-stream) ecosystem.

If you're not yet familar with the scalaz(-stream) ecosystem, I recommend starting with the links given at the beginning of the http4s docs, and would add _Daniel Spiewak_'s [Introduction to scalaz-stream](https://gist.github.com/djspiewak/d93a9c4983f63721c41c) and maybe dig into some of the [other resources](https://github.com/functional-streams-for-scala/fs2/wiki/Additional-Resources) as well.

Finally, I'd look at [this example](https://bitbucket.org/da_terry/scalasyd-doobie-http4s/src) of a microservice using _http4s_, _Doobie_, and _Argonaut_, and the [http4s examples](https://github.com/http4s/http4s/tree/v0.14.11/examples) generally. _http4s_ really is simple in that there are very few concepts to master (and most of them, like pattern matching, you already know—what needs more explanation is how they fit together).

> Finch on the other hand has Twitter behind it(?), so there's at least one user of it :p

Speaking for myself, the Twitter ecosystem makes me nervous, I think because it's sufficiently old that in many cases they wrote alternatives to standard library types that have the same name as the standard library type (e.g. `Promise`) but a different API, making reading code without imports (like the _Finch_ question elsewhere here) difficult, and at least to me, without clear motivation (I know why `scalaz.concurrent.Task` is preferable to `scala.concurrent.Future`; I don't know why `com.twitter.util.Promise` is preferable to `scala.concurrent.Promise`).

> I don't really care if it's 100% super purely functional - libraries that are fanatic about that tends to end up way harder to reason about than libraries where the side-effects are obvious (- if I hear the word "monad transformers" one more time I'm gonna puke cry).

In my experience, this is a matter of experience. :-)

Joking aside, I really have found that, when it seems like I'm missing some information about how some pure-FP thingy works from the docs, it means I already have the building blocks. I just haven't yet formed an intuition about how to combine those building blocks. I also start looking out for ways I'm making things more complicated than they need to be. I think you'll see what I mean if you look at the ScalaSyd http4s example I linked above. It reads like a Ruby or Python example. You won't find the word "monad" anywhere, let alone "monad transformer." But it is entirely referentially transparent. (By the way, there's no way for side-effects to be more obvious than for them to be declared in your types!)

> I guess I'm more pragmatic when it comes to functionalness. All that matters is that it's nice to use and scales as your codebase increases.

That's exactly why I insist on pure FP. No one's been able to explain to me why writing a microservice needs to be any harder than [this](https://bitbucket.org/da_terry/scalasyd-doobie-http4s/src/091460e162cc788cd6e6e33797b9d3aa32b274f4/src/main/scala/scalasyd/Web.scala?at=master&fileviewer=file-view-default). Note that even this simple example is composing an `HttpService` out of two smaller `HttpService`s, constructing `Response`s from a _scalaz-stream_ `Process` and a `scalaz.concurrent.Task`, and relying on a handful of very simple (one-line) JSON codecs to read and write JSON.

So I suggest sticking with http4s even though the docs are incomplete, because the gaps are very easy to fill. On that note, please don't hesitate to ask any questions you may have!

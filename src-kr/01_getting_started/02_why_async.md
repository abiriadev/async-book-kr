# 왜 비동기가 필요할까요?

Rust는 개발자가 더 빠르고 안전한 코드를 작성할 수 있도록 도와주며, 바로 이 특징 때문에 우리는 러스트를 좋아합니다. 하지만 비동기 프로그래밍이 이것과 어떤 관련이 있을까요?

비동기 프로그래밍, 또는 async는 _동시성 프로그래밍 모델_ 의 일종이며, 점점 더 많은 프로그래밍 언어들이 비동기 프로그래밍을 지원해 나가고 있습니다. 무엇보다 비동기 프로그래밍 모델은 동시에 진행되어야 하는 수많은 태스크들을 적은 OS스레드 위에서 동시에 실행할 수 있도록 해주며, 동시에 `async/await` 문법을 이용해 친숙한 동기 코드와 비슷한 모습을 유지합니다.

## 비동기 vs 기타 동시성 모델

동시성 프로그래밍은 현재 널리 사용되는 연속적 프로그래밍 기법에 비해 아직 많이 미성숙하며, 덜 "표준화"되어 있는 상태입니다. As a result, we express concurrency
differently depending on which concurrent programming model
the language is supporting.
A brief overview of the most popular concurrency models can help
you understand how asynchronous programming fits within the broader
field of concurrent programming:

- **OS threads** don't require any changes to the programming model,
  which makes it very easy to express concurrency. However, synchronizing
  between threads can be difficult, and the performance overhead is large.
  Thread pools can mitigate some of these costs, but not enough to support
  massive IO-bound workloads.
- **Event-driven programming**, in conjunction with _callbacks_, can be very
  performant, but tends to result in a verbose, "non-linear" control flow.
  Data flow and error propagation is often hard to follow.
- **Coroutines**, like threads, don't require changes to the programming model,
  which makes them easy to use. Like async, they can also support a large
  number of tasks. However, they abstract away low-level details that
  are important for systems programming and custom runtime implementors.
- **The actor model** divides all concurrent computation into units called
  actors, which communicate through fallible message passing, much like
  in distributed systems. The actor model can be efficiently implemented, but it leaves
  many practical issues unanswered, such as flow control and retry logic.

In summary, asynchronous programming allows highly performant implementations
that are suitable for low-level languages like Rust, while providing
most of the ergonomic benefits of threads and coroutines.

## Async in Rust vs other languages

Although asynchronous programming is supported in many languages, some
details vary across implementations. Rust's implementation of async
differs from most languages in a few ways:

- **Futures are inert** in Rust and make progress only when polled. Dropping a
  future stops it from making further progress.
- **Async is zero-cost** in Rust, which means that you only pay for what you use.
  Specifically, you can use async without heap allocations and dynamic dispatch,
  which is great for performance!
  This also lets you use async in constrained environments, such as embedded systems.
- **No built-in runtime** is provided by Rust. Instead, runtimes are provided by
  community maintained crates.
- **Both single- and multithreaded** runtimes are available in Rust, which have
  different strengths and weaknesses.

## Async vs threads in Rust

The primary alternative to async in Rust is using OS threads, either
directly through [`std::thread`](https://doc.rust-lang.org/std/thread/)
or indirectly through a thread pool.
Migrating from threads to async or vice versa
typically requires major refactoring work, both in terms of implementation and
(if you are building a library) any exposed public interfaces. As such,
picking the model that suits your needs early can save a lot of development time.

**OS threads** are suitable for a small number of tasks, since threads come with
CPU and memory overhead. Spawning and switching between threads
is quite expensive as even idle threads consume system resources.
A thread pool library can help mitigate some of these costs, but not all.
However, threads let you reuse existing synchronous code without significant
code changes—no particular programming model is required.
In some operating systems, you can also change the priority of a thread,
which is useful for drivers and other latency sensitive applications.

**Async** provides significantly reduced CPU and memory
overhead, especially for workloads with a
large amount of IO-bound tasks, such as servers and databases.
All else equal, you can have orders of magnitude more tasks than OS threads,
because an async runtime uses a small amount of (expensive) threads to handle
a large amount of (cheap) tasks.
However, async Rust results in larger binary blobs due to the state
machines generated from async functions and since each executable
bundles an async runtime.

On a last note, asynchronous programming is not _better_ than threads,
but different.
If you don't need async for performance reasons, threads can often be
the simpler alternative.

### Example: Concurrent downloading

In this example our goal is to download two web pages concurrently.
In a typical threaded application we need to spawn threads
to achieve concurrency:

```rust,ignore
{{#include ../../examples/01_02_why_async/src/lib.rs:get_two_sites}}
```

However, downloading a web page is a small task; creating a thread
for such a small amount of work is quite wasteful. For a larger application, it
can easily become a bottleneck. In async Rust, we can run these tasks
concurrently without extra threads:

```rust,ignore
{{#include ../../examples/01_02_why_async/src/lib.rs:get_two_sites_async}}
```

Here, no extra threads are created. Additionally, all function calls are statically
dispatched, and there are no heap allocations!
However, we need to write the code to be asynchronous in the first place,
which this book will help you achieve.

## Custom concurrency models in Rust

On a last note, Rust doesn't force you to choose between threads and async.
You can use both models within the same application, which can be
useful when you have mixed threaded and async dependencies.
In fact, you can even use a different concurrency model altogether,
such as event-driven programming, as long as you find a library that
implements it.

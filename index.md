# Torqlang

## A fantastically simple approach to concurrent programming.

### Why?

Concurrent programming is notoriously hard. Mainstream programming languages require explicit control structures, resulting in unnatural, complex and tangled programs.

### How?

Torqlang solves this problem with a patent-pending programming model, named Actorflow, that fuses message-passing concurrency with declarative dataflow, giving us a naturally sequential style that is impossible to obtain in mainstream languages.

### Where?

Torqlang is a "source available" project hosted at GitHub.

Website: [Torqlang Home Page](http://torqlang.github.io)

License: [Torqlang License v1.0](http://torqlang.github.io/licensing/torqlang-license-v1_0)

Repository: [Torqlang at GitHub](https://github.com/torqlang)

Book: [Torqlang at Leanpub](https://leanpub.com/torqlang)

## The Book (sample)

### Preface

Like many of you, I programmed sequentially for years with an occasional thread, enjoying the performance gains as processors evolved faster and faster. Then, processors began evolving to be more and more parallel. I became painfully aware that my programming style was not the future. Concurrent programming with shared state had been declared a failure (Lee, 2006), and I needed to learn a new programming model.

A colleague introduced me to an early version of Scala that contained actors. Programming in Scala opened my eyes to a new world of actors, monads, and multi-paradigm programming. I worked hard and became confident in this new environment, but the applications that I produced were confusing. Instead of forming a business solution, my programs formed a concurrency solution. Intellectually, I liked the power and sophistication of this new environment, but the results felt wrong. Writing application code, especially for business applications, should be easy, and the results should express the solution clearly without requiring special knowledge.

While battling the concurrent programming experience, other forces affected my opinions of concurrent programming, such as cloud computing, low-code development, and software composition using web services. Along the way, I studied "Concepts, Techniques, and Models in Computer Programming," paying particular attention to a programming model called declarative dataflow that greatly simplified concurrent programming. Declarative dataflow introduced a new construct called the dataflow variable, making concurrent programming extremely simple with a naturally sequential style. I started imagining a new programming experience that could solve these problems foremost in my mind, problems exacerbated by mainstream programming techniques.

In my first experiment with declarative dataflow, I created a dynamic programming language named Ozy that ran as a companion language on the JVM. Ozy was designed as a web services container to run over other Java services. Ultimately, Ozy was not the solution I hoped for. Its programming model required Java services to understand the dataflow variable, making it very difficult to integrate Ozy with existing Java services. To a lesser extent, but nevertheless an obstacle, programmers found the Mozart-Oz syntax off-putting.

In 2020, I found myself locked down because of COVID-19, staring at a blank screen with an epiphany for a new programming experience. The central idea was an actor construct fusing the message-passing protocol with a hidden implementation of declarative dataflow. Unlike typical actor implementations, programs would not be formed as state machines, and unlike declarative dataflow, programs would interoperate with Java services without knowing dataflow variables. The name Torqlang came to me as I imagined a programming language with the power to service thousands of requests incrementally and fairly, moving massive amounts of data without stalling.

Torqlang is the culmination of a personal journey to realize a new programming experience. It is comprised of a language and a patent-pending model named Actorflow. The language is dynamic with optional type annotations, and although currently interpreted, Torqlang can work faster than compiled applications by utilizing native services and multiple processors efficiently. Actorflow gives Torqlang abstraction powers, where actor messages are the inputs and outputs of a hidden dataflow machine.

# Introduction

Mainstream programming languages suffer an incurable problem: shared variables alone cannot refer to future memory values from multiple threads--programs must synchronize access to shared memory. As formalized by [Lee (2006)](#lee-e-a-2006), programming imperatively with threads is a failure. In response, the industry has rallied around message passing and functional programming.

Example languages and frameworks based on message passing or functional programming:

* Erlang -- message passing actors
* Haskel -- functional programming
* Akka -- message passing actors and functional programming
* Go -- message passing channels
* Rust -- message passing channels and shared state
* Promises -- lightweight functional programming
* Async-Await -- syntactic sugar for Promises

These languages and frameworks provide safe alternatives to programming with threads, but unfortunately, they create complex and tangled code (organized as a concurrency solution and not an application solution). Generally, functional programming gets tangled around callbacks and promises, and message passing gets tangled around states and state transitions.

* Erlang -- `receive` syntax partitions code into states and state transitions
* Haskel -- mathematically pure functions organize code as chains of function calls
* Akka -- partial functions and behaviors partition code into states and state transitions
* Go -- channels partition code into states and state transitions
* Rust -- channels partition code into states and state transitions
* Promises -- callbacks partition code into event handlers
* Async-Await -- creates non-blocking sequential code and not concurrent code

> Rust also supports shared state concurrency, but with a twist. Rust imposes an ownership model over shared state concurrency. The Rust ownership model is not easy, but it is safe ([Crichton et al., 2023](#crichton-w-gray-g-krishnamurthi-s-2023)). Rust concurrency organizes code around ownership enforced implicitly by the compiler instead of using explicit language constructs.

Citations:
* Behaviors as finite state machines. "The events the FSM can receive become the type of message the Actor can receive." ([Akka documentation](https://doc.akka.io/docs/akka/current/typed/fsm.html))
* The essence of functional programming. "Shall I be pure or impure?" ([Wadler 1992](#wadler-p-1992-february), [pdf](https://dl.acm.org/doi/pdf/10.1145/143165.143169))
* Continuations and Aspects to Tame Callback Hell on the Web ([Leger et al., 2021](#leger-p-fukuda-h-figueroa-i-2021))

In contrast, consider the following example written in Torqlang. The actor `SimpleMathActor` calculates the number `7` concurrently using three child actors to supply the operands in the expression `1 + 2 * 3`. This example is an unsafe race condition in mainstream languages. However, in Torqlang, this naturally sequential code will always calculate `7`. Notice that Torqlang honors operator precedence without explicit synchronization.

> An actor is delimited using `actor <<configurator>>() in <<statements>> end` and its protocol consists of `ask` and `tell` handlers.

~~~
actor SimpleMathActor() in
    actor Number(n) in
        ask 'get' in
            n
        end
    end
    var one = spawn(Number.cfg(1)),
        two = spawn(Number.cfg(2)),
        three = spawn(Number.cfg(3))
    ask 'calculate' in
        one.ask('get') + two.ask('get') * three.ask('get')
    end
end
~~~

Next, consider `NestedMathActs` that employs four child actors, two levels deep, to perform its calculation concurrently. This example uses syntactic sugar `act <<statements>> end` to define single-performance actors. This naturally sequential code will always calculate `35`.

~~~
actor NestedMathActs() in
    ask 'calculate' in
        var a, b, c, d
        a = act b + c + act d + 11 end end
        c = act b + d end
        d = act 5 end
        b = 7
        a // Will always be 35
    end
end
~~~

What about streaming data? `SumOddIntsStream` uses `IntPublisher`  to sum a sequence of odd numbers using a reactive stream. In Torqlang, reactive streams with back pressure are programmed entirely as a single-request multiple-response interaction, but the result is an asynchronous, non-blocking, multi-threaded program. In contrast, reactive streams in Java 9+ are best implemented by service providers and consumed by application programmers ([Kunicki, 2019](#kunicki_jacek_2019)).

*Publisher*

~~~
actor IntPublisher(first, last) in
    import system[ArrayList, Cell, respond]
    var next_int = Cell.new(first)
    ask 'request'#{'count': n} in
        func calculate_to() in
            var to = @next_int + (n - 1)
            if to < last then to else last end
        end
        var response = ArrayList.new()
        var to = calculate_to()
        while @next_int <= to do
            response.add(@next_int)
            next_int := @next_int + 1
        end
        respond(response.to_tuple())
        if @next_int <= last then
            eof#{'more': true}
        else
            eof#{'more': false}
        end
    end
end
~~~

*Subscriber*

~~~
actor SumOddIntsStream() in
    import system[Cell, Iter, Stream]
    import examples.IntPublisher
    ask 'sum'#{'first': first, 'last': last} in
        var sum = Cell.new(0)
        var int_publisher = spawn(IntPublisher.cfg(first, last))
        var int_stream = Stream.new(int_publisher, 'request'#{'count': 3})
        for i in Iter.new(int_stream) do
            if i % 2 != 0 then sum := @sum + i end
        end
        @sum
    end
end
~~~

# References

Crichton, W., Gray, G., & Krishnamurthi, S. (2023). A Grounded Conceptual Model for Ownership Types in Rust. arXiv preprint arXiv:2309.04134.

Kunicki, Jacek (2019). How (Not) to use reactive Streams in Java 9+. https://www.youtube.com/watch?v=zf_0ydYb3JQ.

Lee, E. A. (2006). The problem with threads. Computer, 39(5), 33-42.

Leger, P., Fukuda, H., & Figueroa, I. (2021). Continuations and Aspects to Tame Callback Hell on the Web. Journal of Universal Computer Science, 27(9), 955-978.

Wadler, P. (1992, February). The essence of functional programming. In Proceedings of the 19th ACM SIGPLAN-SIGACT symposium on Principles of programming languages (pp. 1-14).
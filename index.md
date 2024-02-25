# Torqlang

A fantastically simple approach to concurrent programming.

## Why? 

Mainstream programming languages require explicit control structures, resulting in unnatural, complex, and tangled programs.

Torqlang solves this problem with a patent-pending programming model named *Actorflow*, which fuses message-passing concurrency with declarative dataflow, giving us a naturally sequential style without unnecessary complexity.

Torqlang is a "source available" project hosted at GitHub.

Website: [Torqlang (this page)](http://torqlang.github.io)

License: [Torqlang License v1.0](http://torqlang.github.io/licensing/torqlang-license-v1_0)

Repository: [Torqlang at GitHub](https://github.com/torqlang)

## The Book

Book: [Torqlang at Leanpub](https://leanpub.com/torqlang)

### Preface

Like many of you, I programmed sequentially for years with an occasional thread, enjoying the performance gains as processors evolved faster and faster. Then, processors began evolving to be more and more parallel. I became painfully aware that my programming style was not the future. Concurrent programming with shared state had been declared a failure[^threads_failure], and I needed to learn a new programming model.

[^threads_failure]: [The Problem With Threads](<https://www2.eecs.berkeley.edu/Pubs/TechRpts/2006/EECS-2006-1.html>)

A colleague introduced me to an early version of Scala that contained actors. Programming in Scala opened my eyes to a new world of actors, monads, and multi-paradigm programming. I worked hard and became confident in this new environment, but the applications that I produced were confusing. Instead of forming a business solution, my programs formed a concurrency solution. Intellectually, I liked the power and sophistication of this new environment, but the results felt wrong. Writing application code, especially for business applications, should be easy, and the results should express the solution clearly without requiring special knowledge.

While battling the concurrent programming experience, other forces affected my opinions of concurrent programming, such as cloud computing, low-code development, and software composition using web services. Along the way, I studied "Concepts, Techniques, and Models in Computer Programming," paying particular attention to a programming model called declarative dataflow that greatly simplified concurrent programming. Declarative dataflow introduced a new construct called the dataflow variable, making concurrent programming extremely simple with a naturally sequential style. I started imagining a new programming experience that could solve these problems foremost in my mind, problems exacerbated by mainstream programming techniques.

In my first experiment with declarative dataflow, I created a dynamic programming language named Ozy that ran as a companion language on the JVM. Ozy was designed as a web services container to run over other Java services. Ultimately, Ozy was not the solution I hoped for. Its programming model required Java services to understand the dataflow variable, making it very difficult to integrate Ozy with existing Java services. To a lesser extent, but nevertheless an obstacle, programmers found the Mozart-Oz syntax off-putting.

In 2020, I found myself locked down because of COVID-19, staring at a blank screen with an epiphany for a new programming experience. The central idea was an actor construct fusing the message-passing protocol with a hidden implementation of declarative dataflow. Unlike typical actor implementations, programs would not be formed as state machines, and unlike declarative dataflow, programs would interoperate with Java services without knowing dataflow variables. The name Torqlang came to me as I imagined a programming language with the power to service thousands of requests incrementally and fairly, moving massive amounts of data without stalling.

Torqlang is the culmination of a personal journey to realize a new programming experience. It is comprised of a language and a patent-pending model named Actorflow. The language is dynamic with optional type annotations, and although currently interpreted, Torqlang can work faster than compiled applications by utilizing native services and multiple processors efficiently. Actorflow gives Torqlang abstraction powers, where actor messages are the inputs and outputs of a hidden dataflow machine.
# Torqlang

A fantastically simple approach to concurrent programming.

## Why? 

Mainstream programming languages require explicit control structures, resulting in unnatural, complex, and tangled programs.

Torqlang solves this problem with a patent-pending programming model named *Actorflow*, which fuses message-passing concurrency with declarative dataflow, giving us a naturally sequential style void of unnecessary complexity.

Torqlang is a "source available" project hosted at GitHub.

Website: [Torqlang (this page)](http://torqlang.github.io)

License: [Torqlang License v1.0](http://torqlang.github.io/licensing/torqlang-license-v1_0)

Repository: [Torqlang at GitHub](https://github.com/torqlang)

## The Book

Book: [Torqlang at Leanpub](https://leanpub.com/torqlang)

### Preface

After many years of programming, I was painfully aware that my programming style was not the future. Computers were being manufactured with more and more cores to run faster, and concurrent programming with shared state had been declared a failure.

A colleague introduced me to an early version of Scala that contained actors. Programming in Scala opened my eyes to a new world of monads, actors, and multi-paradigm programming. I worked hard and became confident in this new environment, but the applications that I produced were confusing. Instead of forming a business solution, my programs formed a concurrency solution. Intellectually, I liked the power and sophistication of this new environment, but the results felt wrong. Writing application code, especially for business applications, should be easy, and the results should express the solution clearly without requiring special knowledge.

While battling the concurrent programming experience, other forces affected my opinions of concurrent programming, such as cloud computing, low-code development, and software composition using web services. Along the way, I read "Concepts, Techniques, and Models in Computer Programming," paying particular attention to a programming model called *declarative dataflow* that greatly simplified concurrent programming. Declarative dataflow introduced a new construct called the *dataflow variable*, making concurrent programming extremely simple with a naturally sequential style. I started imagining a new programming experience that could solve these problems foremost in my mind, problems exacerbated by concurrent programming techniques.

In my first experiment with declarative dataflow, I created a dynamic programming language named Ozy that ran as a companion language on the JVM. Ozy was designed as a web services container to run over other Java services. Ultimately, Ozy fell short. Its programming model required Java services to understand the dataflow variable, making it practically unusable to compose over existing Java services. To a lesser extent, but nevertheless an obstacle, programmers found the Mozart-Oz syntax off-putting.

In 2020, I found myself locked down because of COVID-19, staring at a blank screen with an epiphany for a new programming experience. The central idea was an actor construct fusing the message-passing protocol with a hidden implementation of declarative dataflow. Unlike typical actor implementations, programs would not be structured to look like state machines, and unlike declarative dataflow, programs would interoperate with Java services without understanding dataflow variables.

Torqlang is the culmination of a personal journey to realize a new programming experience comprised of a language and a patent-pending model named *Actorflow*. The programming language is dynamic with optional type annotations, and although interpreted, Torqlang can run faster than compiled applications by utilizing native services and multiple processors efficiently. Torqlang gets its abstraction powers from Actorflow, a novel approach where actor messages are the inputs and outputs of a hidden dataflow machine.

According to the separation of language and model, this book is presented in two parts: the Programming Guide, which walks the user through installing and running various programming examples, and the Programming Model, which explains the underlying concepts derived from prior work in computer science.

Incidentally, the name Torqlang came to me as I imagined a programming language with the power to service thousands of requests incrementally and fairly, moving massive amounts of data without stalling.

Async API for Scala.js
================================
[async](https://www.npmjs.com/package/async) - Higher-order functions and common patterns for asynchronous code.

### Description

Async is a utility module which provides straight-forward, powerful functions for working with asynchronous JavaScript. 
Although originally designed for use with Node.js and installable via `npm install --save async`, it can also be used 
directly in the browser.

### Build Dependencies

* [SBT v0.13.15](http://www.scala-sbt.org/download.html)

### Build/publish the SDK locally

```bash
 $ sbt clean publish-local
```

### Running the tests

Before running the tests the first time, you must ensure the npm packages are installed:

```bash
$ npm install
```

Then you can run the tests:

```bash
$ sbt test
```

### Examples

Using `Async` asynchronously via callbacks

```scala
import io.scalajs.npm.async.Async
import scala.scalajs.js
import scala.scalajs.js.annotation.ScalaJSDefined

// create a queue object with concurrency 2
val q = Async.queue[Task]((task: Task, callback: js.Function0[Any]) => {
    println("hello " + task.name)
    callback()
}, 2)

// assign a callback
q.drain = () => println("all items have been processed")

// add some items to the queue
q.push(new Task(name = "foo"), (err: Error) => println("finished processing foo"))

q.push(new Task(name = "bar"), (err: Error) => println("finished processing bar"))

// add some items to the queue (batch-wise)
q.push(js.Array(new Task(name = "baz"), new Task(name = "bay"), new Task(name = "bax")), (err: Error) => {
  println("finished processing item")
})

// add some items to the front of the queue
q.unshift(new Task(name = "bar"), (err: Error) => println("finished processing bar"))

@ScalaJSDefined
class Task(val name: String) extends js.Object
```

### Artifacts and Resolvers

To add the `Async` binding to your project, add the following to your build.sbt:  

```sbt
libraryDependencies += "io.scalajs.npm" %%% "async" % "0.4.0"
```

Optionally, you may add the Sonatype Repository resolver:

```sbt   
resolvers += Resolver.sonatypeRepo("releases") 
```
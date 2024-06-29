<div align='center'>
  <h1>Node.js</h1>
</div>

# Table of Contents

- What is Node.js
- Why Node.js
- Summary

# What is Node.js

NodeJs (and also Deno) is a [cross-platform, open-source JavaScript runtime environment](https://en.wikipedia.org/wiki/Node.js) (not a framework or programming language) that enables `asynchronous operations` to be executed outside the browser. Node.js is a C++ program that uses Google's V8 JavaScript engine to run JavaScript code outside the browser. It is asynchronous by default and single-threaded, i.e., the event loop runs on a single thread. Therefore, it is designed to handle data I/O-intensive and real-time operations more efficiently than CPU-intensive operations such as video processing and image editing. To enable CPU-intensive performance (multi-threading), the [worker_threads](https://nodejs.org/api/worker_threads.html), [child_process](https://nodejs.org/docs/latest/api/child_process.html), and [cluster](https://nodejs.org/docs/latest/api/cluster.html) modules can be used.

Note 1: asynchronous is not concurrent or multi-threading, it is non-blocking.

Note 2: multi-threading in low-level languages such as C++, Java, or Rust is faster than using subprocess modules in NodeJs. 

# Why Node.js

To run JavaScript code outside the browser (e.g. on a mobile device) on the server/backend side, or to use asynchronous operations in JavaScript, one needs a JavaScript runtime environment.

# Summary

- Pros: can be used for both Backend and Frontend, allows fast development, optimal for I/O intensive and real-time operations.
- Cons: single-threaded, not optimal for CPU-intensive operations.

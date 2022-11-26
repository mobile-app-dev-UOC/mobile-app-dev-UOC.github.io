---
layout: default
title: 10.1. Coroutines
parent: 10. Concurrency
nav_order: 1
---

# 10.1. Coroutines 

A **coroutine** is executed by a thread belonging to a given **Dispatcher**. A Dispatcher can be viewed as a pool of threads.

Coroutines and Dispatchers appear primarily to solve two problems:

1. Threading used to be abused for tasks such as uploading images from a remote server. If we loaded 1000 images, the developers would create 1000 threads. This caused an overload at the level of the operating system, both in terms of network and CPU usage. The Dispatchers solve that problem because the number of threads in each pool is controlled by the Kotlin coroutine library.
When the number of pool threads is exceeded, Kotlin locks the request until a thread from the pool becomes available.

2. Setting up a thread according to the intended usage is not easy. Different types of threads (complex computations, local input/output operations, network usage) require a different configuration. The Dispatchers perform the configuration of these threads internally in the Kotlin coroutine library.



Thus, we have three types of Dispatchers:
- **Default:** Common pool of Threads used for calculations. If there are multiple cores in the mobile device, it may be possible to distribute these threads among the available cores.
- **I/O:** This is the pool intended for input/output operations. Kotlin knows that the threads in this pool will spend a long time waiting for data and sets them up accordingly.
- **Main:** This is a pool with a single thread that is the main thread running the interface. It is useful for running code from a coroutine on the main thread.


The simplest example of coroutine can be two coroutines running in parallel to write a message to the Log.

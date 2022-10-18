---
date: 2021-07-19
tags:
  - programming
  - threads
  - thread_pool
  - c++
---

# Thread Pool Waiting

Submitting work to a #threadpool and waiting for the submitted tasks to complete:

```cpp
struct ThreadData
{
    std::mutex mut;
    std::condition_variable cv;
    std::atomic<int> counter = 0;
};
ThreadData msg;

for(int i =0; i < 10; i++) {  // Submit some work
msg.counter++;
threadPool.enqueue([&msg](){
   // < do work here >

   // Notify finished
   std::lock_guard<std::mutex> lk(msg.mut);
   msg.counter--;
   msg.cv.notify_all();
});
}

// Wait for work to finish
std::unique_lock<std::mutex> lock(msg.mut);
msg.cv.wait(lock, [&]() { return msg.counter == 0; }); 
```

<div class="ui section divider"></div>
<section id="socialMediaLinks"></section>
<div class="ui section divider"></div>
<div id="disqus_thread"></div>

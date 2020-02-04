---
title: microtask_and_task
author: bro wen
date: 2019-03-14 12:07:35
tags: [Job, JavaScript]
---
# Tasks, microtasks, queues and schedules

[ecma](https://tc39.github.io/ecma262/#sec-jobs-and-job-queues)
8.4Jobs and Job Queues

A Job is an abstract operation that initiates an ECMAScript computation when no other ECMAScript computation is currently in progress. A Job abstract operation may be defined to accept an arbitrary set of job parameters.

Execution of a Job can be initiated only when there is no running execution context and the execution context stack is empty. A PendingJob is a request for the future execution of a Job. A PendingJob is an internal Record whose fields are specified in Table 24. Once execution of a Job is initiated, the Job always executes to completion. No other Job may be initiated until the currently running Job completes. However, the currently running Job or external events may cause the enqueuing of additional PendingJobs that may be initiated sometime after completion of the currently running Job.

{% youtube cCOL7MC4Pl0 %}
[Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
To understand this you need to know how the event loop handles tasks and microtasks. This can be a lot to get your head around the first time you encounter it. Deep breath…

Each 'thread' gets its own event loop, so each web worker gets its own, so it can execute independently, whereas all windows on the same origin share an event loop as they can synchronously communicate. The event loop runs continually, executing any tasks queued. An event loop has multiple task sources which guarantees execution order within that source (specs such as IndexedDB define their own), but the browser gets to pick which source to take a task from on each turn of the loop. This allows the browser to give preference to performance sensitive tasks such as user-input. Ok ok, stay with me…

Tasks are scheduled so the browser can get from its internals into JavaScript/DOM land and ensures these actions happen sequentially. Between tasks, the browser may render updates. Getting from a mouse click to an event callback requires scheduling a task, as does parsing HTML, and in the above example, setTimeout.

setTimeout waits for a given delay then schedules a new task for its callback. This is why setTimeout is logged after script end, as logging script end is part of the first task, and setTimeout is logged in a separate task. Right, we're almost through this, but I need you to stay strong for this next bit…

Microtasks are usually scheduled for things that should happen straight after the currently executing script, such as reacting to a batch of actions, or to make something async without taking the penalty of a whole new task. The microtask queue is processed after callbacks as long as no other JavaScript is mid-execution, and at the end of each task. Any additional microtasks queued during microtasks are added to the end of the queue and also processed. Microtasks include mutation observer callbacks, and as in the above example, promise callbacks.

Once a promise settles, or if it has already settled, it queues a microtask for its reactionary callbacks. This ensures promise callbacks are async even if the promise has already settled. So calling .then(yey, nay) against a settled promise immediately queues a microtask. This is why promise1 and promise2 are logged after script end, as the currently running script must finish before microtasks are handled. promise1 and promise2 are logged before setTimeout, as microtasks always happen before the next task.

So, step by step:
```js
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});
```
In summary:

    Tasks execute in order, and the browser may render between them
    Microtasks execute in order, and are executed:
        after every callback, as long as no other JavaScript is mid-execution
        at the end of each task

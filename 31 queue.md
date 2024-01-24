#                                           QUEUE AND TASKS 

Consider a scenerio, there may be any api you wants in your application that takes seconds to return a response or you have to send notifications to multiple users. 

In such cases if they will be executed syncronously then it will affect the user experience because of the blockage of application execution as server is busy in waiting for the response of that task.

What is the solution for that ?

Go asyncronously, Asyncronously simply means server do thier respective job( serving application ) and add this long task( response waiting | sending msg ) to the queue, now a worker comes and pick up a job from the queue and execute it. This worker is independent of server execution and will executes job parallely.


In laravel we can create this using Queue and Tasks.


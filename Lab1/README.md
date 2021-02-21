# 50005Lab1
## TODO 4.a: Implement main_loop()

### Worker Process Revival
To assign processes to workers, the buffer is first checked to find any available workers that can accept a new process.
In main_loop(), we busy wait and loop through each of the processes in children_processes and jobs in shmPTR_jobs_buffer to check their status.
The event that a free worker is found is stored by the variable empty. The status of the child process is stored in the variable alive.

If the job status is 0 (i.e. shmPTR_jobs_buffer[i].taskstatus == 0), and the child process is alive, the child can be assined a new process, and the main loop is exited. 

Otherwise, if the child is not alive (i.e. alive != 0), then the worker must be revived in order for it to accept any new processes. This is done using fork() and job_dispatch().




### Legally Terminate Active Worker Processes
In order to legally terminate all the workers that are still alive, a simple but effective approach was used. I started with traversing through each process and check whether their status is 0; ie: if they are free and not taken up by any job. If they are, then 'shmPTR_jobs_buffer[i].task_type = 'z'(I assigned z to it), to make sure that the specific worker process was legally terminated. A counter variable was used, initialized to 0 at start and incremements after killing each process, it returns true and breaks the loop if the counter equals the number of all processes, ensuring that it has traversed through all processes alive.
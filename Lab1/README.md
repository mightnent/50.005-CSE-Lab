# 50005Lab1
## TODO 4.a: Implement main_loop()

### Worker Process Revival
To assign processes to workers, the buffer is first checked to find any available workers that can accept a new process.
In main_loop(), we busy wait and loop through each of the processes in children_processes and jobs in shmPTR_jobs_buffer to check their status.
The event that a free worker is found is stored by the variable empty. The status of the child process is stored in the variable alive.

If the job status is 0 `shmPTR_jobs_buffer[i].taskstatus == 0`, and the child process is alive, the child can be assined a new process, and the main loop is exited. 

Otherwise, if the child is not alive (i.e. alive != 0), then the worker must be revived in order for it to accept any new processes. This is done using fork() and job_dispatch().




### Legally Terminate Active Worker Processes
To legally terminate all active worker processes, we iterate through all the child processes in `children_processes` and check its `task_status` and child process status (stored in the variable `alive`). The variable `death_count` counts the number of children processes that have successfully terminated. 

In the termination loop, if the child process is dead, increase `death_count` by one and continue iterating through the list of children. If the child process is alive and still running a process, continue looping and wait for the child to finish it's task and check it's status in the next loop cycle. If the child process is alive and has no active tasks, assign it a z task and change its status.

The loop exits when all the processes are dead ( `death_count == number_of_processes`).
# Node

## Debug a running process

1. `kill -SIGUSR1 <NODE_PID>` (put in debug mode)
2. `node inspect -p <NODE_PID>`
3. `profile` + `profileEnd` (after a min or two)
4. `profiles[0].save(filepath = 'node.cpuprofile')` : save the cpu profile
5. `takeHeapSnapshot(filepath = 'node.heapsnapshot')` : speak for itself

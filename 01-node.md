# Node

## Debug a running process

1. `kill -SIGUSR1 <NODE_PID>` (put in debug mode)
2. `node inspect -p <NODE_PID>`
3. `profile` + `profileEnd` (after a min or two)
4. `profiles[0].save(filepath = 'node.cpuprofile')` : save the cpu profile
5. `takeHeapSnapshot(filepath = 'node.heapsnapshot')` : speak for itself (careful, that's really heavy!)

## Generate core dump

On crash: start node with `--abort-on-uncaught-exception` (make sure that `ulimit -c` is greater than 0)
When process is running: use `gcore -o <filename> <node_pid>`

Those can be analyzed using lldb-v8 / llnode, or mdb (but it only run on Solaris).

## Resources

- https://www.youtube.com/watch?v=O1YP8QP9gLA
- https://github.com/nodejs/llnode
- https://github.com/TritonDataCenter/mdb_v8
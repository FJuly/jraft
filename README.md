# jraft
Yet another Raft consensus implementation

## Project moved

**Please be noted that this project is no longer maintained, I will not fix any bugs, neither have new features in this project anymore, however, I have found a team to take over this for me, the code has been re-organized and published at [jraft](https://github.com/datatechnology/jraft), the team will start from there and make it under Apache 2, nothing is changed, but better support and more features on it**

The core algorithm is implemented based on the TLA+ spec, whose safety and liveness are proven.

## Supported Features,
- [x] Core Algorithm, safety, liveness are proven
- [x] Configuration Change Support, add or remove servers one by one without limitation
- [x] Client Request Support, to be enhanced, but is working now.
- [x] **Urgent commit**, see below
- [x] log compaction 

> Urgent Commit, is a new feature introduced by this implementation, which enables the leader asks all other servers to commit one or more logs if commit index is advanced. With Urgent Commit, the system's performance is highly improved and the heartbeat interval could be increased to seconds,depends on how long your application can abide when a leader goes down, usually, one or two seconds is fine. 

## About this implementation
it's always safer to implement such kind of algorithm based on Math description other than natural languge description.
there should be an auto conversion from TLA+ to programming languages, even they are talking things in different ways, but they are identical

> In the example of dmprinter (Distributed Message Printer), it takes about 4ms to commit a message, while in Active-Active scenario (sending messages to all three instances of dmprinter), it takes about 9ms to commit a message, the data is collected by CLT (Central Limitation Theory) with 95% of confidence level.

## Code Structure
I know it's lack of documentations, I will try my best, but if you can help, let me know.
* **net.data.technology.jraft**, the core algorithm implementation, you can go only with this, however, you need to implement the following interfaces,
  1. **Logger** and **LoggerFactory**
  2. **RpcClient** and **RpcClientFactory**
  3. **RpcListener**
  4. **ServerStateManager** and **SequentialLogStore**
  5. **StateMachine**
* **jraft-extensions**, some implementations for the interfaces mentioned above, it provides TCP based CompletableFuture<T> enabled RPC client and server as well as **FileBasedSequentialLogStore**, with this, you will be able to implement your own system by only implement **StateMachine** interface
* **jraft-dmprinter**, a sample application, as it's name, it's distributed message printer, for sample and testing.

## Planning
Here is the list of features planned for this project, 
- [x] Implement Http Based **RpcListener** and **RpcClient** plus **RpcClientFactory**
- [x] Implement buffering for FileBasedSequentialLogStore for **SequentialLogStore** to improve the overall system performance
- [ ] Implement a distributed database system by leveraging existing Java based Database system

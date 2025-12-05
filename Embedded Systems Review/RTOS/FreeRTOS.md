![[Pasted image 20251013210850.png]]
- Interrupts: signal to processor indicating an event needs to be handled

## FREE RTOS: 
Concept: FreeROTS is a real time operating system that manages multiple tasks by giving each one a slice of cpu time-giving the illusion of parallel processing 

Why: without RTOS, we need to manually manage timing, priorities, and resource sharing between different parts of program. 

Fundamentals: 
- Realtime: gives predictable timing - not necessarily fast 
- Preemptive: Higher priority tasks can interrupt lower priority ones 
- Portable: works with multiple microcontroller architectures 
- Open source
- Scalable for complex systems 

Core Components: 
- Scheduler: manages which tasks run when based on priorities 
- Task manager: handle task creation, deletion and state transitions 
- Memory manager: manages stack allocation and memory pools 
- Timing services: provides delays timeouts and periodic execution 
- Communication: queues semaphores, and mutexes for task coordination 

Task states: 
- created 
- Ready 
- Running 
- Blocked 
- Depleted 

Key points: 
- vtaskdelay doesnt block CPU, lets other tasks run 
- Stack size 128 is usually enough 
- priority = 1 is lowest priority 
- task runs in while (true) loop

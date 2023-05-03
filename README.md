Download Link: https://assignmentchef.com/product/solved-cs342-project-2-scheduling-threads-synchronization
<br>
In this project you will implement a multi-threaded scheduling simulator.

There will be N threads running concurrently and generating cpu bursts (workload). Let us call them as workload threads (<strong>W threads</strong>).  There will be one server thread that will be responsible from scheduling and executing the bursts (execution in a single cpu will be simulated). Let us call that thread as scheduler thread (<strong>S thread</strong>). Hence, there will be N+1 threads running concurrently, when we run your program.  The main thread will create all these N+1 threads in your program. There will be a <strong>runqueue</strong> (<strong>rq</strong>) structure (a linked list or a tree structure, like a red-black tree), i.e., a ready-queue, that will keep the cpu bursts to be executed by the scheduler thread. W threads will add their cpu bursts to rq. S thread will take and execute cpu bursts from rq.




Each W thread will generate a sequence of cpu bursts one-by-one and add them to rq. The length of each burst is random and exponentially distributed with mean <strong>avgB</strong>. The inter-arrival time between two consecutive busts is also random and exponential distributed with mean <strong>avgA</strong>.  A W thread will sleep between two burst generations (for that you can use the usleep function). When a burst is generated, its information (description) is added to rq structure as a new node. Information about a burst may include (at least): its thread index, its burst index, its length (in ms), the wall-clock time the burst is generated (you can get this by using the gettimeofday function; ms granularity should be enough).




Execution of a burst by the S thread will be simulated. When the S thread selected a burst  to execute, it will simulate the execution of the burst by sleeping for a time duration that is equal to the length of the burst (again you can use usleep). After that much of sleeping, the burst is considered finished (executed), and the S thread will select another burst from the rq structure to run next. The selection is done according to the specified algorithm.  If there is no burst to schedule in rq, the S thread will sleep on a <strong>condition</strong> <strong>variable</strong> until one of the W threads signal on the same condition variable (in this way you will practice <strong>POSIX</strong> condition variables).




Access to the rq structure requires synchronization of the threads (critical section). No two threads should access it simultaneously and cause race condition (in this way you will practice <strong>POSIX</strong> <strong>mutex</strong> variables).




The program will be called as <strong>schedule</strong> and will have the following parameters.




schedule &lt;N&gt; &lt;Bcount&gt; &lt;minB&gt; &lt;avgB&gt; &lt;minA&gt; &lt;avgA&gt; &lt;ALG&gt;




&lt;N&gt; is the number of W threads. It can be a value between 1 and 10. &lt;Bcount&gt; is the number of bursts that each W thread will generate. A burst time is to be generated as an exponentially distributed random value, with parameter &lt;avgB&gt;. If the generated random value is less than &lt;minB&gt;, it has to be generated again. Similarly, an interarrival time between two consecutive bursts (i.e., time to sleep between bursts in a W thread) is also to be generated as an exponentially distributed random value, with parameter &lt;avgA&gt;. If the generated random value is less than &lt;minA&gt;, it has to be generated again.  The &lt;ALG&gt; parameter specifies the scheduling algorithm to use. It can be one of: “FCFS”, “SJF”, “PRIO”, “VRUNTIME”.  All simulated scheduling algorithms are non-preemptive.




W threads are named, i.e., indexed, starting from 1, up to N. For example, W thread 1, or W thread 2. The first burst of a W thread has index 1, the next burst has index 2, etc.




Your simulator should simulate the following scheduling algorithms.




<ul>

 <li>Bursts served in the order they are added to the runqueue.</li>

 <li>SJF: When scheduling decision is to be made, the burst that has the smallest burst length is selected to run next. But the bursts of a particular thread must be scheduled in the same order they are generated. Hence you look to the earliest burst of each thread and select the shortest one.</li>

 <li>A W thread i has priority i. The smaller the number, the higher the priority. The burst in the runqueue belonging to the highest priority thread is selected. Again, we can not reorder the bursts of a particular thread. We have look to the earliest bursts of each thread in the runqueue and select the one that has highest priority.</li>

 <li>Each thread is associated with a virtual runtime (vruntime). When a burst of a thread is executed, the vruntime of the thread is advanced by some amount. The amount depends on the priority of the thread. When thread i runs t ms, its virtual runtime is advanced by t(0.7 + 0.3i). For example, virtual runtime of thread 1 is advanced by t ms  when it runs t ms in cpu; the virtual runtime of thread 11 is advanced by 4t when it runs t ms in cpu. When scheduling decision is to be made, the burst of the thread that has the smallest virtual runtime is selected for execution. Again we can not reorder the bursts of a particular thread. They need to be served in the order they are added to the runqueue.</li>

</ul>




It should also be possible to read burst information from files, instead of generating randomly. For that your program should provide  such an optionL -f &lt;inprefix&gt;. Then, a W thread i will read its burst information from a file called &lt;inprefix&gt;-i.txt. For example, if &lt;inprefix&gt; is “afile”, then the name of the input file for W thread 3 will be “afile-3.txt”.




Example invocations of the program can be as follows.




schedule 3 75 100 200 1000 1500 FCFS




schedule 5 FCFS -f infile




While setting parameter values, we should be careful not to exceed the capacity of the server (cpu). That means, the total burst arrival rate (from N W threads) should not exceed the serving rate (we should have large enough spacing between bursts, on the average).




You can implement the rq structure as a linked list.  There is not bound on how large the list can grow.




<strong>Experiments and Report (35 pts).  </strong>




Do a lot of experiments, i.e., run your program many times with various parameters values and measure some metrics. For example, you can measure the waiting time of bursts. Then you can find out the average waiting time for a thread. Then you can find the average waiting time caused by a scheduling algorithm for some number of threads. You can compare the  scheduling algorithms in terms of waiting time. You can try to draw conclusions.  You should repeat the experiments for various values of input parameters (avgB, avgA, N, etc,). It is up to you to design the experiments. For example, you can try to answer the following question: what is the average waiting time for each thread when we use the VRUNTIME algorithm?



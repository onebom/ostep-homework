
# Overview

process-run.py 이라고 하는 이 프로그램을 사용하면 CPU에서 실행될 때 프로세스 상태가 어떻게 변하는지 확인할 수 있습니다. 이 장에서 설명한 바와 같이 프로세스는 몇 가지 다른 상태에 있을 수 있습니다:

```sh
RUNNING - the process is using the CPU right now
READY   - the process could be using the CPU right now
          but (alas) some other process is
BLOCKED - the process is waiting on I/O
          (e.g., it issued a request to a disk)
DONE    - the process is finished executing
```

이 숙제에서는 프로그램이 실행됨에 따라 이러한 프로세스 상태가 어떻게 변화하는지 살펴보고 이러한 프로세스 상태가 어떻게 작동하는지에 대해 조금 더 잘 배울 것입니다.

To run the program and get its options, do this:

```sh
prompt> ./process-run.py -h
```

If this doesn't work, type `python` before the command, like this:

```sh
prompt> python process-run.py -h
```

What you should see is this:

```sh
Usage: process-run.py [options]

Options:
  -h, --help            show this help message and exit
  -s SEED, --seed=SEED  the random seed
  -l PROCESS_LIST, --processlist=PROCESS_LIST
                        a comma-separated list of processes to run, in the
                        form X1:Y1,X2:Y2,... where X is the number of
                        instructions that process should run, and Y the
                        chances (from 0 to 100) that an instruction will use
                        the CPU or issue an IO
  -L IO_LENGTH, --iolength=IO_LENGTH
                        how long an IO takes
  -S PROCESS_SWITCH_BEHAVIOR, --switch=PROCESS_SWITCH_BEHAVIOR
                        when to switch between processes: SWITCH_ON_IO,
                        SWITCH_ON_END
  -I IO_DONE_BEHAVIOR, --iodone=IO_DONE_BEHAVIOR
                        type of behavior when IO ends: IO_RUN_LATER,
                        IO_RUN_IMMEDIATE
  -c                    compute answers for me
  -p, --printstats      print statistics at end; only useful with -c flag
                        (otherwise stats are not printed)
```

이해해야 할 가장 중요한 옵션은 PROCESS_LIST(-l 또는 --processlist 플래그로 지정됨)이며, 실행 중인 각 프로그램(또는 'process')이 수행할 작업을 정확히 지정합니다. 프로세스는 명령어로 구성되며 각 명령어는 다음 두 가지 중 하나만 수행할 수 있습니다:
1. use the CPU 
2. issue an IO (and wait for it to complete)

프로세스가 CPU를 사용하고 IO가 전혀 없는 경우에는 CPU에서 실행 중인지 실행 준비 중인지를 번갈아 선택해야 합니다. 예를 들어, 프로그램이 하나만 실행되고 해당 프로그램은 CPU만 사용하는 간단한 실행이 있습니다(IO는 사용하지 않음).

아래 코드에서 우리는 "5:100"으로 프로세스를 지정합니다.. 즉, 5개의 명령으로 구성되어야 하며 각 명령이 CPU 명령일 가능성은 100%입니다.

```sh
prompt> ./process-run.py -l 5:100 
Produce a trace of what would happen when you run these processes:
Process 0
  cpu
  cpu
  cpu
  cpu
  cpu

Important behaviors:
  System will switch when the current process is FINISHED or ISSUES AN IO
  After IOs, the process issuing the IO will run LATER (when it is its turn)

prompt> 
```

-c 플래그를 사용하여 프로세스에 어떤 일이 발생하는지 확인할 수 있습니다. -c 플래그는 다음과 같은 답을 계산합니다:

```sh
prompt> ./process-run.py -l 5:100 -c
Time     PID: 0        CPU        IOs
  1     RUN:cpu          1
  2     RUN:cpu          1
  3     RUN:cpu          1
  4     RUN:cpu          1
  5     RUN:cpu          1
```

이 결과는 그다지 흥미롭지 않습니다. 프로세스가 RUN 상태에서 단순한 다음 종료됩니다. CPU를 계속 사용하므로 실행 내내 CPU를 사용하고 I/O를 전혀 수행하지 않습니다

Let's make it slightly more complex by running two processes:

```sh
prompt> ./process-run.py -l 5:100,5:100
Produce a trace of what would happen when you run these processes:
Process 0
  cpu
  cpu
  cpu
  cpu
  cpu

Process 1
  cpu
  cpu
  cpu
  cpu
  cpu

Important behaviors:
  Scheduler will switch when the current process is FINISHED or ISSUES AN IO
  After IOs, the process issuing the IO will run LATER (when it is its turn)
```

이 경우 두 개의 서로 다른 프로세스가 실행되며, 각각 CPU만 사용합니다. 운영 체제에서 이러한 기능을 실행하면 어떻게 됩니까? 확인해 보겠습니다:

```sh
prompt> ./process-run.py -l 5:100,5:100 -c
Time     PID: 0     PID: 1        CPU        IOs
  1     RUN:cpu      READY          1
  2     RUN:cpu      READY          1
  3     RUN:cpu      READY          1
  4     RUN:cpu      READY          1
  5     RUN:cpu      READY          1
  6        DONE    RUN:cpu          1
  7        DONE    RUN:cpu          1
  8        DONE    RUN:cpu          1
  9        DONE    RUN:cpu          1
 10        DONE    RUN:cpu          1
```

위에서 볼 수 있듯이 먼저 "process ID" (or "PID")가 0인 프로세스가 실행되고, process 1은 실행 준비가 되었지만 0이 완료될 때까지 기다립니다. 0이 완료되면 DONE 상태로 이동하고 1이 실행됩니다. 1이 완료되면 추적이 완료됩니다.

몇 가지 질문을 하기 전에 한 가지 예를 더 살펴보겠습니다. 이 예에서는 프로세스가 I/O 요청만 실행합니다. 여기서 I/O가 -L 플래그로 완료하는 데 5개의 시간 단위가 소요되도록 지정합니다.
```sh
prompt> ./process-run.py -l 3:0 -L 5
Produce a trace of what would happen when you run these processes:
Process 0
  io
  io_done
  io
  io_done
  io
  io_done

Important behaviors:
  System will switch when the current process is FINISHED or ISSUES AN IO
  After IOs, the process issuing the IO will run LATER (when it is its turn)
```
운영 체제에서 이러한 기능을 실행하면 어떻게 됩니까? 확인해 보겠습니다:

```sh
prompt> ./process-run.py -l 3:0 -L 5 -c
Time    PID: 0       CPU       IOs
  1         RUN:io             1
  2        BLOCKED                           1
  3        BLOCKED                           1
  4        BLOCKED                           1
  5        BLOCKED                           1
  6        BLOCKED                           1
  7*   RUN:io_done             1
  8         RUN:io             1
  9        BLOCKED                           1
 10        BLOCKED                           1
 11        BLOCKED                           1
 12        BLOCKED                           1
 13        BLOCKED                           1
 14*   RUN:io_done             1
 15         RUN:io             1
 16        BLOCKED                           1
 17        BLOCKED                           1
 18        BLOCKED                           1
 19        BLOCKED                           1
 20        BLOCKED                           1
 21*   RUN:io_done             1
```

보시다시피 프로그램은 3개의 I/O만 실행합니다. 각 I/O가 실행되면 프로세스가 BLOCKED 상태로 이동하고 디바이스가 I/O를 서비스하는 동안 CPU가 유휴 상태가 됩니다
I/O 완료를 처리하기 위해 CPU 작업이 하나 더 발생합니다. I/O 시작 및 완료를 처리하는 단일 지침은 특별히 현실적인 것이 아니라 단순성을 위해 여기에서 사용됩니다.

몇 가지 통계를 인쇄하여(위와 동일한 명령을 실행하지만 -p 플래그를 사용하여) 몇 가지 전반적인 동작을 확인합니다:

```sh
Stats: Total Time 21
Stats: CPU Busy 6 (28.57%)
Stats: IO Busy  15 (71.43%)
```

보시다시피 추적을 실행하는 데 21개의 클럭 틱이 필요했지만 CPU 사용량이 30% 미만이었습니다. 반면에 I/O 장치는 상당히 사용 중이었습니다. 일반적으로 리소스를 더 잘 사용할 수 있기 때문에 모든 장치를 사용 중으로 유지하고 싶습니다.
몇 가지 다른 중요한 플래그가 있습니다:
```sh
  -s SEED, --seed=SEED  the random seed  
    this gives you way to create a bunch of different jobs randomly

  -L IO_LENGTH, --iolength=IO_LENGTH
    this determines how long IOs take to complete (default is 5 ticks)

  -S PROCESS_SWITCH_BEHAVIOR, --switch=PROCESS_SWITCH_BEHAVIOR
                        when to switch between processes: SWITCH_ON_IO, SWITCH_ON_END
    this determines when we switch to another process:
    - SWITCH_ON_IO, the system will switch when a process issues an IO
    - SWITCH_ON_END, the system will only switch when the current process is done 

  -I IO_DONE_BEHAVIOR, --iodone=IO_DONE_BEHAVIOR
                        type of behavior when IO ends: IO_RUN_LATER, IO_RUN_IMMEDIATE
    this determines when a process runs after it issues an IO:
    - IO_RUN_IMMEDIATE: switch to this process right now
    - IO_RUN_LATER: switch to this process when it is natural to 
      (e.g., depending on process-switching behavior)
```

Now go answer the questions at the back of the chapter to learn more, please.





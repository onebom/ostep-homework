# Homeworks

숙제는 각 장에서 자료에 대한 지식을 굳히기 위해 사용될 수 있습니다. 많은 숙제는 운영 체제의 일부 측면을 모방하는 시뮬레이터를 실행하는 것에 기초합니다. 예를 들어 디스크 스케줄링 시뮬레이터는 다양한 디스크 스케줄링 알고리즘이 작동하는 방식을 이해하는 데 유용할 수 있습니다. 일부 다른 숙제들은 실제 시스템이 어떻게 작동하는지 탐구할 수 있게 해주는 짧은 프로그래밍 연습일 뿐입니다.

시뮬레이터의 기본 아이디어는 간단합니다. 아래의 각 시뮬레이터를 사용하면 문제를 생성하고 무한한 수의 문제에 대한 해결책을 얻을 수 있습니다. 일반적으로 다른 랜덤 시드를 사용하여 다른 문제를 생성할 수 있습니다. -c 플래그를 사용하면 정답이 계산됩니다(아마 직접 계산을 시도한 후에!).

아래에 포함된 각 숙제는 어떻게 해야 하는지 설명하는 README 파일을 가지고 있습니다. 이전에, 이 자료는 장 자체에 포함되어 있었지만, 그것은 책을 너무 길게 만들었습니다. 이제 이 책에 남아 있는 것은 시뮬레이터로 답하고 싶은 질문뿐입니다. 코드를 실행하는 방법에 대한 자세한 내용은 README에 모두 있습니다.

그러므로, 여러분의 과제는 한 장을 읽고, 그 장의 끝에 있는 질문들을 보고, 숙제를 함으로써 그 질문들에 답하도록 노력하는 것입니다. 일부는 시뮬레이터(Python으로 작성)가 필요하며, 아래에서 사용할 수 있습니다. 코드를 작성해야 하는 경우도 있습니다. 이 시점에서 관련 README를 읽는 것이 좋습니다. 여전히 다른 것들은 C 코드를 작성하는 것과 같은 몇 가지 다른 것들을 필요로 합니다.

To use these, the best thing to do is to clone the homeworks. For example:
```sh
prompt> git clone https://github.com/remzi-arpacidusseau/ostep-homework/
prompt> cd file-disks
prompt> ./disk.py -h
```

# Introduction

Chapter | What To Do
--------|-----------
[Introduction](http://www.cs.wisc.edu/~remzi/OSTEP/intro.pdf) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; | No homework (yet)

# Virtualization

Chapter | What To Do
--------|-----------
[Abstraction: Processes](http://www.cs.wisc.edu/~remzi/OSTEP/cpu-intro.pdf) | Run [process-run.py](cpu-intro)
[Process API](http://www.cs.wisc.edu/~remzi/OSTEP/cpu-api.pdf) | Run [fork.py](cpu-api) and write some code
[Direct Execution](http://www.cs.wisc.edu/~remzi/OSTEP/cpu-mechanisms.pdf) | Write some code
[Scheduling Basics](http://www.cs.wisc.edu/~remzi/OSTEP/cpu-sched.pdf) | Run [scheduler.py](cpu-sched)
[MLFQ Scheduling](http://www.cs.wisc.edu/~remzi/OSTEP/cpu-sched-mlfq.pdf)	| Run [mlfq.py](cpu-sched-mlfq)
[Lottery Scheduling](http://www.cs.wisc.edu/~remzi/OSTEP/cpu-sched-lottery.pdf) | Run [lottery.py](cpu-sched-lottery)
[Multiprocessor Scheduling](http://www.cs.wisc.edu/~remzi/OSTEP/cpu-sched-multi.pdf) | Run [multi.py](cpu-sched-multi)
[Abstraction: Address Spaces](http://www.cs.wisc.edu/~remzi/OSTEP/vm-intro.pdf) | Write some code
[VM API](http://www.cs.wisc.edu/~remzi/OSTEP/vm-api.pdf) | Write some code
[Relocation](http://www.cs.wisc.edu/~remzi/OSTEP/vm-mechanism.pdf) | Run [relocation.py](vm-mechanism)
[Segmentation](http://www.cs.wisc.edu/~remzi/OSTEP/vm-segmentation.pdf) | Run [segmentation.py](vm-segmentation)
[Free Space](http://www.cs.wisc.edu/~remzi/OSTEP/vm-freespace.pdf) | Run [malloc.py](vm-freespace)
[Paging](http://www.cs.wisc.edu/~remzi/OSTEP/vm-paging.pdf) | Run [paging-linear-translate.py](vm-paging)
[TLBs](http://www.cs.wisc.edu/~remzi/OSTEP/vm-tlbs.pdf) | Write some code
[Multi-level Paging](http://www.cs.wisc.edu/~remzi/OSTEP/vm-smalltables.pdf) | Run [paging-multilevel-translate.py](vm-smalltables)
[Paging Mechanism](http://www.cs.wisc.edu/~remzi/OSTEP/vm-beyondphys.pdf) | Run [mem.c](vm-beyondphys)
[Paging Policy](http://www.cs.wisc.edu/~remzi/OSTEP/vm-beyondphys-policy.pdf) | Run [paging-policy.py](vm-beyondphys-policy)
[Complete VM](http://www.cs.wisc.edu/~remzi/OSTEP/vm-complete.pdf) | No homework (yet)

# Concurrency

Chapter | What To Do
--------|-----------
[Threads Intro](http://www.cs.wisc.edu/~remzi/OSTEP/threads-intro.pdf) | Run [x86.py](threads-intro)
[Thread API](http://www.cs.wisc.edu/~remzi/OSTEP/threads-api.pdf)	| Run [some C code](threads-api)
[Locks](http://www.cs.wisc.edu/~remzi/OSTEP/threads-locks.pdf)	| Run [x86.py](threads-locks)
[Lock Usage](http://www.cs.wisc.edu/~remzi/OSTEP/threads-locks-usage.pdf) | Write some code
[Condition Variables](http://www.cs.wisc.edu/~remzi/OSTEP/threads-cv.pdf) | Run [some C code](threads-cv)
[Semaphores](http://www.cs.wisc.edu/~remzi/OSTEP/threads-sema.pdf) | Read and write [some code](threads-sema)
[Concurrency Bugs](http://www.cs.wisc.edu/~remzi/OSTEP/threads-bugs.pdf) | Run [some C code](threads-bugs)
[Event-based Concurrency](http://www.cs.wisc.edu/~remzi/OSTEP/threads-events.pdf) | Write some code

# Persistence

Chapter | What To Do
--------|-----------
[I/O Devices](http://www.cs.wisc.edu/~remzi/OSTEP/file-devices.pdf) | No homework (yet)
[Hard Disk Drives](http://www.cs.wisc.edu/~remzi/OSTEP/file-disks.pdf) | Run [disk.py](file-disks)
[RAID](http://www.cs.wisc.edu/~remzi/OSTEP/file-raid.pdf) | Run [raid.py](file-raid)
[FS Intro](http://www.cs.wisc.edu/~remzi/OSTEP/file-intro.pdf) | Write some code
[FS Implementation](http://www.cs.wisc.edu/~remzi/OSTEP/file-implementation.pdf) | Run [vsfs.py](file-implementation)
[Fast File System](http://www.cs.wisc.edu/~remzi/OSTEP/file-ffs.pdf) | Run [ffs.py](file-ffs)
[Crash Consistency and Journaling](http://www.cs.wisc.edu/~remzi/OSTEP/file-journaling.pdf) | Run [fsck.py](file-journaling)
[Log-Structured File Systems](http://www.cs.wisc.edu/~remzi/OSTEP/file-lfs.pdf) | Run [lfs.py](file-lfs)
[Solid-State Disk Drives](http://www.cs.wisc.edu/~remzi/OSTEP/file-ssd.pdf) | Run [ssd.py](file-ssd)
[Data Integrity](http://www.cs.wisc.edu/~remzi/OSTEP/file-integrity.pdf) | Run [checksum.py](file-integrity) and Write some code
[Distributed Intro](http://www.cs.wisc.edu/~remzi/OSTEP/dist-intro.pdf) | Write some code
[NFS](http://www.cs.wisc.edu/~remzi/OSTEP/dist-nfs.pdf) | Write some analysis code
[AFS](http://www.cs.wisc.edu/~remzi/OSTEP/dist-afs.pdf) | Run [afs.py](dist-afs)

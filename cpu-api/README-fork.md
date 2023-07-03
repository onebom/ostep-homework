
# Overview: `fork.py`

시뮬레이터 "fork.py "은 프로세스가 생성되고 파괴될 때 프로세스 트리가 어떻게 보이는지 보여주는 간단한 도구입니다.

To run it, just:
```sh
prompt> ./fork.py
```
그러먄 프로세스가 다른 프로세스를 만들기 위해 "fork"를 호출할지 또는 프로세스가 실행을 중지하기 위해 "exit"를 호출할지 여부와 같은 작업 목록이 표시됩니다.

실행 중인 각 프로세스는 여러 개의 children(or none)을 가질 수 있다. 초기 프로세스를 제외한 모든 프로세스는 단일 부모를 가진다. 따라서 모든 프로세스는 초기 프로세스에 뿌리를 둔 트리에 관련된다. 이 트리를 "porcess tree"라고 부른다. 프로세스가 생성되고 파괴되는 과정을 이해하는 것이 이 homework의 요점이다.

# Simple Example

Here is a simple example:
```sh
prompt> ./fork.py -s 4

                           Process Tree:
                               a
Action: a forks b
Process Tree?
Action: a forks c
Process Tree?
Action: b forks d
Process Tree?
Action: d EXITS
Process Tree?
Action: a forks e
Process Tree?
```

출력에서 두가지를 볼 수 있다. 첫째, 오른쪽은 시스템의 초기상태다. 'a'는 하나의 과정을 포함하고 있다. 운영체제는 종종 작업을 진행하기 위해 하나 또는 몇개의 초기 프로세스를 생성한다. 에를 들어, Unix에서는 초기 프로세스를 'init'이라고 하며, 시스템이 실행되는 동안 다른 프로세스를 생성한다.
둘째, 왼쪽에는 일련의 'action' 목록이 있다. 다양한 작업이 수행되고 프로세스 트리의 상태에 대한 질문이 제기된다.
모든 출력을 표시하려면 다음과 같이 '-c'플래그를 사용한다;
```sh
prompt> ./fork.py -s 4 -c                                                                       +100

                           Process Tree:
                               a

Action: a forks b
                               a
                               └── b
Action: a forks c
                               a
                               ├── b
                               └── c
Action: b forks d
                               a
                               ├── b
                               │   └── d
                               └── c
Action: d EXITS
                               a
                               ├── b
                               └── c
Action: a forks e
                               a
                               ├── b
                               ├── c
                               └── e
prompt>
```
특정 작업의 결과 트리가 오른쪽에 표시된다. 첫번째 동작인 "a fork b" 뒤 "a"가 "b"부모로 보이는 매우 단순한 나무가 출력된다. 'exit'은 트리를 줄여준다. 마지막으로 e가 생성되고 최종 트리가 생성된다.

단순모드에서는 '-F'플래그를 사용하여 최종 프로세스 트리를 적어봄으로써 자신을 테스트할 수 있다.
```sh
prompt> ./fork.py -s 4 -F
                           Process Tree:
                               a

Action: a forks b
Action: a forks c
Action: b forks d
Action: d EXITS
Action: a forks e

                        Final Process Tree?
```

Once again, you can use the `-c` flag to compute the answer and see if
you were right (in this case, you should be, because it's the same
problem!)

# Other Options

포크 시뮬레이터에는 그 밖에도 여러 가지 옵션이 있습니다.

프로세스 트리 상태를 보고 어떤 작업이 수행되었을 것인지 추측할 수 있는 '-t' 플래그를 사용하여 질문을 뒤집을 수 있습니다.

다른 임의 시드('-s' 플래그)를 사용하거나 임의로 생성된 다른 시퀀스를 얻기 위해 하나를 지정하지 않을 수 있습니다.

'-f' 플래그를 사용하여 포크(출구)와 포크(출구)의 작업 비율을 변경할 수 있습니다.

'-A' 플래그를 사용하여 특정 포크 및 종료 시퀀스를 지정할 수 있습니다. 예를 들어, "a" 포크 "b", "b", "c" 종료 후 "a" 포크 "d"를 입력하려면 다음과 같이 입력합니다(문제를 해결하기 위해 "-c"도 여기에 표시합니다):

```sh
prompt> ./fork.py -A a+b,b+c,c-,a+d -c

                           Process Tree:
                               a

Action: a forks b
                               a
                               └── b
Action: b forks c
                               a
                               └── b
                                   └── c
Action: c EXITS
                               a
                               └── b
Action: a forks d
                               a
                               ├── b
                               └── d
```

You can only show the final output (and see if you can guess all the
intermediates to get there) with the `-F` flag.

Finally, you can change the printing style of the tree with the `-P`
flag. 

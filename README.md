## Laboratory work VIII

Данная лабораторная работа посвещена изучению систем автоматизации развёртывания и управления приложениями на примере **Docker**

## Tutorial

```sh
$ export GITHUB_USERNAME=Nejelia
```
Следующим шагом клонируем репрозиторий из лабораторной работы 6 в лабораторную работу 8. Поскольку эту лабораторную я выполняла уже, некоторые нужные файлы я потом вручную докидываю из прошлой работы над проектом, чтобы сразу избежать ошибок, с которыми я сталкивалась.П.С. переделываю потому что нашла там ошибку.

```sh
$ cd Nejelia/workspace
$ git clone https://github.com/Nejelia/lab06 lab08fix
$ cd lab08fix
$ git remote remove origin
$ git remote add origin https://github.com/Nejelia/lab08fix
$ git remote -v
origin	https://github.com//Nejelia/lab08fix (fetch)
origin	https://github.com//Nejelia/lab08fix (push)
```

```sh
$ cat > Dockerfile <<EOF
FROM ubuntu:18.04
EOF
```

```sh
$ cat >> Dockerfile <<EOF

RUN apt update
RUN apt install -yy gcc g++ cmake
EOF
```

```sh
$ cat >> Dockerfile <<EOF

COPY . print/
WORKDIR print
EOF
```

```sh
$ cat >> Dockerfile <<EOF

RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build
RUN cmake --build _build --target install
EOF
```

```sh
$ cat >> Dockerfile <<EOF

ENV LOG_PATH /home/logs/log.txt
EOF
```

```sh
$ cat >> Dockerfile <<EOF

VOLUME /home/logs
EOF
```

```sh
$ cat >> Dockerfile <<EOF

WORKDIR _install/bin
EOF
```

```sh
$ cat >> Dockerfile <<EOF

ENTRYPOINT ./demo
EOF
```

```sh
$ sudo docker build -t logger .
```

<details open>
<summary>Развернуть</summary><p>


```
[sudo] пароль для nejelia: 
Sending build context to Docker daemon   1.25MB
Step 1/12 : FROM ubuntu:18.04
 ---> f9a80a55f492
Step 2/12 : RUN apt update
 ---> Using cache
 ---> bb4f9a61bf59
Step 3/12 : RUN apt install -yy gcc g++ cmake
 ---> Using cache
 ---> 0adb81e26bea
Step 4/12 : COPY . print/
 ---> 49cb3d0f9c9a
Step 5/12 : WORKDIR print
 ---> Running in 51d5c6005a94
Removing intermediate container 51d5c6005a94
 ---> e7799ea7c1eb
Step 6/12 : RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
 ---> Running in 43d33518b4cd
-- The C compiler identification is GNU 7.5.0
-- The CXX compiler identification is GNU 7.5.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /print/_build
Removing intermediate container 43d33518b4cd
 ---> 37283de34af9
Step 7/12 : RUN cmake --build _build
 ---> Running in acf35b59eecf
Scanning dependencies of target formatter_ex_lib
[ 12%] Building CXX object CMakeFiles/formatter_ex_lib.dir/formatter_ex_lib/formatter_ex.cpp.o
[ 25%] Linking CXX static library libformatter_ex_lib.a
[ 25%] Built target formatter_ex_lib
Scanning dependencies of target formatter_lib
[ 37%] Building CXX object CMakeFiles/formatter_lib.dir/formatter_lib/formatter.cpp.o
[ 50%] Linking CXX static library libformatter_lib.a
[ 50%] Built target formatter_lib
Scanning dependencies of target solver_lib
[ 62%] Scanning dependencies of target solver_lib
[ 62%] Building CXX object CMakeFiles/solver_lib.dir/solver_lib/solver.cpp.o
[ 75%] Linking CXX static library libsolver_lib.a
[ 75%] Built target solver_lib
Scanning dependencies of target solver
[ 87%] Building CXX object CMakeFiles/solver.dir/solver_application/equation.cpp.o
[100%] Linking CXX executable solver
[100%] Built target solver
Removing intermediate container acf35b59eecf
 ---> 701eb6521f96
Step 8/12 : RUN cmake --build _build && cmake --install _build
 ---> Running in d781e2635bde
[ 25%] Built target formatter_ex_lib
[ 50%] Built target formatter_lib
[ 75%] Built target solver_lib
[100%] Built target solver
-- Configuring done
-- Generating done
-- Build files have been written to: /print/_build
Removing intermediate container d781e2635bde
 ---> 3e64b4efc81a
Step 9/12 : ENV LOG_PATH /home/logs/log.txt
 ---> Running in faec91644c1d
Removing intermediate container faec91644c1d
 ---> 3456baaa892f
Step 10/12 : VOLUME /home/logs
 ---> Running in 6472de5d8244
Removing intermediate container 6472de5d8244
 ---> 1d234e327995
Step 11/12 : WORKDIR _install/bin
 ---> Running in 140fadbb26f4
Removing intermediate container 140fadbb26f4
 ---> d54f0e8780ef
Step 12/12 : ENTRYPOINT ./demo
 ---> Running in 452aa4637d7c
Removing intermediate container 452aa4637d7c
 ---> 12b0cffdd547
Successfully built 12b0cffdd547
Successfully tagged logger:latest
```
  
  </p></details>.

```sh
nejelia@nejelia-ROG-Strix-G533QM-G533QM:~/Nejelia/workspace/lab08fix$ sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
logger       latest    12b0cffdd547   22 seconds ago   335MB
<none>       <none>    040d893dbd33   25 minutes ago   334MB
<none>       <none>    873637952252   35 minutes ago   334MB
<none>       <none>    c71645ac97f3   58 minutes ago   334MB
ubuntu       18.04     f9a80a55f492   2 days ago      63.2MB
```

```sh
$ mkdir logs
$ sudo docker run -it -v "$(pwd)/logs/:/home/logs/" logger
text1
text2
text3
```

```sh
nejelia@nejelia-ROG-Strix-G533QM-G533QM:~/Nejelia/workspace/lab08fix$ sudo docker inspect logger
```

<details open>
<summary>Развернуть</summary><p>


```
[
    {
        "Id": "sha256:12b0cffdd54740cda6182da7f9232c24934f1d836b3983ba80a1386d51db9d14",
        "RepoTags": [
            "logger:latest"
        ],
        "RepoDigests": [],
        "Parent": "sha256:d54f0e8780ef11e35bb45e5c6ccc840e0d95046dba3a3f14bd6de6c42600b6fd",
        "Comment": "",
        "Created": "2023-06-19T19:56:39.226363084Z",
        "Container": "452aa4637d7c35eed90368f66f9289adfde99ef114d1fc9021023be0e7130495",
        "ContainerConfig": {
            "Hostname": "452aa4637d7c",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "ENTRYPOINT [\"/bin/sh\" \"-c\" \"./demo\"]"
            ],
            "Image": "sha256:d54f0e8780ef11e35bb45e5c6ccc840e0d95046dba3a3f14bd6de6c42600b6fd",
            "Volumes": {
                "/home/logs": {}
            },
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "18.04"
            }
        },
        "DockerVersion": "20.10.24",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": null,
            "Image": "sha256:d54f0e8780ef11e35bb45e5c6ccc840e0d95046dba3a3f14bd6de6c42600b6fd",
            "Volumes": {
                "/home/logs": {}
            },
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "18.04"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 334753608,
        "VirtualSize": 334753608,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/snap/docker/common/var-lib-docker/overlay2/0ed7c7fb6f6c40a1a4d3ea8283401d5a6e7c3d5bdce855a1c209bf2fe9c2d7be/diff:/var/snap/docker/common/var-lib-docker/overlay2/383d8a11e1fb44e9a9725c0a5a7ecc8ab7b8f1bfd9e594b9a144aecffdfa2be4/diff:/var/snap/docker/common/var-lib-docker/overlay2/5abc159d497852dbe47dc7c47e234f9a6aea45aeff5329a8151ca7fce28126d9/diff:/var/snap/docker/common/var-lib-docker/overlay2/ee143f9d0a77803f2d29657b169c161030c0085db9a5f3c7cf00d2d43af257f6/diff:/var/snap/docker/common/var-lib-docker/overlay2/f77a7aaac165c08a6cc3a39c613a501aa9d759bd5465e292d547cc9254ce0ef8/diff:/var/snap/docker/common/var-lib-docker/overlay2/7c3e087e9567040111ef0845b3390b4e64b25767f769bf7aceb8791af1adb56b/diff:/var/snap/docker/common/var-lib-docker/overlay2/a342cea26bb78e7e61bdd21c28f392f32572f05c5e0501ac26ef71a100b792a1/diff",
                "MergedDir": "/var/snap/docker/common/var-lib-docker/overlay2/f8f94d94528fcfef7d9fcd5bcbe5b15f97df818ea2350490f50942c3106ee95d/merged",
                "UpperDir": "/var/snap/dock
er/common/var-lib-docker/overlay2/f8f94d94528fcfef7d9fcd5bcbe5b15f97df818ea2350490f50942c3106ee95d/diff",
                "WorkDir": "/var/snap/docker/common/var-lib-docker/overlay2/f8f94d94528fcfef7d9fcd5bcbe5b15f97df818ea2350490f50942c3106ee95d/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:548a79621a426b4eb077c926eabac5a8620c454fb230640253e1b44dc7dd7562",
                "sha256:4b017e1116b6e6d5d8c90070a0c97fa34c52da6ec92d69e6bbcce6ea3ba74277",
                "sha256:5b6865d63919a05eaef6fed20c8f61faf6a7744c6e94436371baed29578e89a6",
                "sha256:61495bfa168a789f0fdb57a5e2da459c4fc85cba65e4f19fa7ba3b85c4dc328e",
                "sha256:18bc568d801319d739a14114e2f23dbc655f2a9e96174fbb405e6beeaf5a2a82",
                "sha256:c2667a798b95cf7d4441e181450bef06bb88ff74cbae1742331771cd92705c3c",
                "sha256:8c718596fada6789ca5cac00cc4218aa9a6af061d159d180b2d384ae4742e2a1",
                "sha256:eb3f05deefcfa4a792f2b2cc31a588c479c9c43af657fd0d613c5945892f2664"
            ]
        },
        "Metadata": {
            "LastTagTime": "2023-06-19T22:56:39.238478727+03:00"
        }
    }
]
```
  
  </p></details>.

```sh
nejelia@nejelia-ROG-Strix-G533QM-G533QM:~/Nejelia/workspace/lab08fix$ cat logs/log.txt
text1
text2
text3
```
Пушим проект

```sh
$ git status
$ git add .
$ git commit -m"update"
$ git push origin main 
```

## Links

- [Book](https://www.dockerbook.com)
- [Instructions](https://docs.docker.com/engine/reference/builder/)

```
Copyright (c) 2015-2021 The ISC Authors
```

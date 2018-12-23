---
layout:     post
title:      在Centos上安装MPI
subtitle:   #1
date:       2018-12-23
author:     tingjieee
header-img: img/post-bg-parallel.jpg
catalog: true
tags:
    - parallel
---


##安装（centos 7.2）
- yum list mpich*，查看MPI可安装的版本，我是直接全部安装
- sudo yum install -y mpich*
- 由于安装之后找不到命令，所以需要设置环境变量
    - sudo find / -name "mpicc"，应该可以查看到安装路径
    - vim ~/.bashrc
    ```
    #在文件中添加，具体路径要根据系统和具体环境，以下是我的情况：
    export PATH=$PATH:/usr/lib64/mpich/bin/
    ```
    - source .bashrc 生效环境变量设置

至此可以在shell中使用mpicc，通过which mpicc可以查看到设置的路径。

mpi代码示例：
```
#include <mpi.h>
#include <stdio.h>
#include <math.h>
int main(int argc,char** argv)
{
	int myid,numproces;
	int namelen;
	char processor_name[MPI_MAX_PROCESSOR_NAME];
	MPI_Init(&argc,&argv);
	MPI_Comm_rank(MPI_COMM_WORLD,&myid);
	MPI_Comm_size(MPI_COMM_WORLD,&numproces);
	MPI_Get_processor_name(processor_name,&namelen);
	fprintf(stdout,"hello world! Process %d of %d on %s\n",
			myid,numproces,processor_name);
	MPI_Finalize();

	return 0;
}
```
编译：
> mpicc -o hello hello.c

运行：
>mpirun -np 4 ./hello

输出:

>hello world! Process 0 of 4 on node25
hello world! Process 1 of 4 on node25
hello world! Process 3 of 4 on node25
hello world! Process 2 of 4 on node25
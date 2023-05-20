---
title: CUDA编程之threadIdx, blockIdx, blockDim, gridDim的使用
date: 2022-06-20 15:01:50.0
updated: 2022-07-28 02:29:07.416
url: /p=8
categories: 
tags: 
- 高性能计算
---

具体：
threadIdx是一个uint3类型，表示一个线程的索引。
blockIdx是一个uint3类型，表示一个线程块的索引，一个线程块中通常有多个线程。
blockDim是一个dim3类型，表示线程块的大小。
gridDim是一个dim3类型，表示网格的大小，一个网格中通常有多个线程块。

cuda 通过<<< >>>符号来分配索引线程的方式，例如以下的索引方式。

```C++
1.	//thread 1D
int i = threadIdx.x;
testThread1<<<1, size>>>(dev_c, dev_a, dev_b);
2.	//thread 2D
int i = threadIdx.x + threadIdx.y*blockDim.x; 
//uint3 s;s.x = size/5;s.y = 5;s.z = 1; 
//testThread2 <<<1,s>>>(dev_c, dev_a, dev_b);
3.	//thread 3D
int i = threadIdx.x + threadIdx.y*blockDim.x + threadIdx.z*blockDim.x*blockDim.y;
//uint3 s; s.x = size / 10; s.y = 5; s.z = 2;
//testThread3<<<1, s >>>(dev_c, dev_a, dev_b);
4.	//block 1D
int i = blockIdx.x;
//testBlock1<<<size,1 >>>(dev_c, dev_a, dev_b); 
5.	//block 2D
int i = blockIdx.x + blockIdx.y*gridDim.x;
//uint3 s; s.x = size / 5; s.y = 5; s.z = 1;
//testBlock2<<<s, 1 >>>(dev_c, dev_a, dev_b);
   
6.	//block 3D
int i = blockIdx.x + blockIdx.y*gridDim.x + blockIdx.z*gridDim.x*gridDim.y;
//uint3 s; s.x = size / 10; s.y = 5; s.z = 2;
//testBlock3<<<s, 1 >>>(dev_c, dev_a, dev_b);
    
7.	//block-thread 1D-1D
int i = threadIdx.x + blockDim.x*blockIdx.x;
//testBlockThread1<<<size/10, 10>>>(dev_c, dev_a, dev_b);
 
8.	//block-thread 1D-2D
int threadId_2D = threadIdx.x + threadIdx.y*blockDim.x;
int i = threadId_2D+ (blockDim.x*blockDim.y)*blockIdx.x;
//uint3 s1; s1.x = size / 100; s1.y = 1; s1.z = 1;
//uint3 s2; s2.x = 10; s2.y = 10; s2.z = 1;
//testBlockThread2 << <s1, s2 >> >(dev_c, dev_a, dev_b);
   
9.	//block-thread 1D-3D
int threadId_3D = threadIdx.x + threadIdx.y*blockDim.x + threadIdx.z*blockDim.x*blockDim.y;
int i = threadId_3D + (blockDim.x*blockDim.y*blockDim.z)*blockIdx.x;
//uint3 s1; s1.x = size / 100; s1.y = 1; s1.z = 1;
//uint3 s2; s2.x = 10; s2.y = 5; s2.z = 2;
//testBlockThread3 << <s1, s2 >> >(dev_c, dev_a, dev_b);
   
10.	//block-thread 2D-1D
int blockId_2D = blockIdx.x + blockIdx.y*gridDim.x;
int i = threadIdx.x + blockDim.x*blockId_2D;
//uint3 s1; s1.x = 10; s1.y = 10; s1.z = 1;
//uint3 s2; s2.x = size / 100; s2.y = 1; s2.z = 1;
//testBlockThread4 << <s1, s2 >> >(dev_c, dev_a, dev_b);
11.	//block-thread 3D-1D
int blockId_3D = blockIdx.x + blockIdx.y*gridDim.x + blockIdx.z*gridDim.x*gridDim.y;
int i = threadIdx.x + blockDim.x*blockId_3D;
//uint3 s1; s1.x = 10; s1.y = 5; s1.z = 2;
//uint3 s2; s2.x = size / 100; s2.y = 1; s2.z = 1;
//testBlockThread5 << <s1, s2 >> >(dev_c, dev_a, dev_b);
12.	//block-thread 2D-2D
int threadId_2D = threadIdx.x + threadIdx.y*blockDim.x;
int blockId_2D = blockIdx.x + blockIdx.y*gridDim.x;
int i = threadId_2D + (blockDim.x*blockDim.y)*blockId_2D;
//uint3 s1; s1.x = size / 100; s1.y = 10; s1.z = 1;
//uint3 s2; s2.x = 5; s2.y = 2; s2.z = 1;
//testBlockThread6 << <s1, s2 >> >(dev_c, dev_a, dev_b);
    
13.	//block-thread 2D-3D
int threadId_3D = threadIdx.x + threadIdx.y*blockDim.x + threadIdx.z*blockDim.x*blockDim.y;
int blockId_2D = blockIdx.x + blockIdx.y*gridDim.x;
int i = threadId_3D + (blockDim.x*blockDim.y*blockDim.z)*blockId_2D;
//uint3 s1; s1.x = size / 100; s1.y = 5; s1.z = 1;
//uint3 s2; s2.x = 5; s2.y = 2; s2.z = 2;
//testBlockThread7 << <s1, s2 >> >(dev_c, dev_a, dev_b);
 
14.	//block-thread 3D-2D
int threadId_2D = threadIdx.x + threadIdx.y*blockDim.x;
int blockId_3D = blockIdx.x + blockIdx.y*gridDim.x + blockIdx.z*gridDim.x*gridDim.y;
int i = threadId_2D + (blockDim.x*blockDim.y)*blockId_3D;
//uint3 s1; s1.x = 5; s1.y = 2; s1.z = 2;
//uint3 s2; s2.x = size / 100; s2.y = 5; s2.z = 1;
//testBlockThread8 <<<s1, s2 >>>(dev_c, dev_a, dev_b);
    
15.	//block-thread 3D-3D
int threadId_3D = threadIdx.x + threadIdx.y*blockDim.x + threadIdx.z*blockDim.x*blockDim.y;
int blockId_3D = blockIdx.x + blockIdx.y*gridDim.x + blockIdx.z*gridDim.x*gridDim.y;
int i = threadId_3D + (blockDim.x*blockDim.y*blockDim.z)*blockId_3D;    
//uint3 s1; s1.x = 5; s1.y = 2; s1.z = 2;
//uint3 s2; s2.x = size / 200; s2.y = 5; s2.z = 2;
//testBlockThread9<<<s1, s2 >>>(dev_c, dev_a, dev_b);
```
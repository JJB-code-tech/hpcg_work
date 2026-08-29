# ASC选拔作业‑基础题 HPCG
姓名：吉延生
基础题：HPCG

## 运行环境
宿主系统：Windows11
运行环境：WSL2 Ubuntu
MPI+OpenMP 并行，HPCG‑3.1版本
算例参数：--nx=16

## 复现步骤
进入hpcg/bin目录执行下面三组实验命令，time统计wall‑clock实际运行时间。

### 运行命令
```bash
#组1 Baseline
time OMP_NUM_THREADS=1 mpirun -np 4 ./xhpcg --nx=16 2>&1

#组2 增加OMP线程
time OMP_NUM_THREADS=2 mpirun -np 4 ./xhpcg --nx=16 2>&1

#组3 调整MPI进程
time OMP_NUM_THREADS=2 mpirun -np 2 ./xhpcg --nx=16 2>&1

#三组的真实时间
组 1：4MPI，1OMP → real=0.630s
组 2：4MPI，2OMP → real=0.367s
组 3：2MPI，2OMP → real=0.375s
1、不同配置性能对比
组 1→组 2：MPI 进程不变，每个进程 OpenMP 线程从 1 改成 2。运行时间从 0.630s 下降到 0.367s，速度变快。增加 OpenMP 线程带来性能提升。
组 2→组 3：OMP 线程不变，MPI 进程从 4 改成 2。运行时间从 0.367s 上涨到 0.375s，速度稍微变慢。减少 MPI 进程，性能小幅下降。
2、哪一组效果最好
组 2 配置（4 个 MPI 进程，每个进程 2 个 OpenMP 线程，OMP_NUM_THREADS=2 mpirun -np 4）综合性能最好，运行时间最短。

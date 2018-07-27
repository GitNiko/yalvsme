配送最优化
====================
## 场景
分解成为为两个问题：
- 路径最优
- 配送组合最优

## keyword
- Dynamic Vehicle Routing Problem
- 节约里程法 
- VRP
- s

## 实验策略
短时间内可以通过遍历的方式找出最优的结果的数据集，通过该数据集来验证算法偏差率。

## P(Deterministic Polynomial Time)
>Deterministic Polynomial Time可以解決的decision problem  

如果一个问题可以找到一个能在多项式的时间里解决它的算法，那么这个问题就属于P问题

## NP
> Nondeterministic Polynomial Time可以解決的decision problem 

找一个解很困难，但验证一个解很容易。  
可以在多项式的时间里验证一个解的问题

## P与NP
P是NP的子集

## NPC
> Nondeterministic Polynomial Complete  
> definition:
> A descision problem c is NP-complete if:
> 1.  c is in NP, and
> 2. Every problem in NP is reducible to c in polynomial time

证明方式:先证明它至少是一个NP问题，再证明其中一个已知的NPC问题能约化到它（由约化的传递性，则NPC问题定义的第二条也得以满足；

## SAT(Boolean Satisfiability Problem)
第一個 NP-Complete problem


## NP-hardness
h is NP-hardness if every problem in NP is reducible to h in polynomial time

## 资料
https://tech.meituan.com/O2O_Intelligent_distribution.html
http://www.jumapeisong.com/industry-knowledge/94.html
[节约里程](http://wiki.mbalib.com/zh-tw/%E8%8A%82%E7%BA%A6%E9%87%8C%E7%A8%8B%E6%B3%95)
[VRP](http://wiki.mbalib.com/zh-tw/VRP%E9%97%AE%E9%A2%98)
[NP](http://www.matrix67.com/blog/archives/105)
[NP,NPC,NPH关系](https://cg2010studio.com/2011/05/27/npc-problem/)
[SAT](https://willyc20.github.io/2016/12/17/sat-problem-1/)
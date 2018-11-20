---
title: 算法_00002_Dijkstra
date: 2018-11-19 18:29:49
categories: [技术]
tags: [算法]
---

### 介绍
* **Dijkstra算法**是由荷兰计算机科学家[狄克斯特拉](http://wiki.mbalib.com/w/index.php?title=%E7%8B%84%E5%85%8B%E6%96%AF%E7%89%B9%E6%8B%89&action=edit)（[Dijkstra](http://wiki.mbalib.com/wiki/Dijkstra)）于1959 年提出的，因此又叫狄克斯特拉算法。是从一个顶点到其余各顶点的最短路径算法，解决的是有向图中最短路径问题。


### 思路
![Dijkstra算法图(来源:见文章结尾参考链接)](http://upload-images.jianshu.io/upload_images/2062311-926a6bae20861cb3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 案例
![邻接矩阵(来源:见文章结尾参考链接)](http://upload-images.jianshu.io/upload_images/2062311-7799e871aa4e3116.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 以起点D开始的演算过程
![Dijkstra算法推演过程(来源:见文章结尾参考链接)](http://upload-images.jianshu.io/upload_images/2062311-30b98cc4326939a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 运行结果
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━
初始化原始点到点距离矩阵:
━━━━━━━━━━━━━━━━━━━━━━━━━━━
     ┃     A     B     C     D     E     F     G
━━━━━━━━━━━━━━━━━━━━━━━━━━━
  A  ┃     0    12   INF   INF   INF    16    14
  B  ┃    12     0    10   INF   INF     7   INF
  C  ┃   INF    10     0     3     5     6   INF
  D  ┃   INF   INF     3     0     4   INF   INF
  E  ┃   INF   INF     5     4     0     2     8
  F  ┃    16     7     6   INF     2     0     9
  G  ┃    14   INF   INF   INF     8     9     0
━━━━━━━━━━━━━━━━━━━━━━━━━━━
初始化起点[D]到各顶点的距离:
[D->A]的距离(INF)
[D->B]的距离(INF)
[D->C]的距离(  3)
[D->D]的距离(  0)
[D->E]的距离(  4)
[D->F]的距离(INF)
[D->G]的距离(INF)
━━━━━━━━━━━━━━━━━━━━━━━━━━━
开始执行 Dijkstra 算法:
━━━━━━━━━━━━━━━━━━━━━━━━━━━
标记[D]不参与下次循环
━━━━━━━━━━━━━━━━━━━━━━━━━━━
比较起点[D]到未被标记过的各顶点的距离:
[D->A]的距离(INF)
[D->B]的距离(INF)
[D->C]的距离(  3)
[D->E]的距离(  4)
[D->F]的距离(INF)
[D->G]的距离(INF)
得出当前[D->C]的距离(  3)最短,标记[C]不参与下次循环,并计算判断当起点[D]在经过中间点[C]的情况下到达未被标记过的各顶点的距离是否变短,如果变短则更新最短距离):
[D->C]的距离(  3) + [C->A]的距离(INF)	得出[D->C->A]的距离(INF) ≥ [D->A]的累积计算的最短距离(INF)	得出[D->A]的最短距离(INF)
[D->C]的距离(  3) + [C->B]的距离( 10)	得出[D->C->B]的距离( 13) <  [D->B]的累积计算的最短距离(INF)	得出[D->B]的最短距离( 13)
[D->C]的距离(  3) + [C->E]的距离(  5)	得出[D->C->E]的距离(  8) ≥ [D->E]的累积计算的最短距离(  4)	得出[D->E]的最短距离(  4)
[D->C]的距离(  3) + [C->F]的距离(  6)	得出[D->C->F]的距离(  9) <  [D->F]的累积计算的最短距离(INF)	得出[D->F]的最短距离(  9)
[D->C]的距离(  3) + [C->G]的距离(INF)	得出[D->C->G]的距离(INF) ≥ [D->G]的累积计算的最短距离(INF)	得出[D->G]的最短距离(INF)
━━━━━━━━━━━━━━━━━━━━━━━━━━━
比较起点[D]到未被标记过的各顶点的距离:
[D->A]的距离(INF)
[D->B]的距离( 13)
[D->E]的距离(  4)
[D->F]的距离(  9)
[D->G]的距离(INF)
得出当前[D->E]的距离(  4)最短,标记[E]不参与下次循环,并计算判断当起点[D]在经过中间点[E]的情况下到达未被标记过的各顶点的距离是否变短,如果变短则更新最短距离):
[D->E]的距离(  4) + [E->A]的距离(INF)	得出[D->E->A]的距离(INF) ≥ [D->A]的累积计算的最短距离(INF)	得出[D->A]的最短距离(INF)
[D->E]的距离(  4) + [E->B]的距离(INF)	得出[D->E->B]的距离(INF) ≥ [D->B]的累积计算的最短距离( 13)	得出[D->B]的最短距离( 13)
[D->E]的距离(  4) + [E->F]的距离(  2)	得出[D->E->F]的距离(  6) <  [D->F]的累积计算的最短距离(  9)	得出[D->F]的最短距离(  6)
[D->E]的距离(  4) + [E->G]的距离(  8)	得出[D->E->G]的距离( 12) <  [D->G]的累积计算的最短距离(INF)	得出[D->G]的最短距离( 12)
━━━━━━━━━━━━━━━━━━━━━━━━━━━
比较起点[D]到未被标记过的各顶点的距离:
[D->A]的距离(INF)
[D->B]的距离( 13)
[D->F]的距离(  6)
[D->G]的距离( 12)
得出当前[D->F]的距离(  6)最短,标记[F]不参与下次循环,并计算判断当起点[D]在经过中间点[F]的情况下到达未被标记过的各顶点的距离是否变短,如果变短则更新最短距离):
[D->F]的距离(  6) + [F->A]的距离( 16)	得出[D->F->A]的距离( 22) <  [D->A]的累积计算的最短距离(INF)	得出[D->A]的最短距离( 22)
[D->F]的距离(  6) + [F->B]的距离(  7)	得出[D->F->B]的距离( 13) ≥ [D->B]的累积计算的最短距离( 13)	得出[D->B]的最短距离( 13)
[D->F]的距离(  6) + [F->G]的距离(  9)	得出[D->F->G]的距离( 15) ≥ [D->G]的累积计算的最短距离( 12)	得出[D->G]的最短距离( 12)
━━━━━━━━━━━━━━━━━━━━━━━━━━━
比较起点[D]到未被标记过的各顶点的距离:
[D->A]的距离( 22)
[D->B]的距离( 13)
[D->G]的距离( 12)
得出当前[D->G]的距离( 12)最短,标记[G]不参与下次循环,并计算判断当起点[D]在经过中间点[G]的情况下到达未被标记过的各顶点的距离是否变短,如果变短则更新最短距离):
[D->G]的距离( 12) + [G->A]的距离( 14)	得出[D->G->A]的距离( 26) ≥ [D->A]的累积计算的最短距离( 22)	得出[D->A]的最短距离( 22)
[D->G]的距离( 12) + [G->B]的距离(INF)	得出[D->G->B]的距离(INF) ≥ [D->B]的累积计算的最短距离( 13)	得出[D->B]的最短距离( 13)
━━━━━━━━━━━━━━━━━━━━━━━━━━━
比较起点[D]到未被标记过的各顶点的距离:
[D->A]的距离( 22)
[D->B]的距离( 13)
得出当前[D->B]的距离( 13)最短,标记[B]不参与下次循环,并计算判断当起点[D]在经过中间点[B]的情况下到达未被标记过的各顶点的距离是否变短,如果变短则更新最短距离):
[D->B]的距离( 13) + [B->A]的距离( 12)	得出[D->B->A]的距离( 25) ≥ [D->A]的累积计算的最短距离( 22)	得出[D->A]的最短距离( 22)
━━━━━━━━━━━━━━━━━━━━━━━━━━━
比较起点[D]到未被标记过的各顶点的距离:
[D->A]的距离( 22)
得出当前[D->A]的距离( 22)最短,标记[A]不参与下次循环,并计算判断当起点[D]在经过中间点[A]的情况下到达未被标记过的各顶点的距离是否变短,如果变短则更新最短距离):
━━━━━━━━━━━━━━━━━━━━━━━━━━━
经过 Dijkstra 算法处理之后D到其他节点的最短路径:
━━━━━━━━━━━━━━━━━━━━━━━━━━━
[D->A]的最短路径为	[D->E->F->A]        距离: 22
[D->B]的最短路径为	[D->C->B]           距离: 13
[D->C]的最短路径为	[D->C]              距离:  3
[D->D]的最短路径为	[D->D]              距离:  0
[D->E]的最短路径为	[D->E]              距离:  4
[D->F]的最短路径为	[D->E->F]           距离:  6
[D->G]的最短路径为	[D->E->G]           距离: 12
━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### 核心算法
```
package krmao.algorithm.dijkstra;

import java.util.ArrayDeque;
import java.util.Queue;

import krmao.algorithm.common.Vocabulary;

import static krmao.algorithm.common.MatrixUtil.INF;

/**
 * 一些思路
 */
public class Dijkstra {

    public static void calculateByDijkstraAlgorithm(int startIndex, final int[][] distanceMatrix, final int[] newStartToNodeDistance, final int pathPreNodeIndex[]) {
        Queue<Integer> markedQueue = new ArrayDeque<Integer>();
        System.out.printf("标记[%s]不参与下次循环\n", Vocabulary.charAt(startIndex));
        markedQueue.add(startIndex);
        System.out.println("━━━━━━━━━━━━━━━━━━━━━━━━━━━");
        while (markedQueue.size() < distanceMatrix.length) {//全部标记完,则循环结束
            System.out.printf("比较起点[%s]到未被标记过的各顶点的距离:\n", Vocabulary.charAt(startIndex));
            int minDistance = INF;
            int minDistanceIndex = INF;
            for (int j = 0; j < distanceMatrix.length; j++) {
                if (!markedQueue.contains(j)) {
                    System.out.printf("[%c->%c]的距离(%3s)\n", Vocabulary.charAt(startIndex), Vocabulary.charAt(j), newStartToNodeDistance[j] == INF ? "INF" : newStartToNodeDistance[j]);
                    if (newStartToNodeDistance[j] != INF && (minDistance == INF || newStartToNodeDistance[j] < minDistance)) {
                        minDistance = newStartToNodeDistance[j];
                        minDistanceIndex = j;
                    }
                }
            }
            if (minDistanceIndex != INF && newStartToNodeDistance[minDistanceIndex] != INF) {
                System.out.printf("得出当前[%c->%c]的距离(%3d)最短,标记[%s]不参与下次循环,并计算判断当起点[%s]在经过中间点[%s]的情况下到达未被标记过的各顶点的距离是否变短,如果变短则更新最短距离):\n", Vocabulary.charAt(startIndex), Vocabulary.charAt(minDistanceIndex), minDistance, Vocabulary.charAt(minDistanceIndex), Vocabulary.charAt(startIndex), Vocabulary.charAt(minDistanceIndex));
                markedQueue.add(minDistanceIndex);

                for (int currentThirdIndex = 0; currentThirdIndex < distanceMatrix.length; currentThirdIndex++) {
                    if (!markedQueue.contains(currentThirdIndex)) {
                        int oldStartToThirdDistance = newStartToNodeDistance[currentThirdIndex];
                        if (distanceMatrix[currentThirdIndex][minDistanceIndex] != INF) {
                            if (newStartToNodeDistance[currentThirdIndex] == INF || newStartToNodeDistance[currentThirdIndex] > distanceMatrix[currentThirdIndex][minDistanceIndex] + newStartToNodeDistance[minDistanceIndex]) {
                                int startToThirdDistance = distanceMatrix[currentThirdIndex][minDistanceIndex] + newStartToNodeDistance[minDistanceIndex];
                                printLog(startIndex, minDistanceIndex, newStartToNodeDistance[minDistanceIndex], currentThirdIndex, distanceMatrix[currentThirdIndex][minDistanceIndex], oldStartToThirdDistance, startToThirdDistance, " <  ");
                                newStartToNodeDistance[currentThirdIndex] = startToThirdDistance;
                                pathPreNodeIndex[currentThirdIndex] = minDistanceIndex;
                            } else
                                printLog(startIndex, minDistanceIndex, newStartToNodeDistance[minDistanceIndex], currentThirdIndex, distanceMatrix[currentThirdIndex][minDistanceIndex], oldStartToThirdDistance, newStartToNodeDistance[currentThirdIndex], " ≥ ");
                        } else
                            printLog(startIndex, minDistanceIndex, newStartToNodeDistance[minDistanceIndex], currentThirdIndex, distanceMatrix[currentThirdIndex][minDistanceIndex], oldStartToThirdDistance, newStartToNodeDistance[currentThirdIndex], " ≥ ");
                    }
                }
                System.out.println("━━━━━━━━━━━━━━━━━━━━━━━━━━━");
            }
        }
    }

    private static void processPath(StringBuffer pathBuffer, final int pathPreNodeIndex[], int index) {
        if (pathPreNodeIndex[index] != INF) {
            processPath(pathBuffer, pathPreNodeIndex, pathPreNodeIndex[index]);
            pathBuffer.append(Vocabulary.charAt(pathPreNodeIndex[index])).append("->");
        }
    }

    public static void printPath(int startIndex, int matrixLength, int[] newStartToNodeDistance, final int pathPreNodeIndex[]) {
        System.out.println("━━━━━━━━━━━━━━━━━━━━━━━━━━━");
        for (int i = 0; i < matrixLength; i++) {
            StringBuffer pathBuffer = new StringBuffer("[").append(Vocabulary.charAt(startIndex)).append("->");
            processPath(pathBuffer, pathPreNodeIndex, i);
            pathBuffer.append(Vocabulary.charAt(i)).append("]");
            System.out.printf("[%c->%c]的最短路径为\t%-20s距离:%3d\n", Vocabulary.charAt(startIndex), Vocabulary.charAt(i), pathBuffer, newStartToNodeDistance[i]);
        }
        System.out.println("━━━━━━━━━━━━━━━━━━━━━━━━━━━");
    }

    public static void printLog(int startIndex, int currentMinDistanceIndex, int startToMinDistance, int currentThirdIndex, int minToThirdDistance, int oldStartToThirdDistance, int newStartToThirdDistance, String symbol) {
        int tmpStartToMinToThirdDistance = startToMinDistance == INF || minToThirdDistance == INF ? INF : startToMinDistance + minToThirdDistance;
        String tmpStartToMinToThirdDistanceStr = tmpStartToMinToThirdDistance == INF ? "INF" : String.valueOf(tmpStartToMinToThirdDistance);
        System.out.printf("[%c->%c]的距离(%3s) + [%c->%c]的距离(%3s)"
                        + "\t得出[%c->%c->%c]的距离(%3s)%s[%c->%c]的累积计算的最短距离(%3s)"
                        + "\t得出[%c->%c]的最短距离(%3s)\n",
                Vocabulary.charAt(startIndex), Vocabulary.charAt(currentMinDistanceIndex), startToMinDistance == INF ? "INF" : startToMinDistance, Vocabulary.charAt(currentMinDistanceIndex), Vocabulary.charAt(currentThirdIndex), minToThirdDistance == INF ? "INF" : minToThirdDistance
                , Vocabulary.charAt(startIndex), Vocabulary.charAt(currentMinDistanceIndex), Vocabulary.charAt(currentThirdIndex), tmpStartToMinToThirdDistanceStr, symbol, Vocabulary.charAt(startIndex), Vocabulary.charAt(currentThirdIndex), oldStartToThirdDistance == INF ? "INF" : oldStartToThirdDistance
                , Vocabulary.charAt(startIndex), Vocabulary.charAt(currentThirdIndex), newStartToThirdDistance == INF ? "INF" : newStartToThirdDistance
        );
    }
}

```

###单元测试
```
package krmao.algorithm.dijkstra.test;

import org.junit.Test;

import krmao.algorithm.common.MatrixUtil;
import krmao.algorithm.common.Vocabulary;

import static krmao.algorithm.floyed.Floyed.INF;

public class DijkstraTest {

    @Test
    public void testDijkstra() {
        int originalMatrix[][] = new int[][]{
             /*A*//*B*//*C*//*D*//*E*//*F*//*G*/
      /*A*/ {0, 12, INF, INF, INF, 16, 14},
      /*B*/ {12, 0, 10, INF, INF, 7, INF},
      /*C*/ {INF, 10, 0, 3, 5, 6, INF},
      /*D*/ {INF, INF, 3, 0, 4, INF, INF},
      /*E*/ {INF, INF, 5, 4, 0, 2, 8},
      /*F*/ {16, 7, 6, INF, 2, 0, 9},
      /*G*/ {14, INF, INF, INF, 8, 9, 0}
        };
        int pathPreNodeIndex[] = new int[originalMatrix.length];
        int newStartToNodeDistance[] = new int[originalMatrix.length];
        char startNode = 'D';
        int startIndex = Vocabulary.indexFrom(startNode);

        System.out.println("━━━━━━━━━━━━━━━━━━━━━━━━━━━");
        System.out.println("初始化原始点到点距离矩阵:");
        MatrixUtil.printMatrix(originalMatrix);
        System.out.printf("初始化起点[%s]到各顶点的距离:\n", Vocabulary.charAt(startIndex));
        System.arraycopy(originalMatrix[startIndex], 0, newStartToNodeDistance, 0, originalMatrix.length);
        for (int j = 0; j < originalMatrix.length; j++) {
            pathPreNodeIndex[j] = INF;//借着这个循环初始化前向节点
            System.out.printf("[%c->%c]的距离(%3s)\n", Vocabulary.charAt(startIndex), Vocabulary.charAt(j), newStartToNodeDistance[j] == INF ? "INF" : newStartToNodeDistance[j]);
        }
        System.out.println("━━━━━━━━━━━━━━━━━━━━━━━━━━━");
        System.out.println("开始执行 Dijkstra 算法:");
        System.out.println("━━━━━━━━━━━━━━━━━━━━━━━━━━━");
        krmao.algorithm.dijkstra.Dijkstra.calculateByDijkstraAlgorithm(startIndex, originalMatrix, newStartToNodeDistance, pathPreNodeIndex);
        System.out.printf("经过 Dijkstra 算法处理之后%c到其他节点的最短路径:\n", startNode);
        krmao.algorithm.dijkstra.Dijkstra.printPath(startIndex, originalMatrix.length, newStartToNodeDistance, pathPreNodeIndex);
    }
}
```

### 涉及到的工具类
[Vocabulary](http://www.jianshu.com/p/a6f34f5f8cfa)
[MatrixUtil](http://www.jianshu.com/p/a6f34f5f8cfa)

### 参考
[http://www.cnblogs.com/skywang12345/p/3711512.html?utm_source=tuicool&utm_medium=referral](http://www.cnblogs.com/skywang12345/p/3711512.html?utm_source=tuicool&utm_medium=referral)
[http://blog.sina.com.cn/s/blog_8fe28e630102wwye.html](http://blog.sina.com.cn/s/blog_8fe28e630102wwye.html)
[http://wiki.mbalib.com/wiki/Dijkstra%E7%AE%97%E6%B3%95](http://wiki.mbalib.com/wiki/Dijkstra%E7%AE%97%E6%B3%95)

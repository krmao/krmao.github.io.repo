---
title: 算法_00001_Floyed
date: 2018-11-19 18:29:49
categories: [技术]
tags: [算法]
---

### 介绍
* 利用动态规划的思想寻找给定的加权图中多源点之间最短路径的算法


### 思路
 * 遍历所有地点(一层循环),判断该地点是否可以让 **任意两地(这里就需要另外两层循环)** 的距离更近,如果可以,记录下这个更短的距离,顺便保存路径
 * 保存路径则按照保存第三地点名称的思路,假如AB之间有C使之更近,则PATH[A][B]=C;而其它循环自然会处理PATH[A][C],PATH[C][B]的问题,**==INF**则表示再无第三地点使之更近
 * 输出路径则按照递归的思想,直到两个地点之间再无第三地点使之更近,递归结束

### 案例

![邻接矩阵.png](http://upload-images.jianshu.io/upload_images/2062311-7dcfdf27226f5f4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 运行结果

结果如下

```
	初始化原始点到点距离矩阵:
--------------------------------------------------
     0     2     6     4
    -1     0     3    -1
     7    -1     0     1
     5    -1    12     0
--------------------------------------------------
	经过 Floyed 算法处理之后的点到点最短距离矩阵:
--------------------------------------------------
     0     2     5     4
     9     0     3     4
     6     8     0     1
     5     7    10     0
--------------------------------------------------
	[0->0] 的最短路径为 :(0->0)
	[0->1] 的最短路径为 :(0->1)
	[0->2] 的最短路径为 :(0->1->2)
	[0->3] 的最短路径为 :(0->3)
	[1->0] 的最短路径为 :(1->2->0)
	[1->1] 的最短路径为 :(1->1)
	[1->2] 的最短路径为 :(1->2)
	[1->3] 的最短路径为 :(1->2->3)
	[2->0] 的最短路径为 :(2->3->0)
	[2->1] 的最短路径为 :(2->3->0->1)
	[2->2] 的最短路径为 :(2->2)
	[2->3] 的最短路径为 :(2->3)
	[3->0] 的最短路径为 :(3->0)
	[3->1] 的最短路径为 :(3->0->1)
	[3->2] 的最短路径为 :(3->0->2)
	[3->3] 的最短路径为 :(3->3)
--------------------------------------------------
```

### 核心算法

```
package krmao.algorithm.floyed;

public class Floyed {
    public static final int INF = -1;//无穷大/不可达

    public static void calculateByFloyedAlgorithm(final int[][] distanceMatrix, final int pathMatrix[][]) {
        if (distanceMatrix != null) {
            for (int k = 0; k < distanceMatrix.length; k++)
                for (int i = 0; i < distanceMatrix.length; i++)
                    for (int j = 0; j < distanceMatrix.length; j++) {
                        if (distanceMatrix[i][k] != Floyed.INF && distanceMatrix[k][j] != Floyed.INF && (distanceMatrix[i][j] == -1 || (distanceMatrix[i][j] > distanceMatrix[i][k] + distanceMatrix[k][j]))) {
                            distanceMatrix[i][j] = distanceMatrix[i][k] + distanceMatrix[k][j];
                            if (pathMatrix[i][j] == INF)
                                pathMatrix[i][j] = k;
                        }
                    }
        }
    }

    public static void printPath(final int[][] distanceMatrix, final int pathMatrix[][]) {
        if (pathMatrix != null && distanceMatrix != null) {
            for (int i = 0; i < pathMatrix.length; i++) {
                for (int j = 0; j < pathMatrix.length; j++) {
                    System.out.print("\t[" + i + "->" + j + "] 的最短路径为 :");
                    if (distanceMatrix[i][j] == INF) {
                        System.out.println("(不可达)");
                    } else {
                        System.out.print("(" + i + "->");
                        printPath(pathMatrix, i, j);
                        System.out.println(j + ")");
                    }
                }
            }
        }
    }

    private static void printPath(int path[][], int i, int j) {//递归输出路径
        if (path != null && path[i][j] != INF) {
            printPath(path, i, path[i][j]);
            System.out.printf("%d->", path[i][j]);
        }
    }

    public static void printMatrix(final int[][] originalMatrix) {
        if (originalMatrix != null) {
            for (int[] itemMatrix : originalMatrix) {
                for (int itemValue : itemMatrix)
                    System.out.printf("%6d", itemValue);
                System.out.println();
            }
        }
    }
}

```

### 单元测试
```
package krmao.algorithm.floyed.test;

import org.junit.Test;

import static krmao.algorithm.floyed.Floyed.INF;

public class FloyedTest {
    @Test
    public void testFloyed() {
        int originalMatrix[][] = new int[][]{
                {0, 2, 6, 4},
                {INF, 0, 3, INF},
                {7, INF, 0, 1},
                {5, INF, 12, 0}
        };

        //记录点到点之间 经过哪一个点(保存这个点的名字) 使得距离更近,初始值INF表示不经过别的点中转
        int pathMatrix[][] = new int[][]{
                {INF, INF, INF, INF},
                {INF, INF, INF, INF},
                {INF, INF, INF, INF},
                {INF, INF, INF, INF}
        };

        System.out.println("--------------------------------------------------");
        System.out.println("\t初始化原始点到点距离矩阵:");
        System.out.println("--------------------------------------------------");
        krmao.algorithm.floyed.Floyed.printMatrix(originalMatrix);
        krmao.algorithm.floyed.Floyed.calculateByFloyedAlgorithm(originalMatrix, pathMatrix);
        System.out.println("--------------------------------------------------");
        System.out.println("\t经过 Floyed 算法处理之后的点到点最短距离矩阵:");
        System.out.println("--------------------------------------------------");
        krmao.algorithm.floyed.Floyed.printMatrix(originalMatrix);
        System.out.println("--------------------------------------------------");
        krmao.algorithm.floyed.Floyed.printPath(originalMatrix, pathMatrix);
        System.out.println("--------------------------------------------------");
    }
}
```

### 参考
[http://developer.51cto.com/art/201403/433874.htm](http://developer.51cto.com/art/201403/433874.htm)
[http://www.cnblogs.com/skywang12345/p/3711532.html](http://www.cnblogs.com/skywang12345/p/3711532.html)

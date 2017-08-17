---
layout: post
category: "data"
title:  "基础数据结构和算法之归并排序"
tags: [算法,数据结构]
---
### 简介

>归并排序分为递归方法和循环处理方法，不建议使用递归，递归对程序堆栈不有好。   


### 实现思路
```
1、循环实现：
2、递归实现：
	分而治之
```

### 时间复杂度 
归并排序一般在外排序的时候使用，需要额外的空间
O(N)

### 代码
```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

/* L = 左边起始位置, R = 右边起始位置, RightEnd = 右边终点位置*/
void Merge( int A[], int TmpA[], int L, int R, int RightEnd )
{ /* 将有序的A[L]~A[R-1]和A[R]~A[RightEnd]归并成一个有序序列 */
     int LeftEnd, NumElements, Tmp;
     int i;
      
     LeftEnd = R - 1; /* 左边终点位置 */
     Tmp = L;         /* 有序序列的起始位置 */
     NumElements = RightEnd - L + 1;
      
     while( L <= LeftEnd && R <= RightEnd ) {
         if ( A[L] <= A[R] )
             TmpA[Tmp++] = A[L++]; /* 将左边元素复制到TmpA */
         else
             TmpA[Tmp++] = A[R++]; /* 将右边元素复制到TmpA */
     }
 
     while( L <= LeftEnd )
         TmpA[Tmp++] = A[L++]; /* 直接复制左边剩下的 */
     while( R <= RightEnd )
         TmpA[Tmp++] = A[R++]; /* 直接复制右边剩下的 */
          
     for( i = 0; i < NumElements; i++, RightEnd -- )
         A[RightEnd] = TmpA[RightEnd]; /* 将有序的TmpA[]复制回A[] */
}

void Msort( int A[], int TmpA[], int L, int RightEnd )
{ /* 核心递归排序函数 */ 
     int Center;

	 printf("L=%d,RightEnd=%d\n", L, RightEnd);
     if ( L < RightEnd ) {
          Center = (L+RightEnd) / 2;
		  printf("L=%d,Center=%d\n", L, Center);
          Msort( A, TmpA, L, Center );              /* 递归解决左边 */ 
		  printf("### L=%d,Center=%d,RightEnd=%d\n", L, Center, RightEnd);
          Msort( A, TmpA, Center+1, RightEnd );     /* 递归解决右边 */  
		  printf("*** L=%d,Center=%d,RightEnd=%d\n", L, Center, RightEnd);
          Merge( A, TmpA, L, Center+1, RightEnd );  /* 合并两段有序序列 */
		  printf("~~~ L=%d,Center=%d,RightEnd=%d\n", L, Center, RightEnd);
     }
}

void MergeSort( int A[], int N )
{ /* 归并排序 - 递归实现 */
     int *TmpA;
     TmpA = (int *)malloc(N*sizeof(int));
      
     if ( TmpA != NULL ) {
          Msort( A, TmpA, 0, N-1 );
          free( TmpA );
     }
     else printf( "空间不足" );
}


/* 归并排序 - 循环实现 */
/* length = 当前有序子列的长度*/
void Merge_pass( int A[], int TmpA[], int N, int length )
{ /* 两两归并相邻有序子列 */
     int i, j, k;
       
     for ( i=0; i <= N-2*length; i += 2*length )
         Merge( A, TmpA, i, i+length, i+2*length-1 );
	 for(k = 0; k < N; k++) {
		printf("\r\nA[%d] = %d\r\n", k, A[k]);
	 }
	 printf("i = %d, length = %d\n", i, length);
     if ( i+length < N ) /* 归并最后2个子列*/
         Merge( A, TmpA, i, i+length, N-1);
     else /* 最后只剩1个子列*/
         for ( j = i; j < N; j++ ) {
		 	printf("==========\n");
		 	TmpA[j] = A[j];
         }
}
 
void Merge_Sort( int A[], int N )
{ 
     int length, i; 
     int *TmpA;
      
     length = 1; /* 初始化子序列长度*/
     TmpA = malloc( N * sizeof( int ) );
     if ( TmpA != NULL ) {
          while( length < N ) {
              Merge_pass( A, TmpA, N, length );
              length *= 2;
			  for(i = 0; i< N; i++) {
				printf("A[%d] = %d\n", i, A[i]);
				printf("TmpA[%d] = %d\n", i , TmpA[i]);
	 		  }
			  printf("N = %d, length = %d\n", N, length);
              Merge_pass( TmpA, A, N, length );
              length *= 2;
          }
          free( TmpA );
     }
     else printf( "空间不足" );
}

int main()
{
	/* 归并排序 */
     int A[15] = {2,3,4,12,5,89,100,32,45,11,65,78,34,109,9};
	 int N=15;
	 int i ,j;
	 
	 for(i = 0; i< N; i++) {
		printf("A[%d] = %d\n", i, A[i]);
	 }

	 //MergeSort(A, N);  /*递归实现*/
	 Merge_Sort(A, N);  /*循环实现*/

	 for(i = 0; i< N; i++) {
		printf("A[%d] = %d\n", i, A[i]);
	 }
	 
	return 1;
}
```

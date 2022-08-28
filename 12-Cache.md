---
permalink: /computer_arch/2/12-Cache
title: "Cache Memory"
author_profile: true

header:
  image: "/images/austin/austin2.jpg"

toc: true
toc_label: "Table"
---


## Why is **Cache** local Memory Important?

When you access memory, a chunks of it are *cached at various levels* (L1, L2, L3). Cache locality refers to the *likelihood of successive operations* being in the cache and thus being *faster*. In an array, you maximize the chances of sequential element access being in the cache.

!["insert"](/images/arch/12/cache1.png)
!["insert"](/images/arch/12/cache2.png)
!["insert"](/images/arch/12/cache3.png)
!["insert"](/images/arch/12/cache4.png)


## 1-D Array Example

```c
#include <stdio.h>

int sum_1darray(int a[], int size) {
    int sum = 0;
    int i;
    for (i = 0; i < size; ++i) {
        printf("%p: a[%d] = %d\n", &a[i], i, a[i]);
        sum += a[i];
    }
    return sum;
}

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    int sum = sum_1darray(arr, 5);
    printf("sum = %d\n", sum);
    return 0;
}
```


**Output:**

```c
0x16d43f280: a[0] = 1
0x16d43f284: a[1] = 2
0x16d43f288: a[2] = 3
0x16d43f28c: a[3] = 4
0x16d43f290: a[4] = 5
sum = 15
```

Read sequentially in order since its a `1-D Array`


## 2-D Array Cache Example (row)


```c
#include <stdio.h>

int sum_2darray_rowwise(int a[3][3], int rows, int cols) {
    int sum = 0;
    int i, j;
    for (i = 0; i < rows; ++i) {
        for (j = 0; j < cols; ++j) {
            printf("%p: a[%d][%d] = %d\n", &a[i][j], i, j, a[i][j]);
            sum += a[i][j];
        }
    }
    return sum;
}

int main() {
    int arr[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    int sum = sum_2darray_rowwise(arr, 3, 3);
    printf("sum = %d\n", sum);
    return 0;
}
```


**Output:**

```bash
0x16d60f274: a[0][0] = 1
0x16d60f278: a[0][1] = 2
0x16d60f27c: a[0][2] = 3
0x16d60f280: a[1][0] = 4
0x16d60f284: a[1][1] = 5
0x16d60f288: a[1][2] = 6
0x16d60f28c: a[2][0] = 7
0x16d60f290: a[2][1] = 8
0x16d60f294: a[2][2] = 9
sum = 45
```

Hit or Miss Cache for Row-Oriented:

| a[i][j] | j = 0        |  j = 2      | j = 3       | j = 4       |
|---------|--------------|-------------|-------------|-------------|
| i = 0   |  w[0] (miss) |  w[1] (hit) |  w[2] (hit) |  w[3] (hit) |
| i = 1   |  w[4] (miss) |  w[5] (hit) |  w[6] (hit) |  w[7] (hit) |
| i = 2   |  w[8] (miss) |  w[9] (hit) | w[10] (hit) | w[11] (hit) |
| i = 3   | w[12] (miss) | w[13] (hit) | w[14] (hit) | w[15] (hit) |



## 2-D Array (Column Oriented)

https://www.geeksforgeeks.org/computer-organization-locality-and-cache-friendly-code/


+++
author = "xinyu"
title = "C/C++常见算法排序的实现"
date = "2024-03-18"
description = "常见算法排序"
categories = [
    "算法","排序"
]
tags = [
    "算法","排序","C","C++"
]
image = "1.jpg"

+++

![](2.jpg)

以下是使用C语言实现冒泡排序、插入排序、选择排序、快速排序、归并排序和堆排序的示例代码：

```c
#include <stdio.h>

// 冒泡排序
/*通过不断比较相邻元素并交换位置来实现排序。这个算法的基本思想是将较大(或较小)的元素逐渐“冒泡”到数组的末尾。
* 冒泡排序的步骤如下：
* 1. 从数组的第一个元素开始，依次比较相邻的两个元素。
* 2. 如果前一个元素大于后一个元素，就交换它们的位置，将较大的元素向后移动。
* 3. 继续向后比较相邻元素，重复上述操作，直到将最大的元素冒泡到数组的末尾。
* 4. 重复执行以上步骤，每次都将未排序部分的最大元素冒泡到末尾，直到整个数组排序完成。*/
void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}


// 插入排序
/*将数组分为已排序和未排序两部分，每次从未排序部分取出一个元素，插入到已排序部分的合适位置。
* 插入排序的步骤如下：
* 1. 从数组的第二个元素开始，将其视为已排序部分。
* 2. 取出未排序部分的第一个元素，将其与已排序部分的元素从后往前依次比较。
* 3. 如果已排序部分的元素大于取出的元素，就将已排序部分的元素后移一位，为取出的元素腾出插入位置。
* 4. 重复步骤3，直到找到已排序部分的合适位置，将取出的元素插入其中。
* 5. 重复步骤2到步骤4，直到未排序部分的所有元素都插入到已排序部分。*/
void insertionSort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}


// 选择排序
/*每次从未排序部分选择最小（或最大）的元素，然后将其与未排序部分的第一个元素交换位置，将其放入已排序部分的末尾。
* 选择排序的步骤如下：
* 1. 将数组分为已排序和未排序两部分，初始时已排序部分为空，未排序部分为整个数组。
* 2. 在未排序部分中找到最小（或最大）的元素。
* 3. 将最小（或最大）的元素与未排序部分的第一个元素交换位置，将其放入已排序部分的末尾。
* 4. 将未排序部分的起始位置向后移动一位，缩小未排序部分的范围。
* 5. 重复步骤2到步骤4，直到未排序部分为空，排序完成。*/
void selectionSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        int temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
}


// 快速排序
/*通过选择一个基准元素，将数组分割成两个子数组，其中一个子数组的所有元素都小于等于基准元素， \
*       另一个子数组的所有元素都大于基准元素，然后对这两个子数组分别进行递归排序。
* 快速排序的步骤如下：
* 1. 选择一个基准元素（通常是数组的最后一个元素）。
* 2. 设定两个指针，一个指向数组的起始位置，称为"low"，另一个指向数组的结束位置，称为"high"。
* 3. 通过一次遍历，将数组中小于等于基准元素的元素放在基准元素的左边，将大于基准元素的元素放在基准元素的右边。
*       这个过程称为"partition"，可以使用"partition"函数来实现。
* 4. 递归地对基准元素左边的子数组和右边的子数组进行排序。
* 5. 递归终止条件是子数组的长度为0或1，即无需排序。*/
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return i + 1;
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// 归并排序
/*归并排序是一种经典的排序算法，它采用分治的思想将一个数组分割成较小的子数组， \
*       然后对子数组进行排序，最后将排好序的子数组合并成一个整体有序的数组。
* 归并排序的步骤如下：
* 1. 分割：将待排序的数组分割成较小的子数组，直到每个子数组只有一个元素或为空。
*       计算数组的中间位置 mid = (left + right) / 2，其中 left 是数组的起始位置，right 是数组的结束位置。
*       递归地将左半部分数组进行分割：mergeSort(arr, left, mid)
*       递归地将右半部分数组进行分割：mergeSort(arr, mid+1, right)
* 2. 合并：将分割后的子数组进行合并，得到一个有序的数组。
*       创建临时数组 merged 用于存储合并后的结果。
*       使用两个指针 i 和 j 分别指向左半部分数组和右半部分数组的起始位置。
*       比较指针所指向的元素，将较小的元素放入 merged 数组中，并移动相应的指针。
*       如果其中一个子数组的元素已经全部放入 merged 数组中，则将另一个子数组中剩余的元素依次放入 merged 数组中。
*       返回合并后的有序数组 merged。
* 3. 返回结果：当分割和合并的过程递归完成后，返回最终的有序数组。*/
void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    int L[n1], R[n2];

    for (int i = 0; i < n1; i++) {
        L[i] = arr[left + i];
    }
    for (int j = 0; j < n2; j++) {
        R[j] = arr[mid + 1 + j];
    }

    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

// 堆排序
/*堆排序是一种基于二叉堆的排序算法，它利用堆的性质进行排序。堆是一种完全二叉树， \ 
*       并且满足堆性质：对于每个节点i，其父节点的值大于等于（或小于等于）它的子节* 点的值。
* 堆排序的步骤如下：
* 1. 构建最大堆：从最后一个非叶子节点开始，依次调用heapify函数，将数组调整为最大堆。
*       遍历非叶子节点，从最后一个非叶子节点开始（n/2 - 1），到根节点结束（0）。
*       对每个非叶子节点调用heapify函数，将当前节点及其子树调整为最大堆。
* 2. 堆排序：重复执行以下步骤，直到所有元素都被排序。
*       将根节点（最大值）与当前未排序部分的最后一个元素交换。
*       缩小堆的范围，即将最后一个元素从堆中移除，对剩余的元素进行调整，使其满足堆的性质。
*       重复上述步骤，直到所有元素都被排序。*/
void heapify(int arr[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }

    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }

    if (largest != i) {
        swap(&arr[i], &arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(int arr[], int n) {
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }

    for (int i = n - 1; i > 0; i--) {
        swap(&arr[0], &arr[i]);
        heapify(arr, i, 0);
    }
}

int main() {
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(arr) / sizeof(arr[0]);

    // 冒泡排序
    bubbleSort(arr, n);
    printf("冒泡排序后的数组：");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    // 插入排序
    insertionSort(arr, n);
    printf("插入排序后的数组：");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    // 选择排序
    selectionSort(arr, n);
    printf("选择排序后的数组：");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    // 快速排序
    quickSort(arr, 0, n - 1);
    printf("快速排序后的数组：");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    // 归并排序
    mergeSort(arr, 0, n - 1);
    printf("归并排序后的数组：");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    // 堆排序
    heapSort(arr, n);
    printf("堆排序后的数组：");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    return 0;
}
```

这些代码实现了冒泡排序、插入排序、选择排序、快速排序、归并排序和堆排序。

以下是使用C++语言实现冒泡排序、插入排序、选择排序、快速排序、归并排序和堆排序的示例代码：

```cpp
#include <iostream>
using namespace std;

// 冒泡排序
void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
            }
        }
    }
}

// 插入排序
void insertionSort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

// 选择排序
void selectionSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        swap(arr[i], arr[minIndex]);
    }
}

// 快速排序
int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// 归并排序
void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    int L[n1], R[n2];

    for (int i = 0; i < n1; i++) {
        L[i] = arr[left + i];
    }
    for (int j = 0; j < n2; j++) {
        R[j] = arr[mid + 1 + j];
    }

    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

// 堆排序
void heapify(int arr[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }

    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }

    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(int arr[], int n) {
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }

    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}

int main() {
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(arr) / sizeof(arr[0]);

    // 冒泡排序
    bubbleSort(arr, n);
    cout << "冒泡排序后的数组：";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // 插入排序
    insertionSort(arr, n);
    cout << "插入排序后的数组：";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // 选择排序
    selectionSort(arr, n);
    cout << "选择排序后的数组：";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // 快速排序
    quickSort(arr, 0, n - 1);
    cout << "快速排序后的数组：";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // 归并排序
    mergeSort(arr, 0, n - 1);
    cout << "归并排序后的数组：";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    // 堆排序
    heapSort(arr, n);
    cout << "堆排序后的数组：";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    return 0;
}
```

这些代码实现了冒泡排序、插入排序、选择排序、快速排序、归并排序和堆排序。
---
layout: post
title:  "Sort algorithm를 정리해 보자"
date:   2019-08-17 15:55:41 +0900
categories: java
---

## 상황

  * Sort algorithm를 정리해 보자

* * *

## Selection Sort (선택정렬)

   > 기준이 되는 수와 나머지 수를 비교하여 작은 수를 앞으로 보낸다.
   
   - 시간복잡도 : O(n<sup>2</sup>)
   - [생활코딩-선택정렬](https://opentutorials.org/course/543/2726){: target="_blank"}

```java
private void selectionSort(int[] input) {
    int tmp;
    for (int i = 0; i < input.length - 1; i ++) {
        for (int j = i + 1; j < input.length; j ++) {
            if (input[i] > input[j]) {
                tmp = input[j];
                input[j] = input[i];
                input[i] = tmp;
            }
        }
    }
}
```

## Bubble Sort (버블정렬)

   > n번째 인덱스와 n + 1번째 값 비교하여 큰 값을 뒤로 보낸다.
   
   - 시간복잡도 : O(n<sup>2</sup>)
   - [생활코딩-버블정렬](https://opentutorials.org/course/543/2722){: target="_blank"}

```java
private void bubbleSort(int[] input){
    int tmp;
    for (int i = 0 ; i < input.length; i ++) {
        for (int j = 0 ; j < input.length - i - 1; j ++) {
            if (input[j] > input[j + 1]) {
                tmp = input[j];
                input[j] = input[j + 1];
                input[j + 1] = tmp;
            }
        }
    }
}
```

## Insertion Sort (삽입정렬)

   > 기준이 되는 수와 앞의 수의 값 비교하여 앞에 큰 수가 있다면 큰 수를 뒤로 옮기고 기준이 되는 수를 삽입한다.
   
   - 시간복잡도 : O(n<sup>2</sup>)
   - [생활코딩-삽입정렬](https://opentutorials.org/course/543/2725){: target="_blank"}

```java
private void insertionSort(int[] input){
    int tmp, j;
    for(int i = 1 ; i < input.length ; i ++) {
        tmp = input[i];
        for(j = i - 1 ; j >= 0 && input[j] > tmp; j --) {
            input[j + 1] = input[j];
        }
        input[j + 1] = tmp;
    }
}
```
* * *
## 참고
   - 원본코드는 여기에 [[Github]](https://github.com/jyjalgorithmstudy/algorithm/blob/master/yongjin/java/src/sort/SortAlgorithm.java){: target="_blank"}
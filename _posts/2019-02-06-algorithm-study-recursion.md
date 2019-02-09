---
layout: post
title: "알고리듬 공부 // 재귀(recursion)"
comments: true
author: feynubrick
date: 2019-02-06
tags: [JavaScript, Study, Algorithm, CodeStates]
---

# 들어가면서

나는 재귀를 그저 자신을 다시 불러오는 함수를 사용하는 알고리듬 정도로만 알고 있었다.
그래서 피보나치 수열(Fibonacci Series) 문제 같은 비교적 간단히 이해할 수 있는 알고리듬만 공부해보고는 "이거 순차적으로 풀어도 되는건데, 별거 아니겠지?" 라는 생각을 가졌지만,
한편으로는 이해하기 어려웠다.
순차적 문제 해결에 익숙해져서 재귀적 알고리듬은 생각해보지 않았기 때문이다.

그리고 순차적 알고리듬만으로도 여태까지 큰 문제없이 잘 살아왔다.
코드스테이츠(CodeStates)의 이머시브(Immersive)과정에 가기 위한 시험인 IAT를 치르기 전까지는...

## IAT 시험 통과 실패... 그리고 다시 공부

나는 IAT 1차시험에 떨어졌다.
당연히 붙을 거라고 생각했지만, 그랬다.
뭔가 얻고 싶은 것을 얻지 못할 때 항상 그렇지만, 기분이 몹시 안좋았다.

변명거리도 많았다. 이런 경우에 응당 자기 보호를 위해 행복회로가 돌기 마련이니까.
제대로 풀었는데 디버깅을 철저하게 못해서 제대로 못풀었다느니,
괜찮게 풀었는데 주석처리를 해놨다느니...

<figure>
  <img src="https://media.giphy.com/media/9mmWez1yhWNXO/giphy.gif" alt="spinning-top"/>
  <figcaption>영화 "인셉션"에 나온 팽이. 주인공이 이곳이 꿈인지 현실인지 확인하기 위해 사용한다. 꿈이라면 영원히 돈다. 이 정도면 충분히 본 것이다. 물리적 관점에서 이 팽이는 넘어진 것이나 다름없다. 그러니까 지금은 현실이다.</figcaption>
</figure>

하지만 바뀌지 않는 사실이 있다.
시험에서 떨어졌다는 것과 내가 재귀를 제대로 이해하지 못하고 있었다는 것.
그리고 또하나, 아직 재시험 기회가 남아있다는 것.
이건 바뀌지 않는다.
문제를 해결하려면 현실로 돌아와야한다.

그래서 다시 공부를 시작했다.
어찌 보면 이 실패(작다고 할수는 있겠으나 실패는 실패다)는 내가 배울 수 있는 기회다.
시험이 많은 스트레스를 줘서 나쁘긴 하지만, 자기가 모르는 것을 알려준다는 순기능도 있다는 걸 다시한번 깨닫는다.

내가 잘 모르고 있던 재귀를 다시 공부해보자.

# 재귀(recursion)?

구글에서 "recursion"을 검색하면 다음과 같이 "Did you mean: recursion" 이라고 나온다. 클릭하면? 같은 페이지가 나온다.

![google-search-recursion](/assets/recursion-google.png)

`[피식, 깔깔, 하하]` 중 어떤 반응이라도 보였다면, 슬프지만 당신은 진성 이과충(?)이다. 어떻게 이런 유머에 웃을 수 있는가...(?) 

슬픈 감정은 뒤로하고, 이 예에서 봤듯이 재귀는 뭔가 자기를 다시 불러오는 방식을 말하는 것 같다. 좀 더 구체적으로 쓰면 다음과 같을 것이다.

> 재귀 함수는 바닥 조건(base condition)이 참일 때까지 자기 자신을 호출하고, 실행을 종료하는 함수를 말한다.

간단한 코드 예제로 이게 무슨 뜻인지 살펴보자.

```javascript
function recursive (n) {
    if (n !== 0) {
        return recursive(n - 1);
    } else {
        return 0;
    }
}
```

여기서, 바닥 조건은 `n === 0`에 해당하고, 조건이 이 바닥 조건에 도달하지 않을 때는 한 단계씩 바닥 조건에 다가가게 하면서 자기자신을 부른다.

## 재귀 함수 사용 예제

당신도 나와 같다면, 정의를 달달 외우기 보다는 예제를 통해 몸(?)으로 익하는 방법을 선호할 것이다.

여러 예제를 보면서 재귀 함수에 익숙해져보자.

### Example 1: N 번째 짝수 구하기

`N` 번째 짝수를 구하는 함수 `getNthEvenNumber(N)`를 재귀를 사용해 만들어보자.

```javascript
function getNthEvenNum (N) {
    if (N === 1) {
        return 0;
    } else {
        return getNthEvenNum(N-1) + 2;
    }
};

getNthEvenNum(3); // 4
```

`getNthEvenNum(3)`을 실행하면, 다음의 순서로 콜스택이 채워지고 비워진다.

- `getNthEvenNum(3)`
- `getNthEvenNum(3)`, `getNthEvenNum(2)`
- `getNthEvenNum(3)`, `getNthEvenNum(2)`, `getNthEvenNum(1)`
- `getNthEvenNum(3)`, `getNthEvenNum(2)`
- `getNthEvenNum(3)`
- empty

이 과정을 풀어서 설명하면 다음과 같다.

- `getNthEvenNumber(3)`: `N`이 `3`이군, `getNthEvenNumber(2)`야 결과 좀 보내줘!
  - `getNthEvenNumber(2)`: `N`이 `2`이군, `getNthEvenNumber(1)`야 결과 좀 보내줘!
    - `getNthEvenNumber(1)`: `N`이 `1`이군, `0`을 반환하고 쉬어야지.
  - `getNthEvenNumber(2)`: `getNthEvenNumber(1)`에서 반환 값이 왔군. `0`이라는데?
  - `getNthEvenNumber(2)`: 그럼 `2`를 반환하고 쉬어야지.
- `getNthEvenNumber(3)`: `getNthEvenNumber(2)`에서 응답이 왔군. `2`이라는데?
- `getNthEvenNumber(3)`: 그럼 `4`를 반환하고 쉬어야지.

### Example 2: 2의 N 제곱 계산하기

`2`의 `N` 제곱을 계산하는 `getPowerOf2(N)`을 재귀함수를 사용해 만들어보자.

- `N`이 `0` 이라면, `1`을 반환한다.
- 아니라면, `2 * getPowerOf2(N - 1)`을 반환한다.

코드로 쓰면 다음과 같다.

```javascript
function getPowerOf2 (N) {
    if (N === 0) {
        return 1;
    } else {
        return 2 * getPowerOf2(N - 1);
    }
}

getPowerOf2(3); // 8
```

### Example 3: 순차 검색(sequential search)

- 입력: `array`가 배열이고, `i`, `j`는 `i <= j`를 만족하는 양의 정수이고, `x`는 배열 `array`에서 검색될 키를 말한다.
- 출력: `x`가 `array`의 `i` 번째, `j` 번째 사이에 있다면, 결과는 `x`가 들어있는 인덱스이고, 없으면, `-1`이다.

```javascript
function searchArraySequentially (array, i, j, x) {
    if (i <= j) {
        if (array[i] === x) {
            return i;
        } else {
            return searchArraySequentially(array, i + 1, j, x);
        }
    } else {
        return -1;
    }
}

var array = ['a', 'b', 'c', 'd', 'e'];
var result1 = searchArraySequentially(array, 0, 4, 'e');
var result2 = searchArraySequentially(array, 0, 3, 'e');
console.log(result1); // 4;
console.log(result2); // -1;
```

### Example 4: 자연수 판별

```javascript
function isNaturalNumber (x) {
    if (x < 0) {
        return false;
    } else {
        if (x === 0) {
            return true;
        } else {
            return isNaturalNumber(x - 1);
        }
    }
}

console.log(isNaturalNumber(-1)); // false
console.log(isNaturalNumber(0)); // true
console.log(isNaturalNumber(100)); // true
console.log(isNaturalNumber(12.4)); // false
```

### Example 5: 피보나치 수열(Fibonacci series)

피보나치 수열은 잘 알려져 있듯이,
앞의 두 수를 더한 결과가 다음 수로 정해지는 수열을 말한다.

```
0, 1, 1, 2, 3, 5, 8, 13, ...
```

```javascript
var fibonacci = function (n) {
    // base case
    if (n === 1) {
        return 0;
    } else if (n === 2) {
        return 1;
    }

    return fibonacci(n - 2) + fibonacci(n - 1);
}
```

### Example 6: 문자열 뒤집기(string reverse)

```javascript
function reverse(str) {
    var arr = str.split('');
    
    function rev (arr, l, h) {
        if (l < h) {
            swap(arr, l, h);
            rev(arr, l + 1, h - 1);
        }
    } 

    function swap (arr, i, j) {
        var buf = arr[i];
        arr[i] = arr[j];
        arr[j] = buf;
    }
    
    rev(arr, 0, str.length - 1);
    return arr.join('');
}
```

## 재귀적 알고리듬의 문제 해결 전략

자기자신을 호출하는 형식을 충분히 몸으로 익혔다.
그러나 이런 형식보다 더 중요한 것은 문제 해결 전략이다.
계속 살펴봤듯이 재귀적 알고리듬은 문제를 좀 더 단순한 문제로 쪼개어 문제를 해결한다.
이런 방식을 "나눠서 정복하라"라 부른다.

# 나눠서 정복하라!

"나눠서 정복하라(Divide and Conquer)!"는 재귀를 가장 잘 설명해주는 말일 것 같다.

이 문제해결 전략은 다음의 세가지 단계로 이루어져 있다.

- 나누기(divide): 문제를 여러 개의 하위문제(subproblem)로 나누는데, 같은 문제를 보다 작은 단위로 쪼개는 것을 말한다.
- 정복하기(conquer): 이렇게 나눠진 문제는 재귀적으로 해결한다. 하위문제의 크기가 충분히 작다면, 직접 해결한다.
- 합치기(combine): 하위문제들의 답을 합쳐 원래 문제의 답으로 만든다.

하위문제가 재귀적으로 풀만큼 충분히 클 때를 재귀 조건(recursive case)에 해당하고, 더이상 재귀를 사용지 않아도 될 만큼 충분히 작아지면 바닥 조건(base case)에 해당한다..

## Example 7: Merge Sort

배열 `arr`이 있을 때, 인덱스 `a` 부터 인덱스 `b`까지 순서대로 정렬하는 알고리듬이다.
이 문제는 전체 문제를 여러개의 하위문제로 쪼갠 뒤 합치는 방식으로 정렬하는데,
이 알고리듬에서 가장 중요한 부분은 정렬된 두 배열을 합치는(merge) 과정이기 때문에 이름에 merge가 붙게 됐다.

merge sort를 이해하려면, 이 merge 과정을 반드시 이해해야한다.
카드를 순서대로 정렬하고 있다고 해보자.
카드 뭉치 두개가 서로 나누어져 있는데, 각각은 순서대로 정렬되어있다.
각각의 뭉치의 제일 위에는 제일 작은 것(예를들어 Spade A)이 섞는 사람이 그 내용을 볼 수 있는 방향으로 올라와 있다.
이 때, 두 카드 뭉치를 순서대로 섞으려면 어떻게 해야할까?
매우 간단한 문제로 보인다.
그저 양쪽 카드 뭉치의 제일 위에 올라와 있는 카드를 서로 비교한 뒤에 작은 것을 집어서 테이블 위에 뒤집어 놓고,
이를 반복하면 된다.
한쪽 뭉치가 다 없어지면, 다른쪽 뭉치는 이미 정렬되어 있었으므로 더 볼 것없이 나머지 뭉치를 계속 뒤집어 쌓아놨던 카드에 뒤집어 얹어 놓는다.
그러면 새로 합쳐진 뭉치는 정렬되어 있을 것이다.

자, 이렇게 놓고 보니 별로 어렵지는 않다.
그럼 merge sort가 "나눠서 정복하라" 방식을 구체적으로 어떻게 따라서 문제를 해결하는지 살펴보자.

- 나누기: n 개의 요소가 있는 배열을 n/2개의 요소로 이루어진 배열 두 개로 쪼개 정렬한다.
- 정복하기: 두 개의 하위 배열을 `mergeSort`를 사용해 재귀적으로 정렬한다.
- 합치기: 두 개의 정렬된 하위배열로 정렬된 답을 만든다.

알고리듬을 이해했으니 이제 실제 코드를 보면서 살펴보자.

```javascript
function mergeSort(arr, a, b) {
    console.log(`mergeSort([${arr}], ${a}, ${b})`);

    if (a < b) {
        var m = parseInt((a + b) / 2);
        mergeSort(arr, a, m);
        mergeSort(arr, m + 1, b);
        merge(arr, a, m, b);
    }
}

function merge(arr, a, m, b) {
    console.log(`merge([${arr}], ${a}, ${m}, ${b})`);

    var left = arr.slice(a, m + 1); // arr[a] ~ arr[m]
    var right = arr.slice(m + 1, b + 1); // arr[m+1] ~ arr[b]
    console.log(`  left : [${left}]`);
    console.log(`  right: [${right}]`);
    left.push(Infinity);
    right.push(Infinity);

    var iLeft = 0;
    var iRight = 0;

    for (var i = a; i <= b; i++) {
        if (left[iLeft] <= right[iRight]) {
            arr[i] = left[iLeft];
            iLeft++;
        } else {
            arr[i] = right[iRight];
            iRight++;
        }
    }
}

var arr = [5, 2, 4, 6, 1, 3];
mergeSort(arr, 0, arr.length - 1);
console.log(`sorted: ${arr}`);
```

`mergeSort(arr, a, b)`는 배열 `arr`의 `a` 번째 항목부터 `b` 번째 항목까지를 merge sort로 정렬하는 함수다.
처음에 중간 인덱스인 `m`을 찾고, 문제를 `mergeSort(arr, a, m)`, `mergeSort(arr, m + 1, b)`로 쪼갠다.
그리고 정렬된 두 배열을 `merge(arr, a, m, b)`를 이용해 합친다.

`merge(arr, a, m, b)`는 위에서 설명한 카드를 순서대로 정렬하는 것을 생각하면서 보면 이해할 수 있다.
`left`와 `right`는 각각 왼쪽과 오른쪽의 카드뭉치다.
각각의 뭉치 끝에 크기가 `Infinity` 즉, 무한대인 카드를 끼워넣었는데,
이는 매번 카드뭉치 한쪽이 다 없어졌는지 확인하는 과정을 피하기 위해 끼워 넣은 트릭일 뿐이다.
이제 `for`문을 통해 왼쪽 뭉치의 카드와 오른쪽 뭉치의 카드를 비교하면서 `arr`에 차곡차곡 쌓아 넣는다.
이 과정이 끝나면, `a` 부터 `b`까지 정렬이 완료된다.

이 과정을 자세히 이해해보기 위해 `console.log()`를 중간중간에 끼워 넣었다.
코드를 실행하면 다음 결과를 얻을 수 있다.

```
mergeSort([5,2,4,6,1,3], 0, 5)
mergeSort([5,2,4,6,1,3], 0, 2)
mergeSort([5,2,4,6,1,3], 0, 1)
mergeSort([5,2,4,6,1,3], 0, 0)
mergeSort([5,2,4,6,1,3], 1, 1)
merge([5,2,4,6,1,3], 0, 0, 1)
  left : [5]
  right: [2]
mergeSort([2,5,4,6,1,3], 2, 2)
merge([2,5,4,6,1,3], 0, 1, 2)
  left : [2,5]
  right: [4]
mergeSort([2,4,5,6,1,3], 3, 5)
mergeSort([2,4,5,6,1,3], 3, 4)
mergeSort([2,4,5,6,1,3], 3, 3)
mergeSort([2,4,5,6,1,3], 4, 4)
merge([2,4,5,6,1,3], 3, 3, 4)
  left : [6]
  right: [1]
mergeSort([2,4,5,1,6,3], 5, 5)
merge([2,4,5,1,6,3], 3, 4, 5)
  left : [1,6]
  right: [3]
merge([2,4,5,1,3,6], 0, 2, 5)
  left : [2,4,5]
  right: [1,3,6]
sorted: 1,2,3,4,5,6
```

이 결과를 보면 이 알고리듬이 어떻게 "나눠서 정복하라!" 방식으로 작동하는지 볼 수 있다.
그림으로 그려보면 다음과 같다.

<figure>
  <img src="/assets/merge-sort-example.png" alt="merge-sort-example"/>
  <figcaption>
  [5,2,4,6,1,3] 배열을 merge sort를 사용해 정렬하는 방법을 묘사한 그림.
  회색은 정렬되지 않은 배열을, 연한 녹색은 정렬된 배열을 의미한다.
  빨간색으로 쓰인 숫자들은 실행되는 순서를 의미하며 각각의 순서에서 일어나는 일은 아래에 자세히 묘사해두었다.
  </figcaption>
</figure>

각각의 과정에서 일어나는 일은 아래와 같다.

index 0 부터 5까지 정렬한다. (`[5,2,4,6,1,3]`)

   1. 0부터 2까지 정렬한다. (`[5,2,4]`) **@1**
      1. 0부터 1까지 정렬한다. (`[5,2]`) **@2**
         1. 0부터 0까지 정렬한다. (`[5]`) **@3**
         2. 1부터 1까지 정렬한다. (`[2]`) **@4**
         3. 두 결과를 순서대로 합친다. => `[2,5]` **@5**
      2. 2부터 2까지 정렬한다. (`[4]`) **@6**
      3. 두 결과를 순서대로 합친다. => `[2,4,5]` **@7**
   2. 3부터 5까지 정렬한다. (`[6,1,3]`) **@8**
      1. 3부터 4까지 정렬한다. (`[6,1]`) **@9**
         1. 3부터 3까지 정렬한다. (`[6]`) **@10**
         2. 4부터 4까지 정렬한다. (`[1]`) **@11**
         3. 두 결과를 순서대로 합친다. => `[1,6]` **@12**
      2. 5부터 5까지 정렬한다. (`[3]`) **@13**
      3. 두 결과를 순서대로 합친다. => `[1,3,6]` **@14**
   3. 두 결과를 순서대로 합친다. => `[1,2,3,4,5,6]` **@15**

## Example 8: Maximum-Subarray Problem

Maximum-subarray 문제는 존재하는 배열의 각 항목의 합이 최대가 되는 연속적인 하위 배열을 찾는 문제다.
쉽게 말해 

`sum(arr[low], ..., arr[high])`이 최대가 되는 `arr`의 하위 배열 즉, `[arr[low], ..., arr[high]]`을 찾는 문제다.

이것도 "나눠서 정복하라!" 방식이므로 다음과 같이 나눠서 문제를 볼 수 있다.

- 나누기: 배열을 두부분으로 나눈 다음, 세가지 다른 방법으로 최대값을 주는 하위 배열을 찾는다.
  - 왼쪽
  - 오른쪽
  - 양쪽에 걸쳐
- 정복하기: 왼쪽과 오른쪽의 경우 재귀를 사용해 최대값을 주는 하위 배열을 찾고, 양쪽에 걸치는 경우는 그저 쭉 계산한다.
- 합치기: 세 결과를 비교해 최대값을 주는 하위 배열을 반환한다.

다음은 이 알고리듬을 사용한 코드다.

```javascript
function findMaxCrossingSubarray (arr, low, mid, high) {
    console.log(`findMaxCrossSubarr(${low}, ${mid}, ${high})`);

    var leftMaxSum = -Infinity;
    var sum = 0;
    
    for (var i = mid; i >= low; i--) {
        sum += arr[i];

        if (sum > leftMaxSum) {
            leftMaxSum = sum;
            var maxLeftIndex = i;
        }
    }

    var rightMaxSum = -Infinity;
    sum = 0;

    for (i = mid + 1; i <= high; i++) {
        sum += arr[i];

        if (sum > rightMaxSum) {
            rightMaxSum = sum;
            var maxRightIndex = i;
        }
    }

    return new result(maxLeftIndex, maxRightIndex, leftMaxSum + rightMaxSum);
}

function findMaximumSubarray (arr, low, high) {
    console.log(`findMaxSubarr(${low}, ${high})`);

    if (high === low) {
        return new result(low, high, arr[low]);
    } else {
        var mid = parseInt((low + high) / 2, 10);
        var left = findMaximumSubarray(arr, low, mid);
        var right = findMaximumSubarray(arr, mid + 1, high);
        var cross = findMaxCrossingSubarray(arr, low, mid, high);
        
        if (left.sum >= right.sum && left.sum >= cross.sum) {
            return new result(left.low, left.high, left.sum);
        } else if (right.sum >= left.sum && right.sum >= cross.sum) {
            return new result(right.low, right.high, right.sum);
        } else {
            return new result(cross.low, cross.high, cross.sum);
        }
    }
}

function result(low, high, sum) {
    this.low = low;
    this.high = high;
    this.sum = sum;
}

var arr = [1, -4, 3, -4];
console.log(`arr: [${arr}]`);
var result = findMaximumSubarray(arr, 0, arr.length - 1);
console.log(`maximum subarray: ${result.low} to ${result.high} and it gives ${result.sum}`);
```

결과는 다음과 같다.

```
0  arr: [1,-4,3,-4]
1  findMaxSubarr(0, 3)
2  findMaxSubarr(0, 1)
3  findMaxSubarr(0, 0)
4  findMaxSubarr(1, 1)
5  findMaxCrossSubarr(0, 0, 1)
6  findMaxSubarr(2, 3)
7  findMaxSubarr(2, 2)
8  findMaxSubarr(3, 3)
9  findMaxCrossSubarr(2, 2, 3)
10 findMaxCrossSubarr(0, 1, 3)
11 maximum subarray: 2 to 2 and it gives 3
```

이 알고리듬을 더 잘 이해하기 위해 아래의 구조도를 통해 문제가 어떤 순서로 어떻게 해결되는지 살펴보자.

`arr: [1,-4,3,-4]`

- 1. f(0, 3)
  - mid: 1
  - 2. left: f(0, 1)
    - mid: 0
    - 3. left: f(0, 0) => (0, 0, 1)
    - 4. right: f(1, 1) => (1, 1, -4)
    - 5. cross: g(0, 0, 1) => (0, 1, -3)
    - 비교 후 (0, 0, 1) 반환
  - 6. right: f(2, 3)
    - mid: 2
    - 7. left: f(2, 2) => (2, 2, 3)
    - 8. right: f(3, 3) => (3, 3, -4)
    - 9. cross: g(2, 2, 3) => (2, 3, -1)
    - 비교 후 (2, 2, 3) 반환
  - 10. cross: g(0, 1, 3) => (0, 2, 0)
  - 비교 후 (2, 2, 3) 반환

# 참고

- [재귀 알고리듬과 예제 (psuedocode)](https://www.cs.odu.edu/~toida/nerzic/content/recursive_alg/rec_alg.html)
- [재귀 알고리듬 설명 (파이썬)](https://hackernoon.com/algorithms-explained-recursion-54831247dd85)
- [재귀는 어렵지 않아요 (JS)](https://medium.freecodecamp.org/recursion-is-not-hard-858a48830d83)
- [Introduction to Algorithm 3rd Edition, Thomas H. Cormen et al.](https://www.amazon.com/Introduction-Algorithms-3rd-MIT-Press/dp/0262033844/ref=sr_1_1?ie=UTF8&qid=1549426550&sr=8-1&keywords=introduction+to+algorithms)

# 공부할만한 사이트

- [Techie Delight](https://www.techiedelight.com/recursion-practice-problems-with-solutions/): 재귀 알고리듬 문제와 해법이 나와있다.
- [HackerRank](https://www.hackerrank.com): 관련 문제를 풀어볼 수 있다. 테스트도 제공하고 있어, 자신이 해결한 방식이 일반적인 상황에서도 작동하는지 확인할 수 있다.
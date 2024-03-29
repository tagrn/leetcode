# 문제 1 - 15

## 88. Merge Sorted Array

:::note
### 문제 요약

* 정렬된 두 리스트 `nums1`, `nums2`가 주어지고 `nums1`에 `nums2`를 병합하여 정렬하라.
* `nums1` 리스트에는 `nums2`의 길이 만큼 0이 추가로 채워져 있다.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/merge-sorted-array/)
:::

<br/>



#### 첫 번째 시도 ✅

* `nums1` 리스트에 `nums2` 길이 만큼 뒤에서 부터 없앤다. (0이 추가로 채워져 있는 것을 없앰.)
* `nums2` 리스트를 `nums1`으로 삽입한다.
* `num1` 리스트를 정렬한다.

```ts
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
    for (let i=0; i<n; i++) {
        nums1.pop()
    }
    for (let i=0; i<n; i++) {
        nums1.push(nums2[i])
    }

    nums1.sort((a,b) => a - b)
};
```



<br/>



#### 두 번째 시도 ✅

* `nums1` 리스트에 추가로 채워져 있는 부분에 `nums2`을 넣어준다.
* `num1` 리스트를 정렬한다.

```ts
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
    for (let i=0; i<n; i++) {
        nums1[m+i] = nums2[i]
    }

    nums1.sort((a,b) => a - b)
};
```



<br/>



#### 세 번째 시도 ✅

* 정렬된 리스트라는 점을 이용하여 각 리스트에서 제일 큰 숫자를 하나씩 빼온다.
* `nums1`의 알맞은 자리에 넣어준다.

```ts
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
    let [i, j, k] = [m-1, n-1, n+m-1]
    while (k > -1) {
        if (i > -1 && nums1[i] > nums2[j]) {
            nums1[k--] = nums1[i--]
        } else if (j > -1) {
            nums1[k--] = nums2[j--]
        } else {
            break
        }
    }
};
```



<br/>

---

## 27. Remove Element

:::note

### 문제 요약

* 주어진 리스트에서 특정 값을 빼고 남은 리스트의 길이를 반환하라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/remove-element/)

:::



<br/>


#### 첫 번째 시도 ✅

* 리스트를 필터링해서 필터링한 값을 `nums` 리스트의 알맞은 위치에 넣어준다.
* 필터링된 리스트의 길이를 반환한다.

```ts
function removeElement(nums: number[], val: number): number {
    const filtered = nums.filter(x => x !== val)
    for (let i=0; i<filtered.length; i++) {
        nums[i] = filtered[i]
    }
    
    return filtered.length
};
```



<br/>



#### 두 번째 시도 ✅

* 리스트에 삽입할 인덱스를 지정해두고 `nums` 리스트를 순회하며 타겟 숫자가 아닐 경우 삽입할 인덱스에 하나씩 삽입한다.
* 삽입 후, 삽입할 인덱스를 1씩 올린다.
* 삽입할 인덱스를 반환하면 삽입했던 수를 반환할 수 있다.

```ts
function removeElement(nums: number[], val: number): number {
    let j = 0
    for (const num of nums) {
        if (num !== val) {
            nums[j++] = num
        }
    }
    
    return j
};
```



<br/>

---

## 26. Remove Duplicates from Sorted Array

:::note

### 문제 요약
* 주어진 정렬된 리스트에서 중복된 값을 빼고 남은 리스트의 길이를 반환하라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

:::

<br/>

#### 첫 번째 시도 ✅

* 리스트에 삽입할 인덱스와 이전 값을 저장할 변수(`before`)를 지정해 둔다.
* `nums` 리스트를 순회하며 현재 값과 `before`이 같다면 삽입할 인덱스에 삽입한다.
* 삽입 후, 삽입할 인덱스를 1씩 올리고, 삽입을 하지 않더라도 `before`에 현재 값을 저장하고 다음 순회로 넘어간다.
* 삽입할 인덱스를 반환하면 삽입했던 수를 반환할 수 있다.

```ts
function removeDuplicates(nums: number[]): number {
    let j = 0
    let before = null
    for (const num of nums) {
        if (num !== before) {
            nums[j++] = num
        }
        before = num
    }
    
    return j
};
```



<br/>



#### 더 나아가기

* `before` 이 아니라 그냥 이전 인덱스의 값과 비교해도 무방하다.



<br/>

---

## 80. Remove Duplicates from Sorted Array II

:::note
### 문제 요약

* 주어진 정렬된 리스트에서 3개 이상 중복된 값을 빼고 남은 리스트의 길이를 반환하라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

:::

<br/>

#### 첫 번째 시도 ✅

* 리스트에 삽입할 인덱스와 현재 값이 있었다는 확인을 할 Object를 선언한다.
* `nums` 리스트를 순회하며 현재 값에 대한 `visit[현재 값]` 값이 없다면 0으로 초기화시켜주고 `visit[현재 값]`이 2보다 작다면 삽입할 인덱스에 현재 값을 삽입해준다.
* 삽입 후, 삽입할 인덱스를 1씩 올리고, `visit[현재 값]`을 1씩 올려준다.
* 삽입할 인덱스를 반환하면 삽입했던 수를 반환할 수 있다.

```ts
function removeDuplicates(nums: number[]): number {
    let j = 0
    let visit = {}
    for (const num of nums) {
        if (!visit[num]) {
            visit[num] = 0
        }
        if (visit[num] < 2) {
            visit[num]++
            nums[j++] = num
        }
    }
    
    return j
};
```



<br/>


---

## 169. Majority Element

:::note
### 문제 요약
* 주어진 리스트에서 `리스트의 길이/2` 보다 큰 개수를 가진 숫자를 반환하라.

<br/>


[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/majority-element/)
:::



<br/>

#### 첫 번째 시도 ✅

* 현재 값이 있었다는 확인을 할 Object(`visit`)를 선언한다.
* `nums` 리스트를 순회하며 현재 값에 대한 `visit[현재 값]` 값이 없다면 0으로 초기화시켜주고 `visit[현재 값]`에 1을 더해준다.
* `visit[현재 값]`이  `리스트의 길이/2` 보다 크다면 해당 숫자를 반환한다.

```ts
function majorityElement(nums: number[]): number {
    const visit = {}
    for (const x of nums) {
        if (!visit[x]) {
            visit[x] = 0
        }
        visit[x]++
        if (visit[x] > nums.length/2) {
            return x
        }
    }
};
```



<br/>



#### 두 번째 시도 ✅

* 정렬을 이용해서 n/2 번째의 값을 리턴하는 방법도 있다.

```ts
function majorityElement(nums: number[]): number {
    nums.sort((a,b) => a-b)
    return nums[Math.floor(nums.length/2)]
};
```



<br/>

---

## 189. Rotate Array

:::note
### 문제 요약

* 주어진 리스트의 맨 앞의 값을 맨 뒤로 옮기는 행동을 `k` 하라.

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/rotate-array/)
:::


<br/>

#### 첫 번째 시도 ✅

* `k`를 `nums` 길이로 나눈 나머지(`mod`)를 구한다.
*  `nums` 길이에서 `mod`를 뺀 만큼의 값들을 잠시 모아둔다.
* `nums`에 삽입할 인덱스를 0으로 선언해놓는다.
* `nums` 길이에서 `mod`를 뺀 값을 시작으로 `nums` 길이 + `nums` 길이에서 `mod`를 뺀 값까지 순회한다.
* 해당 순회에서 `i`가 `nums` 길이보다 작으면 삽입할 인덱스에 `nums` 값에서 바로 찾아 넣어주고, 그보다 크다면 `filteredNums`에 잠시 모아둔 값을 활용하여 삽입할 인덱스에 값을 넣어준다. (해당인덱스는 `nums` 길이보다 크므로 나머지 연산을 통해 `filteredNums` 인덱스를 구한다.)
* 삽입하였다면 삽입할 인덱스의 값에 1을 더해준다.

```ts
function rotate(nums: number[], k: number): void {
    const len = nums.length
    const mod = k%len
    const filteredNums = nums.filter((_,i) => i<len-mod)
    let idx = 0
    for (let i=len-mod; i<2*len-mod; i++) {
        if (i < len) {
            nums[idx++] = nums[i]
        } else {
            nums[idx++] = filteredNums[i%len]
        }
    }
};
```



<br/>



#### 더 나아가기

* 첫 번째 시도에서 순회방향을 문제에 맞게 반대로 만들어도 풀린다.
* Deque를 구현하여 풀어도 된다. (파이썬에는 기본적으로 장착되어 있지만 TS에는 없어서 아쉽다.)
* Object(HashMap or Dictionary)로 체크하며 풀어도 될 듯하다.



<br/>

---

## 121. Best Time to Buy and Sell Stock

:::note
### 문제 요약

* 주식가격이 주어진 리스트가 있고 각 인덱스는 날짜라고 볼 수 있다.
* 최소 가격으로 사서 미래에 최대 가격으로 팔 수 있는 기댓값을 구하여라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

:::

<br/>

#### 첫 번째 시도 ✅

* 이전 날짜까지 포함하여 작은 값을 `min`이라는 변수에 저장해둔다.
* 리스트 순회를 하며 현재 값이 `min`보다 작은 값은 `min`에 저장하고 현재 값에서 `min` 값을 뺀다.
* 뺀 값 중 제일 큰 숫자를 반환한다.

```ts
function maxProfit(prices: number[]): number {
    let min = prices[0]
    let ans = 0
    for (const price of prices) {
        if (min > price) {
            min = price
        }
        ans = Math.max(price - min, ans)
    }

    return ans
};
```



<br/>



#### 두 번째 시도 ✅

* `Math.min`을 사용하고 리팩토링하여 좀 더 깔끔한 코드를 만들어 보았다.

```ts
function maxProfit(prices: number[]): number {
    let [min, ans] = [prices[0], 0]
    for (const price of prices) {
        min = Math.min(min, price)
        ans = Math.max(price - min, ans)
    }
    return ans
};
```



<br/>

---

## 122. Best Time to Buy and Sell Stock II

:::note

### 문제 요약

* 주식가격이 주어진 리스트가 있고 각 인덱스는 날짜라고 볼 수 있다.
* 최소 가격으로 사서 미래에 최대 가격으로 팔 수 있으며, 횟수에는 제한이 없다.
* 최대 이익을 구하여라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

:::

<br/>

#### 첫 번째 시도 ✅

* 주어진 리스트의 첫 번째 값을 `min`이라는 변수에 저장해둔다.
* 리스트 순회를 하며 현재 값이 `min`보다 크다면 두 값의 차를 결과값에 더하고 `min` 값을 현재 값으로 업데이트한다.
* 현재 값이 `min`보다 작다면 `min` 값을 현재 값으로 업데이트만 한다.
* 순회가 끝나면 결과값을 반환한다.

```ts
function maxProfit(prices: number[]): number {
    let [min, ans] = [prices[0], 0]
    for (const price of prices) {
        if (min < price) {
            ans += price - min
        }
        min = price
    }
    return ans
};
```



<br/>



#### 두 번째 시도 ✅ 🤙🏻

* 더 이상 코드를 개선할 방안이 생각나지 않아 찾아보니 `Math.max`를 사용하여 이전 값과 비교한 값이 0보다 크다면 결과 값에 더하는 것으로 리팩토링할 수 있었다.

```ts
function maxProfit(prices: number[]): number {
    let ans = 0
    for (let i=1; i<prices.length; i++) {
        ans += Math.max(0, prices[i]-prices[i-1])
    }
    return ans
};
```



<br/>

---

## 55. Jump Game

:::note

### 문제 요약

* 주어진 리스트의 각 인덱스에는 점프할 수 있는 길이가 주어진다.
* 마지막 인덱스로 갈 수 있는지 없는지를 확인하여 반환하라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/jump-game/)

:::



<br/>

#### 첫 번째 시도 ✅

* 주어진 리스트의 첫 번째 값을 `canJump`이라는 변수에 저장해둔다.
* 이 변수는 점프할 수 있는 최대 칸을 뜻한다. 그러므로 이 변수를 현재 값과 비교하여 계속 업데이트 해나간다.
* 점프를 할 수 없게 되면 `false`를 반환하고 리스트 순회가 성공적으로 마치면 `true`를 반환한다.

```ts
function canJump(nums: number[]): boolean {
    let canJump = nums[0]
    for (let i=1; i<nums.length; i++) {
        if (canJump <= 0) {
            return false
        }
        canJump = Math.max(nums[i], canJump-1)
    }

    return true
};
```



<br/>



#### 두 번째 시도 ✅

* 거꾸로 하는 방법도 있다.
* 도착지에서 부터 출발지까지 가능한지 확인하며, `arrival`이 출발지인 0에 도착할 수 있으면 `true`를 반환한다.

```ts
function canJump(nums: number[]): boolean {
    let arrival = nums.length-1
    for (let i=nums.length-2; i>-1; i--) {
        if (i+nums[i] >= arrival) {
            arrival = i
        }
    }

    return arrival ? false : true
};
```

<br/>

---

## 45. Jump Game II

:::note

### 문제 요약

* 주어진 리스트의 각 인덱스에는 점프할 수 있는 길이가 주어진다.
* 마지막 인덱스로 갈 수 있는 최소 점프 횟수를 구하여라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/jump-game-ii/)

:::



<br/>

#### 첫 번째 시도 ✅

* `mem` 이라는 리스트를 만들고 해당 리스트에 각 인덱스의 최소 점프 횟수를 저장한다.
* `mem[0]` 에는 먼저 0을 넣어주고 다음 인덱스부터 현재 인덱스의 `mem` 값이랑 이전 인덱스의 `mem` 값 + 1을 비교하여 더 작은 숫자를 `mem`에 업데이트하고`mem`의 마지막 인덱스 값을 반환한다.

```ts
function jump(nums: number[]): number {
    const mem = Array.from(Array(nums.length), x => Infinity)
    mem[0] = 0
    for (let i=1; i<nums.length; i++) {
        for (let j=0; j<nums[i-1]; j++) {
            if (i+j >= nums.length) {
                break
            }
            mem[i+j] = Math.min(mem[i-1] + 1, mem[i+j])
        }
    }

    return mem[mem.length-1]
};
```

<br/>


#### 두 번째 시도 ✅ 🤙🏻

* 더 개선할 방법이 없을까하여 고민하다 갈 수 있는 점프 수를 계속 갱신해주는 방법이 있었다.
* 그리고 나서 찾아보니 점프 갱신 개선방식보다 도착지를 갱신하면 순회 한 번당 `-1`을 더 안해줘도 풀리는 걸 알게 되었다.

```ts
function jump(nums: number[]): number {
    let [end, arr, ans] = [0, 0, 0]
    for (let i=0; i<nums.length-1; i++){
        arr = Math.max(arr, nums[i]+i)
        if (i === end){
            end = arr
            ans++
        }
    }

    return ans
};
```

<br/>


---

## 274. H-Index

:::note
### 문제 요약
* 한 연구원의 각 논문마다의 인용 횟수가 담긴 리스트를 제공한다.
* 해당 연구원의 h-index를 구하여라.
* h-index는 최대 논문 수 혹은 각 논문 n개가 각 인용 횟수 n번 이상일 때의 최대 n 중 낮은 숫자를 뜻한다.
* `max(최대 논문 수, 각 논문 n개가 각 인용 횟수 n번 이상일 때의 최대 n)`

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/h-index/)
:::

<br/>

#### 첫 번째 시도 ✅

* 먼저 인용횟수 리스트를 내림차순 정렬한다.
* 리스트를 순회하며 해당 인용횟수가 인덱스보다 작거나 같으면 인덱스를 반환한다.
* 리스트 순회가 성공적으로 끝나면 논문 수를 반환하면 된다.

```ts
function hIndex(citations: number[]): number {
    citations.sort((a,b) => b-a)
    for (let i=0; i<citations.length; i++) {
        if (citations[i] <= i) {
            return i
        }
    }
    return citations.length
};
```

<br/>


#### 더 나아가기

* 거꾸로 순회도 할 수 있다.
* 소팅하지 않는다면 각 인용 수를 따로 저장해두고 저장한 리스트를 통해 제일 큰 수부터 0까지 정답이 맞는지 확인하는 방법도 있다. (해당 방법이 코드는 더 길더라도 빠르다.)

<br/>

---

## 380. Insert Delete GetRandom O(1)

:::note
### 문제 요약
* 랜덤 셋을 구현하라. 기능 구현은 `insert`, `remove`, `getRandom`을 구현하면 된다.
* 단, random의 확률은 동일해야한다.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/insert-delete-getrandom-o1/)
:::

<br/>

#### 첫 번째 시도 ❌

* data를 오브젝트로 잡아 해당 객체에 넣고 빼는 작업을 하였다.
* 랜덤은 날짜 기준으로 랜덤화를 했는데, 동일하다고 인식하지 못 했는지 틀렸다고 나왔다.

```ts
class RandomizedSet {
    private data: Object
    constructor() {
        this.data = {}
    }

    insert(val: number): boolean {
        if (this.data[val] !== undefined) {
            return false
        }
        this.data[val] = val
        return true
        
    }

    remove(val: number): boolean {
        if (this.data[val] !== undefined) {
            delete this.data[val]
            return true
        }
        return false
    }

    getRandom(): number {
        const keys = Object.keys(this.data)
        return this.data[keys[Date.now()%keys.length]]
    }
}
```

<br/>


#### 두 번째 시도 ❌

* seed를 통해 해보았지만 이건 동일한 양으로 나와도 순서대로 나와서 그런지 테케를 통과하지 못 했다.

```ts
class RandomizedSet {
    private data: Object
    private seed: number
    constructor() {
        this.data = {}
        this.seed = 0
    }

    insert(val: number): boolean {
        if (this.data[val] !== undefined) {
            return false
        }
        this.data[val] = val
        return true
        
    }

    remove(val: number): boolean {
        if (this.data[val] !== undefined) {
            delete this.data[val]
            return true
        }
        return false
    }

    getRandom(): number {
        const keys = Object.keys(this.data)
        // Math.floor(Math.random() * keys.length);
        return this.data[keys[this.seed++%keys.length]]
    }
}
```

<br/>

#### 세 번째 시도 ✅

* 랜덤함수를 써서 통과했는데, 임의의 요소 반환과 확률동일에 대한 테케까지 준비해놨다는 사실에 좀 놀랐다.

```ts
class RandomizedSet {
    private data: Object
    private seed: number
    constructor() {
        this.data = {}
        this.seed = 0
    }

    insert(val: number): boolean {
        if (this.data[val] !== undefined) {
            return false
        }
        this.data[val] = val
        return true
        
    }

    remove(val: number): boolean {
        if (this.data[val] !== undefined) {
            delete this.data[val]
            return true
        }
        return false
    }

    getRandom(): number {
        const keys = Object.keys(this.data)
        
        return Number(keys[Math.floor(Math.random() * keys.length)])
    }
}
```

<br/>

---

## 238. Product of Array Except Self

:::note
### 문제 요약
* 주어진 리스트에서 각 인덱스의 자기 자신을 뺀 모든 요소를 곱한 값을 리스트로 반환하라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/product-of-array-except-self)
:::

<br/>

#### 첫 번째 시도 ✅

* 값이 0인 요소의 인덱스를 모으고 값 0이 아닌 요소는 모두 곱한다.
* 값이 0인 요소가 2개 이상이면 답은 모두 0으로 반환하고 1개라면 값이 0인 인덱스에만 모두 곱한 값을 삽입하여 반환한다.
* 0이 하나도 없다면 모두 곱한 값에서 각 인덱스의 값을 나눈 값을 리스트에 삽입하여 반환한다.

```ts
function productExceptSelf(nums: number[]): number[] {
    const index0 = []
    let multi = 1
    for (let i=0; i<nums.length; i++) {
        if (nums[i] === 0) {
            index0.push(i)
        } else {
            multi *= nums[i]
        }
    }

    const ans = Array.from(Array(nums.length), x => 0)
    if (index0.length === 1) {
        ans[index0[0]] = multi
    } else if (index0.length < 1) {
        for (let i=0; i<nums.length; i++) {
            ans[i] = multi/nums[i]
        }
    }

    return ans
};
```

<br/>


#### 더 나아가기

* `const multi = nums.reduce((cur, x) => x === 0 ? cur*1 : cur*x, 1)` 이런 식으로 좀 더 깔끔하게 변화시킬 수 있을 듯하다.

<br/>

---

## 134. Gas Station

:::note
### 문제 요약
* 각 인덱스에 도착하면 가스를 얻을 수 있고, 다음 인덱스로 진행하려면 비용을 내야한다.
* 얻을 수 있는 가스 리스트와 비용 리스트를 각각 제공하고 둘의 길이는 같다.
* 가스가 0이하로 떨어지지 않고 순회하는게 가능한 시작지점을 구하라.
* 순회할 수 없다면 -1을 반환하면 된다.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/gas-station)
:::

<br/>

#### 첫 번째 시도 ✅

* 전체 비용이 전체 얻을 수 있는 가스보다 높으면 무조건 못 가므로 먼저 체크해준다.
* `i` 지점에서 가스를 얻어 진행하면서 `j` 지점에서 가스가 0보다 낮게 된다면 `i ~ j` 지점 전체는 시작지점이 절대 될 수 없기 때문에 다음 인덱스에서 시작지점으로 가정하여 순회를 돈다.
* 순회가 끝나고 시작지점이 인덱스 범위 밖으로 나가면 순회가 불가능하고 인덱스 범위 안이라면 지정했던 시작지점을 반환한다.

```ts
function canCompleteCircuit(gas: number[], cost: number[]): number {
    if (gas.reduce((cur, x) => cur + x, 0) < cost.reduce((cur, x) => cur + x, 0)) {
        return -1
    }

    let [fuel, s] = [0, 0]
    for (let i=0; i<gas.length; i++) {
        fuel += gas[i] - cost[i]
        if (fuel < 0) {
            fuel = 0
            s = i+1
        }
    }

    if (s === gas.length) return -1
    return s
};
```

<br/>


#### 두 번째 시도 ✅

* `reduce`를 쓰면 n번씩 더 돌게 되므로 순회 안에 넣는 것으로 변경해 보았다.

```ts
function canCompleteCircuit(gas: number[], cost: number[]): number {
    let [fuel, s, totalGas, totalCost] = [0, 0, 0, 0]
    for (let i=0; i<gas.length; i++) {
        totalGas += gas[i]
        totalCost += cost[i]
        fuel += gas[i] - cost[i]
        if (fuel < 0) {
            fuel = 0
            s = i+1
        }
    }

    if (s === gas.length || totalGas < totalCost) return -1
    return s
};
```

<br/>

---

## 134. Gas Station

:::note
### 문제 요약
* 각 인덱스에 도착하면 가스를 얻을 수 있고, 다음 인덱스로 진행하려면 비용을 내야한다.
* 얻을 수 있는 가스 리스트와 비용 리스트를 각각 제공하고 둘의 길이는 같다.
* 가스가 0이하로 떨어지지 않고 순회하는게 가능한 시작지점을 구하라.
* 순회할 수 없다면 -1을 반환하면 된다.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/gas-station)
:::

<br/>

#### 첫 번째 시도 ✅

* 전체 비용이 전체 얻을 수 있는 가스보다 높으면 무조건 못 가므로 먼저 체크해준다.
* `i` 지점에서 가스를 얻어 진행하면서 `j` 지점에서 가스가 0보다 낮게 된다면 `i ~ j` 지점 전체는 시작지점이 절대 될 수 없기 때문에 다음 인덱스에서 시작지점으로 가정하여 순회를 돈다.
* 순회가 끝나고 시작지점이 인덱스 범위 밖으로 나가면 순회가 불가능하고 인덱스 범위 안이라면 지정했던 시작지점을 반환한다.

```ts
function canCompleteCircuit(gas: number[], cost: number[]): number {
    if (gas.reduce((cur, x) => cur + x, 0) < cost.reduce((cur, x) => cur + x, 0)) {
        return -1
    }

    let [fuel, s] = [0, 0]
    for (let i=0; i<gas.length; i++) {
        fuel += gas[i] - cost[i]
        if (fuel < 0) {
            fuel = 0
            s = i+1
        }
    }

    if (s === gas.length) return -1
    return s
};
```

<br/>


#### 두 번째 시도 ✅

* `reduce`를 쓰면 n번씩 더 돌게 되므로 순회 안에 넣는 것으로 변경해 보았다.

```ts
function canCompleteCircuit(gas: number[], cost: number[]): number {
    let [fuel, s, totalGas, totalCost] = [0, 0, 0, 0]
    for (let i=0; i<gas.length; i++) {
        totalGas += gas[i]
        totalCost += cost[i]
        fuel += gas[i] - cost[i]
        if (fuel < 0) {
            fuel = 0
            s = i+1
        }
    }

    if (s === gas.length || totalGas < totalCost) return -1
    return s
};
```

<br/>

---

## 135. Candy

:::note
### 문제 요약
* 아이들에 레이팅을 매기고 그 레이팅만큼 차순위를 두어 캔디를 준다.
* 캔디를 받는 것은 양 옆의 애보다 레이팅이 높으면 양 옆의 애의 캔디보다 더 받는다는 조건이 있다.
* 각 아이들에게는 하나의 캔디를 무조건 주어야 하고, 최소로 캔디를 줄 방법을 구하여라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/candy/)
:::

<br/>

#### 첫 번째 시도 ❌

* 리스트의 모든 요소에서 협곡같이 이전 인덱스에서는 내려가고 이후 인덱스에서는 올라가는 부분을 찾아서 캔디를 하나준다.
* 양 옆으로 더 큰 레이팅을 가진 아이에게 캔디를 하나 더 주며 더 이상 큰 레이팅이 없다면 그만둔다.
* 서로 제일 큰 레이팅을 가진 아이를 찾으며 충돌하는 경우에는 캔디를 둘 중 최대로 준다.
* 그리고 남은 부분은 양옆이 같은 레이팅인 아이들이므로 캔디를 하나만 준다.
* 그렇게 모든 아이들에게 준 캔디를 더한 값을 반환한다.
* 예외 케이스가 있어서 실패.

```ts
function candy(ratings: number[]): number {
    const valley = []
    for (let i=0; i<ratings.length; i++) {
        if ((i+1 === ratings.length || ratings[i+1] > ratings[i]) && (ratings[i] < ratings[i-1] || i-1 === -1)) {
            valley.push(i)
        }
    }
    const candy = Array.from(Array(ratings.length), x => 1)

    for (const s of valley) {
        let i = s
        while (ratings[i+1] > ratings[i]) {
            if (candy[i+1] > 1) {
                candy[i+1] = Math.max(candy[i+1], candy[i]+1)
            } else {
                candy[i+1] = candy[i] + 1
            }
            i++
        }

        i = s
        while (ratings[i-1] > ratings[i]) {
            if (candy[i-1] > 1) {
                candy[i-1] = Math.max(candy[i-1], candy[i]+1)
            } else {
                candy[i-1] = candy[i] + 1
            }
            i--
        }
    }
    return candy.reduce((cur, x) => cur + x, 0)
};
```

<br/>


#### 두 번째 시도 ✅

* 첫 번째 시도의 `리스트의 모든 요소에서 협곡같이 이전 인덱스에서는 내려가고 이후 인덱스에서는 올라가는 부분을 찾아서 캔디를 하나준다.` 를 내려가거나 같고, 올라가거나 같은 부분을 찾아서 라고 변경하여 시도해서 성공하였다.

```ts
function candy(ratings: number[]): number {
    const valley = []
    for (let i=0; i<ratings.length; i++) {
        if ((i+1 === ratings.length || ratings[i+1] >= ratings[i]) && (ratings[i] <= ratings[i-1] || i-1 === -1)) {
            valley.push(i)
        }
    }
    const candy = Array.from(Array(ratings.length), x => 1)

    for (const s of valley) {
        let i = s
        while (ratings[i+1] > ratings[i]) {
            if (candy[i+1] > 1) {
                candy[i+1] = Math.max(candy[i+1], candy[i]+1)
            } else {
                candy[i+1] = candy[i] + 1
            }
            i++
        }

        i = s
        while (ratings[i-1] > ratings[i]) {
            if (candy[i-1] > 1) {
                candy[i-1] = Math.max(candy[i-1], candy[i]+1)
            } else {
                candy[i-1] = candy[i] + 1
            }
            i--
        }
    }
    return candy.reduce((cur, x) => cur + x, 0)
};
```

<br/>

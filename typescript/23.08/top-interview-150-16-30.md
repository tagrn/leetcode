# 문제 16 - 30

## 12. Integer to Roman

:::note
### 문제 요약
* 주어진 숫자를 로마문자로 바꾸어라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/integer-to-roman)
:::

<br/>

#### 첫 번째 시도 ✅

* 먼저 로마문자를 리스트로 선언해두고 순회하며 4일 때는 다음 큰 문자를 데려와서 뒤에 현재 자리 문자를 붙여주고 답에 더해준다.
* 9일 때는 다다음 큰 문자를 데려와서 뒤에 현재 자리 문자를 붙여주고 답에 더해준다.
* 4, 9가 아닌 숫자 중 5이상일 경우에는 다음 큰 숫자 하나와 현재 자리 문자를 숫자-5 만큼 반복하여 더해준다.
* 3이하일 경우엔 숫자만큼 반복하여 답에 더해준다.

```ts
function intToRoman(num: number): string {
    const roman = ["I", "V", "X", "L", "C", "D", "M"]
    let ans = ""
    const reversedNum = String(num).split("").reverse()
    for (let i=0; i<reversedNum.length; i++) {
        if (reversedNum[i] === "9") {
            ans = roman[i*2] + roman[i*2+2] + ans
        } else if (reversedNum[i] === "4") {
            ans = roman[i*2] + roman[i*2+1] + ans
        } else if (Number(reversedNum[i]) > 4) {
            ans = roman[i*2+1] + roman[i*2].repeat(Number(reversedNum[i])-5) + ans
        } else {
            ans = roman[i*2].repeat(Number(reversedNum[i])) + ans
        }
    }

    return ans
};
```

<br/>

#### 더 나아가기

* 문자를 1, 10, 100, 1000 단위로 먼저 바꾼 후, 5, 50, 500 단위로 바꾸는 방법도 있다.

<br/>

---

## 151. Reverse Words in a String

:::note
### 문제 요약
* 주어진 문장을 단어 단위로 거꾸로 반환해라.
* 단어 사이에만 공백이 있어야 하며, 원래 문장에 공백이 많은 경우 하나만 남기고 제거해서 반환해야 한다.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/reverse-words-in-a-string)
:::

<br/>

#### 첫 번째 시도 ✅

* 문자열을 공백으로 분리된 리스트로 만들어서 빈 문자열을 제거한다.
* 그 후, 리스트를 뒤집고 리스트를 요소마다 공백과 함께 문자열로 합치면 된다.

```ts
function reverseWords(s: string): string {
    return s.split(" ").filter(x => x !== "").reverse().join(" ")
};
```

<br/>

#### 더 나아가기

* 양 옆의 공백을 제거하는 `trim`이 있다.
* 정규표현식을 이용하여 변경할 수도 있다.

<br/>

---

## 6. Zigzag Conversion

:::note
### 문제 요약
* 지그재그로 암호화된 문자열이 있다. 이 문자열의 암호화를 풀어서 반환하라.

<br/>

[**🧑🏻‍💻 LeetCode 문제 풀러가기**](https://leetcode.com/problems/zigzag-conversion)
:::

<br/>

#### 첫 번째 시도 ❌

* 지그재그 문자열을 2차원 배열로 변형시킨다.
* 변형시키고 해당 문자열을 첫 번째 row부터 마지막 row까지 합쳐서 반환한다.

```ts
function convert(s: string, numRows: number): string {
    const arr = Array.from(Array(numRows), x => [])
    for (const x of s) {
        arr[cnt].push(x)
        ch ? cnt++ : cnt--
        if (cnt === 0) ch = true
        if (cnt === numRows-1) ch = false
    }

    return arr.reduce((cur, x) => cur + x.reduce((cur, x) => cur + x, ""), "")
};
```

<br/>

#### 두 번째 시도 ✅

* 첫 번째 시도에서 `numRows`가 1일 때에 대한 예외케이스를 추가하여 성공했다.

```ts
function convert(s: string, numRows: number): string {
    const arr = Array.from(Array(numRows), x => [])
    let [cnt, ch] = [0, true]
    if (numRows === 1) {
        return s
    }

    for (const x of s) {
        arr[cnt].push(x)
        ch ? cnt++ : cnt--
        if (cnt === 0) ch = true
        if (cnt === numRows-1) ch = false
    }

    return arr.reduce((cur, x) => cur + x.reduce((cur, x) => cur + x, ""), "")
};
```

<br/>

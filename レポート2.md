
##  プログラミングII　2025　課題レポート（第2回）

# 解答用紙 → このファイルにレポートを書き込み、Githubへ提出する　
- 問題用紙をよく読み、この回答用紙にレポート記述して提出すること
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させること
- 作成したコード、動作確認（実行結果）、分析・考察を自分の言葉で記入する
- 生成AIの回答、他人のレポートをコピーしてはいけない（判明し次第、減点ペナルティ対象とする）

レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
|A | 20125004| 大野悠豊 |

------

# 解答編

- それぞれの問題をよく読み、以下の項目に自分の言葉でしっかり記述する
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させる

------

## **課題1. 関数の作成とスコープルールの理解**

### **問題 1-1: 温度変換関数**

**作成したpythonコード**

```python

absolute_zero_celsius = -273.15
absolute_zero_fahrenheit =-459.67

def celsius_to_fahrenheit(celsius: float) ->float:
   return celsius *9/5+32
def fahrenheit_to_celsius(fahrenheit: float) ->float:
   return (fahrenheit -32)*5/9
def c_2_f_2_c(c: float ) ->float:
   return celsius_to_fahrenheit(fahrenheit_to_celsius(c))
def f_2_c_2_f(f: float ) ->float:
   return fahrenheit_to_celsius(celsius_to_fahrenheit(f))

print("摂氏→華氏→摂氏:", c_2_f_2_c(absolute_zero_celsius))

print("華氏→摂氏→華氏:",f_2_c_2_f(absolute_zero_fahrenheit))

tolerance =1e-8
assert abs(c_2_f_2_c(absolute_zero_celsius)-absolute_zero_celsius) < tolerance,"摂氏変化エラー"
assert abs(f_2_c_2_f(absolute_zero_fahrenheit)-absolute_zero_fahrenheit) < tolerance,"華氏変化エラー"

print("動作確認:成功")

```

**動作確認結果**
```

摂氏→華氏→摂氏: -273.15
華氏→摂氏→華氏: -459.66999999999996
動作確認:成功

```

**分析** 
tolerance =1e-8で計算誤差を許容してる

**考察**  
自動的に検証しているからテストコードとしても使える


**改善点**  


---

### **★チャレンジ問題 (1-1-aux)　長さ・重さの単位変換**

**作成したpythonコード**

```python

ここにコードを書く

```

**動作確認結果**
```

ここに実行結果を書く

```

**分析** 

<!-- 
出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

**考察**  

<!-- 
  - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
  - 用いた技法、例えば、関数プログラミングの利点、プロトコルの活用場面について、応用シーンを考察する
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

----

### **問題 1-2: スコープルールの確認**

**作成したpythonコード**

```python

x =24
def outer():
   x =12

   def inner():
      nonlocal x
      x +=1
      print(f"Inner:{x}")

   inner()
   print(f"Outer:{x}")
   return inner

o =outer()
o()
print(f"Global:{x}")
p =outer()
p()
print(f"Global:{x}")
o()
o()
p()
print(f"Global:{x}")

```

**動作確認結果**
```

Inner:13
Outer:13
Inner:14
Global:24
Inner:13
Outer:13
Inner:14
Global:24
Inner:15
Inner:16
Inner:15
Global:24

```

**分析** 

このコードはnonlocalを使うことによって新しいローカル変数を作らずに関数xを変更することができる。

**考察**  



**改善点**  
変数をxじゃなくてvalueなどにしてわかりやすくする

----


### **★チャレンジ問題 (1-2-aux)**　グローバルコンテキスト、ローカルコンテキストの仕組み


**作成したpythonコード**

```python

ここにコードを書く

```

**動作確認結果**
```

ここに実行結果を書く

```

**分析** 

<!-- 
出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

**考察**  

<!-- 
  - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
  - 用いた技法、例えば、関数プログラミングの利点、プロトコルの活用場面について、応用シーンを考察する
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

----
----

## **課題2. 関数プログラミング**

### **問題 2-1: 高階関数と部分関数**

**作成したpythonコード**
```
python

from functools import partial

Three = 3


def divisible_by(n: int, d=Three) -> bool:
    return n % d == 0


def has_char(n: int, d=str(Three)) -> bool:
    return d in str(n)


def multiply_by(n: int, m=Three) -> int:
    return n * m

triples = partial(multiply_by, m=Three)


def combined_filter(n: int) -> bool:
    return divisible_by(n) and has_char(n)


def triple_three(numbers: list[int]) -> list[int]:
    filtered_numbers = filter(combined_filter, numbers)
    result = map(triples, filtered_numbers)
    return list(result)


if __name__ == "__main__":
    numbers = list(range(1, 100))
    result = triple_three(numbers)
    print(f"Result: {result}")
```
**動作確認結果**
```

[9, 90, 99, 108, 117, 189, 279]

```

**分析** 
divisible_by(),has_char(),multiply_by()など、単一の目的を持つ関数が明確に分離されている



**考察**  
divisible_by(n)とhas_cher(n)を組み合わせることで3の倍数かつ数字に3を含む複雑な条件を簡潔に表現していて、実用性の高い設計。


**改善点**  


### **問題 2-2: デコレータの実装**

**作成したpythonコード**

```python

import time
from functools import wraps

def timer(func):
   @wraps(func)

   def wrapper(*args, **kwargs):
      print(f"{func.__name__}(args={args}, kwargs={kwargs})")
      start = time.time()

      result = func(*args, **kwargs) 

      end = time.time() 
      print(f"実行時間: {end - start:.4f} 秒")
      return result
   return wrapper

@timer
def example(sec: int = 1) -> None:
    time.sleep(sec)
    print("関数が実行されました")

if __name__ == "__main__":
    example(sec=2)


```

**動作確認結果**
```

example(args=(), kwargs={'sec': 2})
関数が実行されました
実行時間: 2.0001 秒

```

**分析** 

実行結果の精度も高く、秒単位での測定ができている。

**考察**  

@timeはどんな関数にも適応できるため、再利用性が非常に高い

**改善点**  
秒だけでなく、ミリ秒や分などにも対応すると柔軟性が増す


## **課題3. プロトコルの実装**

### **問題 3-1: イテレータの実装**


**作成したpythonコード**

```python

class FibonacciIterator:
    def __init__(self, size: int):
        self.size = size
        self.count = 0
        self.a, self.b = 0, 1

    def __iter__(self):
        return self

    def __next__(self):
        if self.count >= self.size:
            raise StopIteration
        current = self.a
        self.a, self.b = self.b, self.a + self.b
        self.count += 1
        return current

fib_iter = FibonacciIterator(20)
for num in fib_iter:
    print(num, end=',')

```

**動作確認結果**
```

0,1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,1597,2584,4181

```

**分析** 
フィボナッチ数列の生成ロジックがself.a, self.b = self.b, self.a + self.b によって正確に次の値を正しく更新している。



**考察**  
yieldを使っていて複雑なロジックに向いている。

**改善点**  
start_a,start_bを引数にすることによって1,1,始まりにも対応できる

---

### **問題 3-2: コンテキストマネージャの作成**

**作成したpythonコード**

```python

class FileManager:
    def __init__(self, filename: str, mode: str):
        self.filename = filename
        self.mode = mode
        self.file = None

    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file

    def __exit__(self, exc_type, exc_value, traceback):
        if self.file:
            self.file.close()

```

**動作確認結果**
```

ここに実行結果を書く

```

**分析** 
コンテキストマネージャプロトコルに準拠に__enter__() と__exit__() を実装しており、with 構文で安全に使える



**考察**  

__exit__()の引数やexc_type, exc_value, traceback を使えば、ログ出力やエラーハンドリングを追加できる。




**改善点**  
self.file.closeを使うことでファイルが既に閉じられていないかを確認してから閉じることができます。

----
----

## **課題4. 再帰,lambda式**

### **問題 4-1: クイックソートの再帰的実装**
**作成したpythonコード**

```python

def quicksort(arr: list[int]) -> list[int]:
    if len(arr) <= 1:
        return arr  ）

    pivot = arr[0]  
    less = [x for x in arr[1:] if x <= pivot] 
    greater = [x for x in arr[1:] if x > pivot]  

    
    return quicksort(less) + [pivot] + quicksort(greater)

import random

randoms = [random.randint(-20, 80) for _ in range(15)]
print("元のリスト:", randoms)

qs = quicksort(randoms)
print("ソート後:", qs)


```

**動作確認結果**
```

元のリスト: [-18, -6, 9, 77, 63, -17, 46, 6, 5, 49, 77, 49, 76, 19, 22]
ソート後: [-18, -17, -6, 5, 6, 9, 19, 22, 46, 49, 49, 63, 76, 77, 77]

```

**分析** 
再帰構造がquicksort()関数で明確になっており、終了条件と再帰呼びが正しく構築されており、典型的な分割治法の流れに沿っている


**考察**  
リスト内包的表現と再帰を組み合わせることで、複雑なアルゴリズムも崇高で表現ができる点が印象的


**改善点**  
if not arrのように書くと、空リストの処理がより明確になる
----

### **問題 4-2: ハノイの塔の動作原理**
**作成したpythonコード**

```python

def hanoi(n: int, source: str, target: str, auxiliary: str) -> None:
    if n == 1:
        print(f"ディスク 1 を {source} から {target}へ移動")
        return
    hanoi(n - 1, source, auxiliary, target)  
    print(f"ディスク {n} を {source} から {target}へ移動")  
    hanoi(n - 1, auxiliary, target, source)  
hanoi(3, 'A', 'C', 'B')


```

**動作確認結果**
```

ディスク 1 を A から Cへ移動
ディスク 2 を A から Bへ移動
ディスク 1 を C から Bへ移動
ディスク 3 を A から Cへ移動
ディスク 1 を B から Aへ移動
ディスク 2 を B から Cへ移動
ディスク 1 を A から Cへ移動

```

**分析** 
hanoi(n, source, target, auxiliary)はn枚のディスクを三つも棒間で移動する処理を、再帰的に分割して実行している。



**考察**  
ハノイの塔は問題を小さく分けて解決する再帰の考え方を直感的に学べる。


**改善点**  
ステップ数のカウントを追加することで再帰の流れを追いやすくなり、何回の移動が必要かわかるようになる。


----

### **★チャレンジ問題 (4-2-aux)　lambdaを使った関数プログラミング**
**多数の英単語をカウント、ソートするlambda関数プログラミング**

**作成したpythonコード**

```python

ここにコードを書く

```

**動作確認結果**
```

ここに実行結果を書く

```

**分析** 

<!-- 
出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

**考察**  

<!-- 
  - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
  - 用いた技法、例えば、関数プログラミングの利点、プロトコルの活用場面について、応用シーンを考察する
-->

**改善点**  
<!-- 
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->


----

## 所感、自由記述欄
今回の課題では、前回よりも難しく自分にはまだプログラミングの勉強が足りていないと痛感しました。これからさらに難しくなっていくと思うので、モチベーションが低い日でも、ほんの数分でも勉強をして、後から後悔しないように、今のうちに少しずつでも頑張っていこうと思いました。
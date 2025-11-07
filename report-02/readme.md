
##  プログラミングII　2025　課題レポート（第2回）

# 解答用紙 → このファイルにレポートを書き込み、Githubへ提出する　
- 問題用紙をよく読み、この回答用紙にレポート記述して提出すること
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させること
- 作成したコード、動作確認（実行結果）、分析・考察を自分の言葉で記入する
- 生成AIの回答、他人のレポートをコピーしてはいけない（判明し次第、減点ペナルティ対象とする）

レポート提出者：

|クラス|学籍番号|　氏名　|
|B|20125047|星野日那|
|A or B（どちらか選択）| (記入する) | (記入する) |

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
absolute_zero_fahrenheit = -459.67

def celsius_to_fahrenheit(celsius: float) -> float:
    return (celsius * 9 / 5) + 32

def fahrenheit_to_celsius(fahrenheit: float) -> float:
    return (fahrenheit -32)*5 / 9

def c_2_f_2_c(c:float) -> float:
    return fahrenheit_to_celsius(celsius_to_fahrenheit(c))

def f_2_c_2_f(f: float) -> float:
    return celsius_to_fahrenheit(fahrenheit_to_celsius(f))

print("摂氏 → 華氏 → 摂氏:", c_2_f_2_c(absolute_zero_celsius))
print("華氏 → 摂氏 → 華氏:", f_2_c_2_f(absolute_zero_fahrenheit))

tolerance = 1e-8
assert abs(c_2_f_2_c(absolute_zero_celsius) - absolute_zero_celsius) < tolerance, "摂氏変換エラー"
assert abs(f_2_c_2_f(absolute_zero_fahrenheit) - absolute_zero_fahrenheit) < tolerance, "華氏変換エラー"
print("動作確認:成功")

```

**動作確認結果**
```

摂氏 → 華氏 → 摂氏: -273.15
華氏 → 摂氏 → 華氏: -459.66999999999996

```

**分析** 

<!-- 
関数定義や関数合成、浮動小数の誤差などの基本的なプログラミング概念を理解するため
-->

**考察**  

<!-- 
摂氏→華氏、華氏→摂氏の計算式を小さな関数に分けて作る。
それらをつなげて摂氏→華氏→摂氏に戻るのように関数を組み合わせる発想を身につける。
利点としては処理を小さな関数に分けることで読みやすく、テストしやすく、再利用しやすいこと。応用場面としては画像処理のフィルタ処理などがあると思います。
-->

**改善点**  
<!-- 
   今回やってみてプログラム自体は正しく動作したけど応用シーンまで自分一人でできないのでコードの中身をしっかり理解しないといけないと感じた
-->

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

x = 24

def outer():
    
    x = 12

    def inner():
        nonlocal x 
        x += 1
        print(f"Inner: {x}")

    inner()
    print(f"Outer: {x}")
    return inner

o = outer()
o()
print(f"Global: {x}")
p = outer()
p()
print(f"Global: {x}")
o()
o()
p()
print(f"Global: {x}")

```

**動作確認結果**
```

Inner: 13
Outer: 13
Inner: 14
Global: 24
Inner: 13
Outer: 13
Inner: 14
Global: 24
Inner: 15
Inner: 16
Inner: 15
Global: 24

```

**分析** 

<!-- 
関数の中に関数を作る仕組みと外側の変数を内側の関数で使うためのnonlocalの働きを理解するためのもの。
-->

**考察**  

<!-- 
外側の関数と内側の関数がそれぞれどんな役割を持っているか整理することが大切だった。また、outer() が返す inner() を変数に入れて何度も呼ぶことで、「関数が状態を覚えている」ことを確かめながら動作を追っていくと全体が理解しやすかった。
クリック回数を数えるボタンの処理などに役立つと思いました。
-->

**改善点**  
<!-- 
   今回の課題では、クロージャを利用して外側の変数を保持し続ける仕組みを勉強できましたが振り返ってみるといくつか改善点や別の書き方があると感じました。まず、inner() が状態を更新する処理を担当しているが、用途が増えると関数の中身が複雑になりやすいため、変数の管理を明確にするために、クラスを使って「状態を持つオブジェクト」として書く方法も考えられるのではないかなとおもいました。
-->

----


### **★チャレンジ問題 (1-2-aux)**　グローバルコンテキスト、ローカルコンテキストの仕組み


**作成したpythonコード**

```python

code = """
x = 24
def outer():
   x = 12
   
   def inner():
      nonlocal x
      x -= 1
      print(f"Inner: {x}")
      
   inner()
   print(f"Outer: {x}")
   
outer()
print(f"Global: {x}")
"""

global_context = {}
local_context = {}

exec(code, global_context, local_context)

print("Global Context:", global_context)
print("Local Context:", local_context)

```

**動作確認結果**
```

Inner: 11
Outer: 11
Global: 24

```

**分析** 

<!-- 
この課題は、exec() によるコード実行時に、グローバルコンテキストとローカルコンテキストが辞書として管理される仕組みを理解することが目的です。
また、文字列として記述したコードの中で、global・local・nonlocal がどのように解釈されるかを確認することで、**Python におけるスコープの階層構造を体系的に学ぶ意図があると考えます。
-->

**考察**  

<!-- 
  通常の関数によるスコープのふるまいを理解することと、exec() が指定した辞書をグローバル・ローカルとして使うことを意識して、どの変数がどこに残るかを追跡する。inner → outer → global の順に x の値がどう変化するかを確認しながら実行ログを読むと理解しやすいなと感じました。使った技法としてはexec() を使った動的コード実行や辞書型による実行コンテキスト管理です。
-->

**改善点**  
<!-- 
   改善点としてはexec() の動作が危険なケースもあるため、実行するコードの安全性チェックやサンドボックス化が必要であるのではないかと考えました。- 
-->

----
----

## **課題2. 関数プログラミング**

### **問題 2-1: 高階関数と部分関数**

**作成したpythonコード**

```python

Three = 3
from functools import partial

def divisible_by(n: int, d=Three) -> bool:
    return n % d == 0

def has_char(n: int, d=str(Three)) -> bool:
    return d in str(n)

def multiply_by(n: int, m=Three) -> int:
    return n*m

triples = partial(multiply_by, m=Three)

def combined_filter(n: int) -> bool:
    return divisible_by(n) and has_char(n)

def triple_three(numbers: list[int]) -> list[int]:

    filtered_numbers = filter(combined_filter,numbers)

    result = map(triples, filtered_numbers)
    return list(result)

if __name__ == "__main__":
    numbers = list(range(1, 100))
    result = triple_three(numbers)
    print(f"Result: {result}")

```

**動作確認結果**
```

Result: [9, 90, 99, 108, 117, 189, 279]

```

**分析** 

<!-- 
map・filter・部分適用（partial）・高階関数 といった基本的な技法をまとめて理解する問題。数値の判定処理を複数の関数に分けて組み合わせたり、部分関数を使って3倍専用の関数を作ることで、関数を部品として組み立てる考え方を身につける
-->

**考察**  

<!-- 
  この課題では、まず何をどう判定するかを小さな関数に分けて整理することが大事だと考えます。
divisible_by と has_char で判定処理を分けておくことで、combined_filter で簡単に条件を組み合わせられるようになる。また、部分適用を使って3倍専用の関数を先に作っておくと、その後の map 処理がとてもシンプルになります。
この問題では、map・filter・高階関数・部分適用（partial） など、関数プログラミングの基本的なテクニックを使いました。応用場面としては複数の条件でデータを絞り込むさいに利用できるのではないかなと考えます。
-->

**改善点**  
<!-- 
   map と filter を重ねて使う構成はスッキリしているものの、慣れていない人には少し読みにくい場合がある。そのため、リスト内包表記を使えば、条件と変換処理をひとまとまりで書けて、可読性を上げられるという代替案もあるのではないかなと思いました。
- 
-->

----

### **問題 2-2: デコレータの実装**

**作成したpythonコード**

```python

import time
from functools import wraps

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        end = time.perf_counter()
        elapsed = end - start
        print(f"{func.__name__}:{elapsed:6f}")
        return result
    return wrapper
@timer
def example(sec:int=1) -> None:
    time.sleep(sec)
    print("関数が実行されました")

if __name__ == "__main__":
    example(sec=2)

```

**動作確認結果**
```

example:2.000865

```

**分析** 

<!-- 
この課題は、デコレータの基本構造、@wraps によるメタデータ保持、time.perf_counter() を使った高精度計時、そして“ラップ関数 wrapper で前後処理を挿入する”という横断的関心事（ロギングや計測）の分離を身につけること。関数本体を一切変更せずに機能追加できる点を理解する課題だと思いました。
-->

**考察**  

<!-- 
    攻略法としては①計時の前後で時刻を取る→②元関数を実行→③差分を出力→④返り値をそのまま返す。
高階関数・デコレータ・functools.wraps・プロトコル的思考などを使いました。応用シーンとしては関数の実行時間計測があると思います。
-->

**改善点**  
<!-- 
    - 改善点としては出力フォーマットを選べるように小数点桁数や単位切り替え、関数因数の表示ができるのではないかなと感じました。
-->


----
----

## **課題3. プロトコルの実装**

### **問題 3-1: イテレータの実装**


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
        self.file = open(self.filename, self.mode, encoding="utf-8")
        return self.file
    
    def __exit__(self, exc_type, exc_value, trackback):
        if self.file and not self.file.closed:
            self.file.close()
        return False
    
with FileManager('test.txt', 'w') as f:
    f.write('Hello,World!')

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

## **課題4. 再帰,lambda式**

### **問題 4-1: クイックソートの再帰的実装**
**作成したpythonコード**

```python

def quicksort(arr: list[int]) -> list[int]:
    if len(arr) <= 1:
        return arr
    
    pivot = arr[0]
    less = [x for x in arr[1:] if x < pivot]
    greater = [x for x in arr[1:] if x >= pivot]
    return quicksort(less) + [pivot] + quicksort(greater)

import random
randoms = [random.randint(-20, 80) for _ in range(15)]
print(randoms)
qs = quicksort(randoms)
print(qs)

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

### **問題 4-2: ハノイの塔の動作原理**
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



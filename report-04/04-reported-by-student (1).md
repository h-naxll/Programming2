##  プログラミングII　2025　課題レポート（第4回）

# 解答用紙 → このファイルにレポートを書き込み、Githubへ提出する　
- 問題用紙をよく読み、この回答用紙にレポート記述して提出すること
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させること
- 作成したコード、動作確認（実行結果）、分析・考察を自分の言葉で記入する
- 生成AIの回答、他人のレポートをコピーしてはいけない（判明し次第、減点ペナルティ対象とする）

レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
|A or B（B）| (20125047) | (星野日那) |


### 選択する回答コース:  「〇〇」コース

- **骨太コース：SA級**：生成AIを使わず，ノーヒントで自力で解く骨太な勇者・猛者・トリックスターコース
- **庶民コース：AB級**：生成AIを使わず，ヒントにしたがってアプローチする一般的コース
- **ゆるふわコース：CD級**：生成AIや他力本願を前提とする ゆとりコース

### 選択する回答コース:  「〇〇〇〇」コース

**コース選択：アプローチの違い（道筋の険しさ）によって難易度・努力値を考慮する**
- **骨太コース：SA級**：生成AIを使わず，ノーヒントで自力で解く骨太な勇者・猛者・トリックスターコース
- **庶民コース：AB級**：生成AIを使わず，ヒントにしたがってアプローチする一般的コース
- **ゆるふわコース：CD級**：生成AIや他力本願を前提とするゆったりコース


### **問題1: シングルトンと属性制約を保証するメタクラスの設計**
- **概要**: メタクラスを使い、下記要件を満たす「AppConfigクラス」を**空欄を埋めて**設計せよ:
- **要件**
  - シングルトンパターン: クラスのインスタンスは1つしか生成できない
  - 属性制約: クラス内の属性名に制約:「属性名はすべて小文字」を課す
  - デフォルト値: 属性が指定されていない場合にデフォルト値を自動設定
- **課題**:
  1. **コード設計**:  空欄を埋めて、コードを完成させよ
  2. **分析**: なぜこの設計が有用か？実行結果の確認と検証
  3. **考察**: メタクラスを用いることで得られる利点を論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class SingletonMeta(type):
    """Singleton を強制するメタクラス"""
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]
class AttributeConstraintMeta(type):
    """属性名がすべて小文字で命令されることを保証するメタクラス"""
    def __new__(cls, name, bases, dct):
        for attr_name in dct:
            if not attr_name.islower and not attr_name.startswith("__"):
                raise TypeError(f"Attribute '{attr_name}' in class '{name}' must be in lowercase.")
        return super().__new__(cls, name, bases, dct)
    
class DefaultValuesMeta(type):
    """属性のデフォルト値を自動設定するメタクラス"""
    DEFAULT_VALUES = {
        'database' : 'sqlite'
    }
    def __call__(cls, *args, **kwargs):
        instance = super().__call__(*args, kwargs)
        for attr, default in cls.DEFAULT_VALUES.items():
            if not hasattr(instance,attr) or getattr(instance, attr) is None:
                setattr(instance, attr, default)
        return instance
    
class CombinedMetaClass(SingletonMeta, AttributeConstraintMeta,DefaultValuesMeta):
    pass


class AppConfig(metaclass = CombinedMetaClass):
    app_name = "MyApplication"
    version = "1.0"
    Port = 8080
    database = None

configA = AppConfig()
configB = AppConfig()

assert configA is configB
print(configA,configB)

print(configA.database)

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

PS C:\Users\uyu06\OneDrive\Desktop\PROG-Ⅱ> python prog-2.py
<__main__.AppConfig object at 0x0000020DA7706F90> <__main__.AppConfig object at 0x0000020DA7706F90>
sqlite
PS C:\Users\uyu06\OneDrive\Desktop\PROG-Ⅱ> 


```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-普通のクラス定義だけでなく、クラスが作られる段階で制約をかけることで、ミスをはやく見つけられることを学ぶ問題だと感じた
-シングルトンや
属性名の制約、デフォルト値の自動設定などを組み合わせることで、プログラムを動かすだけでなく、安全に設計する視点が重要であることを理解する目的があると感じた
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-どんな制約を強制するかを最初に考え、それをコードで表現することが大事だと感じた。
-完全にバグがないプログラムを作ることは難しいため、実行後に問題を見つけるのではなく、クラス定義や生成の段階でミスを防ぐという考え方が大事だと思った。
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-ちょっとしたタイプエラーが多くなかなか実行結果を確認できなかったから気を付ける
-
-

---
---

### **問題2: 「チューリングマシンの停止問題」「万能デバッガが存在しない理由」**
- **課題**:  
- メタ循環インタプリタに関係する「チューリングマシンの停止問題」について、
- 講義中に説明した比喩「どんなバグもみつける完璧なデバッガが存在しない」を交えて
- 理解したことを自分の言葉で説明せよ
- **考察**: 
- あわせて「ゲーデルの不完全性定理」をふまえてどんな教訓を示しているか分析・考察せよ
 
---

**説明・分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-プログラムを同じ仕組みのプログラムで実行・解釈する方法であって、一見するとプログラムの動作をすべて理解できるように思えるけど、停止問題が示すようにプログラムが必ず停止するかどうかを完全に判定することは不可能であるためメタ循環インタプリタを使ったとしても、すべてのプログラムの正しさやバグの有無を完全に調べることはできない。
-
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-オブジェクト指向では、Observerパターンのように役割を分けて設計することで、すべてを1つで管理しようとせず、変更に強いプログラムを作ることができる。完全な判定ができない状況でも問題を分割して扱う方法として有効だと感じた。
-ゲーデルの不完全性定理はどんなに厳密なルールを持つ体系でも証明できないことが必ずあるということを示してると思った。停止問題の定理から自分自身を完全に説明したり、完全にチェックしたりすることは限界があると感じました。
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-教科書や講義資料を見てもよく分からない部分があったから時間をかけて勉強する必要がある。
-
-

---
---

### **問題3: オブザーバパターンを保証するメタクラスの設計**
- **概要**: メタクラスを使い、特定のクラスが「モデル（Observable）」として動作し、他の「オブザーバ（Observer）」に変更を通知する仕組みを確実に実施するサンプルコード設計を**空欄を埋めて**完成させよ
  
- **要件**
1. **`ObserverMeta` メタクラス**  
   - Observable クラスが必ず、 `add_observer`、`remove_observer`、`notify_observers` メソッドを持つことを強制し、実装されていない場合、例外をスローする

2. **`Observable` クラス**  
   - `ObserverMeta`メタクラスを適用した**基底クラス**で、観察される対象を管理する仕組みを提供、オブザーバを登録・解除し、状態変化時に通知する

3. **`Observer` クラス**  
   - `update` メソッドを持つ**抽象基底クラス**で、具体的なオブザーバクラスが継承して実装

4. **具体例（`TemperatureModel` と `TemperatureDisplay`）**  
   - `TemperatureModel` は(温度という)状態を持つモデルで、`temperature` プロパティが変更されると自動的にオブザーバへ通知
   - `TemperatureDisplay` はモデルから通知を受け取り、画面に表示を更新するオブザーバの具体例
   - 観察対象モデル（`TemperatureModel`）の状態変更がオブザーバ(`TemperatureDisplay`)に確実に通知される仕組みが可能
 
- **課題**:
  1. **コード設計**:  空欄を埋めて、コードを完成させよ
  2. **分析**: なぜこの設計が有用か？実行結果の確認と検証
  3. **考察**: メタクラスを用いることで得られる利点を論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python
class ObserverMeta(type):
    """
    -Observable クラスが必ず、'add_observer'、'remove_observer'、'notify_observers'メソッドを持つことを強制する
    -実装されていない場合、エラーをスローする
    """
    def __new__(cls, name, bases, dct):
        required_methods = {"add_observer","remobe_observer","notify_observers"}
        implemented_methods = set(dct.keys()).union(*(set(dir(base))for base in bases))
        if not required_methods.issubset(implemented_methods):
            missing = required_methods - implemented_methods
            raise TypeError(f"Class {name} is missing required methods: {','.join(missing)}")
        return super().__new__(cls, name, bases, dct)
    
class Observable(metaclass=ObserverMeta):
    """
    -'ObserverMeta'メタクラスを適用した**基底クラス**:観察される対象を管理する仕組みを提供
    -オブザーバを登録・解除し、状態変化時に通知する
    """
    def __init__(self):
        self._observers = []

    def add_observer(self, observer):
        if observer not in self._observers:
            self._observers.append(observer)

    def remove_observer(self, observer):
        if observer in self._observers:
            self._observers.remove(observer)

    def notify_observers(self):
        for observer in self._observers:
            observer.update(self)

class Observer:
    """
    -'update'メソッドを持つ**抽象基底クラス**で、具体的なオブザーバクラスが継承して実装
    """
    def update(self,observable):
        raise NotImplementedError("The 'update' method must be implemented by subclasses.")
    
class TemperatureDisplay(Observer):
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"[Display] Model updated to {observable.temperature}℃ [Display]")

class TemperatureBrowser(Observer):
    def update(self, observable):
        if isinstance(observable, TemperatureModel):
            print(f"<Web> Model updated to {observable.temperature}℃</Web>")

class TemperatureModel(Observable):
    def __init__(self):
        super().__init__()
        self._temperature = 0

    @property
    def temperature(self):
        return self._temperature
    
    @temperature.setter
    def temperature(self, value):
        self._temperature = value
        self.notify_observers

def test():
    model = TemperatureModel()
    display = TemperatureDisplay()
    view = TemperatureBrowser()

    model.add_observer(display)
    model.add_observer(view)

    model.temperature = 25
    model.temperature = 30

    model.remove_observer(display)
    model.temperature = 35

    model.add_observer(display)
    model.temperature = 44

test()


```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

PS C:\Users\uyu06\OneDrive\Desktop\PROG-Ⅱ> python prog-2.py
[Display] Model updated to 25℃ [Display]
<Web> Model updated to 25℃</Web>
<Web> Model updated to 25℃</Web>
<Web> Model updated to 25℃</Web>
<Web> Model updated to 25℃</Web>
<Web> Model updated to 25℃</Web>
[Display] Model updated to 30℃ [Display]
<Web> Model updated to 30℃</Web>
<Web> Model updated to 35℃</Web>
<Web> Model updated to 25℃</Web>
[Display] Model updated to 30℃ [Display]
<Web> Model updated to 30℃</Web>
<Web> Model updated to 35℃</Web>
<Web> Model updated to 25℃</Web>
[Display] Model updated to 30℃ [Display]
<Web> Model updated to 30℃</Web>
<Web> Model updated to 25℃</Web>
[Display] Model updated to 30℃ [Display]
<Web> Model updated to 25℃</Web>
<Web> Model updated to 25℃</Web>
<Web> Model updated to 25℃</Web>
[Display] Model updated to 30℃ [Display]
<Web> Model updated to 30℃</Web>
<Web> Model updated to 35℃</Web>
<Web> Model updated to 44℃</Web>
[Display] Model updated to 44℃ [Display]
<Web> Model updated to 44℃</Web>
[Display] Model updated to 44℃ [Display]
PS C:\Users\uyu06\OneDrive\Desktop\PROG-Ⅱ>

<Web> Model updated to 44℃</Web>
[Display] Model updated to 44℃ [Display]
PS C:\Users\uyu06\OneDrive\Desktop\PROG-Ⅱ>
<Web> Model updated to 44℃</Web>
<Web> Model updated to 44℃</Web>
<Web> Model updated to 44℃</Web>
[Display] Model updated to 44℃ [Display]
<Web> Model updated to 44℃</Web>
[Display] Model updated to 44℃ [Display]
<Web> Model updated to 44℃</Web>
<Web> Model updated to 44℃</Web>
[Display] Model updated to 44℃ [Display]
PS C:\Users\uyu06\OneDrive\Desktop\PROG-Ⅱ>

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-Observerパターンとメタクラスの使い方を理解しているかを確かめるためのものだとおもった。ObservableクラスとObserverクラスに役割を分けることで、データが変化したときに、自動で別の処理が実行される仕組みを作っている。
-
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-温度の値が変わったときに表示側が自動で更新される仕組みを作る問題だと思う
-メタクラスで必そうなメソッドがそろっているかクラス定義時にチェックすることで実行してからバグに気付くのではなく、早い段階でミスを見つけられるのが大事だと感じた
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-エラーが多く、そのエラー内容が理解できなくてAIに投げたしまったのでエラーの意味も理解できるようにする
-
-

-----
-----


### **問題4: メタクラスではなく、`__init_subclass__()`で代用する方法**
- **課題**: 問題３をシンプルに、`__init_subclass__()`で代用するコードを**空欄を埋めて**完成させよ



---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class Observable:
   def __init__(self):
      self._observers = []

   def add_observer(self, observer):
      if observer not in self._observers:
         self._observers.append(observer)

   def remove_observer(self, observer):
      if observer in self._observers:
         self._observers.remove(observer)

   def notify_observers(self):
      for observer in self._observers:
         observer.opdate(self)

    @classmethod
    def __init_subclass__(cls,**kwrgs):
      super().__init_subclass__(**kwrgs)
      required_methods = {"add_observer","remove_observer", "notify_observers"}
      for method in required_methods:
         if not callable(getattr(cls,method,None)):
            raise TypeError(f"Class{cls.__name__}is missing required method: {method}")
class Observer:
   def update(self,observable):
      raise NotImplementedError("The 'update' method must be implemented by subclasses.")

class TemperatureModel(Observable):
   def __init__(self):
      super().__init__()
      self._temperature = 0

    @property
    def temperature(self):
      return self._temperature
    
    @temperature.setter
    def temperature(self, value):
      self._temperature = value
      self.notify_observers()

class TemperatureDisplay(Observer):
   def update(self, observable):
      if isinstance(observable, TemperatureModel):
         print(f"Temperature updated to {obsevable.temperature}℃")

    def test():
      temp_model = TemperatureModel()
      display = TemperatureDisplay()

      temp_model.add_observer(display)

      temp_model.temperature = 25
      temp_model.temperature = 30

      temp_moddel.remove_observer(display)
      temp_model.temperature = 35

test()

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

PS C:\Users\uyu06\OneDrive\Desktop\PROG-Ⅱ> python prog-2.py
Temperature updated to 25℃
Temperature updated to 30℃
PS C:\Users\uyu06\OneDrive\Desktop\PROG-Ⅱ> 

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-Observerパターンとクラスの継承関係を理解しているかを確認するためのものだと思った。
-__init_subclass__を使うことでObservableを継承したクラスが、必要なメソッドを正しく実装しているかをクラス定義の時点でチェックしている点も重要だと感じた。実行してからエラーになるのではなくて設計段階でミスを防ぐための仕組みだと分かった。
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-オブジェクト指向の考え方を使っていると思った。オブジェクト指向を使うことで、温度データを扱うクラスと表示を行うクラスを分け、変更に強いプログラムになっている
-
-GUIアプリやWebアプリで、データが更新されたときに画面を自動で更新するような場面で使えると思う

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-
-
-

----
----

### **問題5: 自作パッケージとデプロイの実践
- **概要**: 自作パッケージを作って、デプロイし、他者が`pip insatll`や、`import`を使って活用できる状態にする
- 第12回の練習課題（プラクティスNo.1，No.2）を参考に、**自作パッケージを作ってデプロイするプラクティスNo.3**を実施する
- 簡単なユーティリティ関数を束ねたパッケージを自作する
- 例えば、次のような簡単な関数や、その他有用な関数を追加する：
- 

    - 1. BMI計算関数: 身長と体重を入力として受け取り、BMI（Body Mass Index）を計算して返す関数 : `calculate_bmi(weight, height)`
    - 2. 温度変換関数: 摂氏温度を華氏温度に変換する関数:`celsius_to_fahrenheit(celsius)`と、その逆変換:`fahrenheit_to_celsius(fahrenheit)`
    - 3. 単語カウント関数: テキスト内の単語数をカウントする関数:`count_words(text)`
    - 4. リストの平均値計算関数:`calculate_average(numbers)`
    - 5. 素数判定関数:`is_prime(number)`
    - 6. フィボナッチ数列生成関数:` generate_fibonacci(n)`
    - 7. 文字列の逆順関数:`reverse_string(s)`
    - 8. 最大公約数（GCD）計算関数:`gcd(a, b)`

----
---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

ここにコードを書く

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

ここに実行結果を書く

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-
-
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-
-
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-
-
-

-----

### 所感・今後の目標課題 (自由記述欄、フィードバックご意見を尊重します)

- プログラミングに興味が湧かない原因
- プログラミングには興味がないが、それ以外で没頭していること
- 授業中に質問や意見が言えない理由
- コードや英語や文字、活字が好きにならない理由
- 私とプログラミング
- なぜプログラミングを学ぶのか？
- これまで学んだことを振り返り
- これからの学びに活かすべき知見
 
---
仕事で使いこなすうえでPython以外にも他の言語をやったほうがいいと聞いてPythonでもすごく手こずっているのに他の言語までできる気がしない



---


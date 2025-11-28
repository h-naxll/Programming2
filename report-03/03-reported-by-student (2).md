##  プログラミングII　2025　課題レポート（第3回）

# 解答用紙 → このファイルにレポートを書き込み、Githubへ提出する　
- 問題用紙をよく読み、この回答用紙にレポート記述して提出すること
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させること
- 作成したコード、動作確認（実行結果）、分析・考察を自分の言葉で記入する
- 生成AIの回答、他人のレポートをコピーしてはいけない（判明し次第、減点ペナルティ対象とする）

レポート提出者：

|クラス|学籍番号|　氏名　|
|-|-|-|
|A or B（B）| (20125047) | (星野日那) |

# 解答編

- それぞれの問題をよく読み、以下の項目に自分の言葉でしっかり記述する
- マークダウン記法を、しっかりプレビューして、記述ミスはすべて修正して完成させる

### 選択する回答コース:  「〇〇」コース

- **骨太コース：SA級**：生成AIを使わず，ノーヒントで自力で解く骨太な勇者・猛者・トリックスターコース
- **庶民コース：AB級**：生成AIを使わず，ヒントにしたがってアプローチする一般的コース
- **ゆるふわコース：CD級**：生成AIや他力本願を前提とする ゆとりコース



### **問題1: ジェネレータを使ったコルーチンの設計と実装**
- **概要**: Pythonジェネレータを使って生産者-消費者問題を解決するコードを設計せよ
- **課題内容**:
  1. 要件：生産者コルーチンがデータを生成し、消費者コルーチンがデータを消費するコード
  2. `send`と`receive = yield`を用いた協調的並列処理の実装
  3. **分析**: なぜこの設計が必要か？実行結果の確認と検証
  4. **考察**: コルーチンを用いることで得られる効率性や設計上の利点を論じる
  5. **応用（チャレンジ）** 　ジェネレータに対する`StopIteration`例外を使うことで、生産者が生産を終了した後、消費者も消費を停止するコードに進化せよ

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

import time
import random

def producer (consumer_coroutine):
   print("[生産者]: 生産準備OK...")
   consumer_coroutine.send(None)
   for i in range(5):
      duration = random.uniform(0.5, 1.5)
      time.sleep(duration)
      item = f"商品{i}"
      print(f"\n---[生産者]: 所要時間{duration:.2f}秒で, {item} を生産")
      consumer_coroutine.send(item)
   consumer_coroutine.close()
   print("\n---[生産者]: 生産終了")

def consumer():
   print("<消費者>: 準備OK...")
   while True:
      item = yield
      print(f"<消費者>: {item} 受領... 消費中...")
      duration = random.uniform(0.5, 1.5)
      time.sleep(duration)
      print(f"<消費者>: 消費完了")

if __name__ == "__main__":
   consumer_coroutine = consumer()
   producer(consumer_coroutine)

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ> python prog-2.py
[生産者]: 生産準備OK...
<消費者>: 準備OK...

---[生産者]: 所要時間1.15秒で, 商品0 を生産
<消費者>: 商品0 受領... 消費中...
<消費者>: 消費完了

---[生産者]: 所要時間1.28秒で, 商品1 を生産
<消費者>: 商品1 受領... 消費中...
<消費者>: 消費完了

---[生産者]: 所要時間0.55秒で, 商品2 を生産
<消費者>: 商品2 受領... 消費中...
<消費者>: 消費完了

---[生産者]: 所要時間1.46秒で, 商品3 を生産
<消費者>: 商品3 受領... 消費中...
<消費者>: 消費完了

---[生産者]: 所要時間1.29秒で, 商品4 を生産
<消費者>: 商品4 受領... 消費中...
<消費者>: 消費完了

---[生産者]: 生産終了
PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ>

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-yieldを使ったコルーチンの動作を学ばせようとしている。特にconsumer()　は通常の関数ではなくコルーチンとして動く
yieldによって外部から値を受け取る
.send()によってコルーチンを再開しデータを送る
.send(None)で初期化する必要がある。
最後はclose()で終了できる一連の流れを理解させるための実装だと思います。
-
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-一つのスレッドの中でもyield　と　send()　を使うことで生産者と消費者が強調しながら処理を進められることを理解できました。
-
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-for i in range(5): で生産する個数がコードに埋め込まれていますが、個数を変えたいときにソースを直接書き換えないといけないので、引数や定数で指定できるようにしたほうが再利用性が高くなるのではないかなと感じました。
-
-

---
---

### **問題2: 基本的なオブジェクト指向の例題**
- **概要**: 図形クラスとその派生クラスを設計し、オブジェクト指向の原則の理解度を確認する
- **課題内容**:
  1. 抽象基底クラス`Shape`を定義し、以下のクラスを継承して設計実装せよ
     - 四角形クラス`Rectangle`
     - 三角形クラス`Triangle`
     - 楕円クラス`Ellipse`
     - 五角形クラス`Pentagon`
  2. 各クラスに`rotate`メソッドを実装し、「指定角度（degree）だけ時計回りに回転させる」機能を追加しなさい（シミュレーションでよい、厳密な行列・ベクトルを用いた幾何学的回転の実装は不要）
  3. ポリモーフィズムの概念を用いた操作を記述せよ
  4. **考察**: 継承、抽象基底クラスによるインターフェースの強制、ポリモーフィズムのメリットと、設計上の効果を分析しなさい



---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def rotate(self, angle):
        pass
    
class Shape2D(Shape):
    def rotate(self, angle):
        if not hasattr(self, "history"):
            self.history = []

        before = self.angle
        after = (self.angle + angle) % 360

        self.history.append({
            "before": before,
            "delta": angle,
            "after": after,
        })

class Rectangle(Shape2D):
    def __init__(self, width, height, angle=0):
        self.width = width
        self.height = height
        self.angle = angle

    def rotate(self, angle):
        super().rotate(angle)
        print(f'calc rotate (width,height)={self.width,self.height}@{self.angle}')

class Triangle(Shape2D):
    def __init__(self, base, height, angle=0):
        self.base = base
        self.height = height
        self.angle = angle

    def rotate(self, angle):
        super().rotate(angle)
        print(f'calc rotate (base,height)={self.base,self.height}@{self.angle}')

class Ellipse(Shape2D):
    def __init__(self, major_axis, minor_axis, angle=0):
        self.major = major_axis
        self.minor = minor_axis
        self.angle = angle

    def rotate(self, angle):
        super().rotate(angle)
        print(f'calc rotate (major,minor)={self.major,self.minor}@{self.angle}')

class Pentagon(Shape2D):
    def __init__(self, side_length, angle=0):
        self.side = side_length
        self.angle = angle
      
    def rotate(self, angle):
        super().rotate(angle)
        print(f'calc rotate (side)={self.side}@{self.angle}')

shapes = [
    Rectangle(10, 5, angle=270),
    Triangle(6,8, angle=90),
    Ellipse(5,3,angle=330),
    Pentagon(7,angle=-180)
]

for shape in shapes:
    shape.rotate(30)

print("\n=== 回転履歴 ===")
for shape in shapes:
    print(f"\n{shape.__class__.__name__} の履歴:")
    for i, h in enumerate(getattr(shape, "history", []), start=1):
        print(f" #{i}: before={h['before']}, delta={h['delta']}, after={h['after']}")

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ> python prog-2.py
calc rotate (width,height)=(10, 5)@270
calc rotate (base,height)=(6, 8)@90
calc rotate (major,minor)=(5, 3)@330
calc rotate (side)=7@-180

=== 回転履歴 ===

Rectangle の履歴:
 #1: before=270, delta=30, after=300

Triangle の履歴:
 #1: before=90, delta=30, after=120

Ellipse の履歴:
 #1: before=330, delta=30, after=0

Pentagon の履歴:
 #1: before=-180, delta=30, after=210
PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ>

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-抽象クラスと継承、そしてメソッドのオーバーライドを通して、共通のインターフェースを持つ複数のクラスを統一的に扱うというオブジェクト指向の基本概念を理解させるためのものだと思います
-図形ごとにrotateの中身は違うけれど、すべての図形はrotateを持っているという共通ルールを抽象クラスと@abstractmethodで保証しています。
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-オブジェクト指向の効果としては共通部分をまとめて再利用できること、図形ごとに異なる計算だけを実装すればいいこと、型に依存しないコードが書けて拡張しやすいことです
-活用場面はゲームのキャラクターやオブジェクトに使えると思いました。
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-回転履歴のデータ形式を統一するとより扱いやすいのではないかなと思いました。
-
-

---

### **問題3: **コマンド(Command)** デザインパターンの実装**
- **概要**: テキストエディタに対する編集操作と`Undo`/`Redo`機能をコマンドデザインパターンで実装せよ
- **課題内容**:
  1. 基本的なコマンドクラス`Command`を設計
  2. 具体的なコマンドとして、`InsertText`、`DeleteText`を実装しなさい（シミュレーションでよい）
  3. `Undo`/`Redo`機能を有効にする履歴管理を追加しなさい
  4. **考察**: コマンドパターンがもたらす柔軟性と拡張性について分析

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class Command:
    def execute(self):
        raise NotImplementedError

    def undo(self):
        raise NotImplementedError


class InsertText(Command):
    def __init__(self, document, text, position):
        self.document = document
        self.text = text
        self.position = position

    def execute(self):
        self.document.insert(self.text, self.position)

    def undo(self):
        self.document.delete(self.position, len(self.text))

    def __repr__(self):
        return f"(挿入:'{self.text}'@{self.position})"


class DeleteText(Command):
    def __init__(self, document, position, length):
        self.document = document
        self.position = position
        self.length = length
        self.deleted_text = ""

    def execute(self):
        self.deleted_text = self.document.content[self.position:self.position + self.length]
        self.document.delete(self.position, self.length)

    def undo(self):
        self.document.insert(self.deleted_text, self.position)

    def __repr__(self):
        return f"(削除:'{self.deleted_text}'@{self.position}から{self.length}文字)"



class Document:
    def __init__(self):
        self.content = ""

    def insert(self, text, position):
        self.content = self.content[:position] + text + self.content[position:]
        print(f"Inserted '{text}' at {position}.\nCurrent content: '{self.content}'")

    def delete(self, position, length):
        self.content = self.content[:position] + self.content[position + length:]
        print(f"Deleted text at {position} with length {length}.\nCurrent content: '{self.content}'")


class CommandManager:
    def __init__(self, max_history=5):
        self.undo_stack = []
        self.redo_stack = []
        self.max_history = max_history

    def execute_command(self, command):
        command.execute()
        self.undo_stack.append(command)

        if len(self.undo_stack) > self.max_history:
            dropped = self.undo_stack.pop(0)
            print(f"[Info] 履歴上限のため古いコマンドを破棄: {dropped}")

        self.redo_stack.clear()

    def undo(self):
        if self.undo_stack:
            command = self.undo_stack.pop()
            command.undo()
            self.redo_stack.append(command)
        else:
            print("Nothing to undo.")

    def redo(self):
        if self.redo_stack:
            command = self.redo_stack.pop()
            command.execute()
            self.undo_stack.append(command)
        else:
            print("Nothing to redo.")


if __name__ == "__main__":
    document = Document()

    edit_actions = [
        InsertText(document, "Peace", 0),
        InsertText(document, " and Love ", 5),
        InsertText(document, " for the World", 15),
        DeleteText(document, 6, 9),
    ]
    print(edit_actions)

    manager = CommandManager(max_history=3)
    for ed in edit_actions:
        manager.execute_command(ed)

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ> python prog-2.py
[(挿入:'Peace'@0), (挿入:' and Love '@5), (挿入:' for the World'@15), (削除:''@6から9文字)]
Inserted 'Peace' at 0.
Current content: 'Peace'
Inserted ' and Love ' at 5.
Current content: 'Peace and Love '
Inserted ' for the World' at 15.
Current content: 'Peace and Love  for the World'
Deleted text at 6 with length 9.
Current content: 'Peace  for the World'
[Info] 履歴上限のため古いコマンドを破棄: (挿入:'Peace'@0)
PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ>

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-Undo　Redoの仕組みをオブジェクト指向的に実現する方法を学ぶためのものです。すべての編集操作をコマンドとしてクラス化し、その履歴をCommandManagerが管理する設計を理解させることが目的だと思いました。
-
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-テキスト編集の操作をそのままif文を多く使って書くのではなく一つ一つの編集操作をオブジェクトとして扱うことでUndo Redoを簡単に
実現できるようにしています。
-挿入や削除の処理をそれぞれInsertTextやDeleteTextクラスに分けて実装し、CommandManagerがそれらをスタックで管理することで、直前の操作を取り消す　取り消した操作をやり直すという流れを共通の仕組みで扱えるようにしました。
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-範囲チェックやエラーハンドリングがないので範囲チェックや例外処理を入れると現実のエディタに近くなると感じました。
-
-

-----
-----

### **問題4: オブザーバ(Observer) デザインパターンの実装**
- **概要**: Pythonのダックタイピングを活用して**オブザーバパターン**を実装し、MVCモデルについて論じよ
- **課題内容**:
  1. `Observer`と`Subject`を設計し、観察者-被観察者関係を構築
  2. サンプルアプリとして簡易的なMVCモデルを実装
  3. 各コンポーネントの役割を論述
  4. **考察**: ダックタイピングの利点と、オブザーバパターンによる柔軟なシステム設計についてパターンを使わない場合と比較し，効能・使い道，論じる

---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

class Command:
    def execute(self):
        raise NotImplementedError

    def undo(self):
        raise NotImplementedError


class InsertText(Command):
    def __init__(self, document, text, position):
        self.document = document
        self.text = text
        self.position = position

    def execute(self):
        self.document.insert(self.text, self.position)

    def undo(self):
        self.document.delete(self.position, len(self.text))

    def __repr__(self):
        return f"(挿入:'{self.text}'@{self.position})"


class DeleteText(Command):
    def __init__(self, document, position, length):
        self.document = document
        self.position = position
        self.length = length
        self.deleted_text = ""

    def execute(self):
        self.deleted_text = self.document.content[self.position:self.position + self.length]
        self.document.delete(self.position, self.length)

    def undo(self):
        self.document.insert(self.deleted_text, self.position)

    def __repr__(self):
        return f"(削除:'{self.deleted_text}'@{self.position}から{self.length}文字)"


class Document:
    def __init__(self):
        self.content = ""

    def insert(self, text, position):
        self.content = self.content[:position] + text + self.content[position:]
        print(f"Inserted '{text}' at {position}.\nCurrent content: '{self.content}'")

    def delete(self, position, length):
        self.content = self.content[:position] + self.content[position + length:]
        print(f"Deleted text at {position} with length {length}.\nCurrent content: '{self.content}'")


class CommandManager:
    def __init__(self, max_history=5):
        self.undo_stack = []
        self.redo_stack = []
        self.max_history = max_history

    def execute_command(self, command):
        command.execute()
        self.undo_stack.append(command)

        if len(self.undo_stack) > self.max_history:
            dropped = self.undo_stack.pop(0)
            print(f"[Info] 履歴上限のため古いコマンドを破棄: {dropped}")

        self.redo_stack.clear()

    def undo(self):
        if self.undo_stack:
            command = self.undo_stack.pop()
            command.undo()
            self.redo_stack.append(command)
        else:
            print("Nothing to undo.")

    def redo(self):
        if self.redo_stack:
            command = self.redo_stack.pop()
            command.execute()
            self.undo_stack.append(command)
        else:
            print("Nothing to redo.")


class Model:
    def __init__(self, name):
        self._name = name
        self._data = None

    def set_data(self, data):
        self._data = data

    def get_data(self):
        return self._data
    
    def get_name(self):
        return self._name
    
    def serialize(self):
        return {"data": self._data}
    
    def deserialize(self, data):
        self._data = data["data"]


class Observer:
    def update(self,model):
        pass
    

class HTMLView(Observer):
    def update(self, model):
        print(f"<div class='html-view'>HTMLView ▶ {model.get_name()} → {model.get_data()}</div>")


class CSVView (Observer):
    def update(self, model):
        print(f"CSVView,NAME={model.get_name()},DATA={model.get_data()}")


class PDFView(Observer):
    def update(self, model):
        print(f"★PDFView★ {model.get_name()} の状態が '{model.get_data()}' に更新されました。")


class GUIView(Observer):
    def update(self, model):
        print(f"□ GUIView: TEXT_FIELD='{model.get_name()}'→ '{model.get_data()}' ✓更新完了")

class Subject:
    def __init__(self):
        self._observers = []

    def register_observer(self, observer):
        self._observers.append(observer)

    def unregister_observer(self, observer):
        self._observers.remove(observer)

    def notify_observers(self, model):
        for observer in self._observers:
            observer.update(model)


class Controller(Subject):
    def __init__(self, model):
        super().__init__()
        self.model = model

    def set_data(self, data):
        self.model.set_data(data)
        self.notify_observers(self.model)


if __name__ == "__main__":
    model1 = Model("主人公キャラ")
    model2 = Model("仲間キャラ")

    html_view = HTMLView()
    csv_view = CSVView()
    pdf_view = PDFView()
    gui_view = GUIView()

    controller1 = Controller(model1)
    controller2 = Controller(model2)

    controller2.register_observer(pdf_view)
    controller2.register_observer(gui_view)

    controller1.set_data("イベント通知: 主人公のHPが全回復しました")
    controller2.set_data("イベント通知: 仲間キャラが新スキルを習得しました")

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ> python prog-2.py   
★PDFView★ 仲間キャラ の状態が 'イベント通知: 仲間キャラが新スキルを習得しました' に更新されました。
□ GUIView: TEXT_FIELD='仲間キャラ'→ 'イベント通知: 仲間キャラが新スキルを習得しました' ✓更新完了
PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ>


```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-Observerパターンを通じてデータの変化を複数の画面に伝える仕組みを理解するためのものだと感じました。Modelが更新されたとき、直接Viewを書き換えるのではなく、ControllerがObserverに通知することで処理を分離している。こうした設計を学ぶことで、データと表示をゆるく結びつける疎結合のメリットや、クラス同士が直接依存しないように構造化する考え方を身につけることが狙いだと思います。
-
-
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-Command,InsertText,DleteText　→　Undo/Redoを司る操作履歴の管理
Document→編集の対象となるテキスト
CommandManager　→　実行・取り消し・やり直しの制御
Model　→データを保持する主体
Observer各種　→　画面表示の更新担当
Controller　→　Modelの変更をまとめて扱う　このように各要素の責務を明確にすることで、コードの理解が進みバグの切り分けもしやすくなると感じました。
-
-Commandパターンの効果としては
・操作の履歴を簡単に保存できる
・UndoとRedoが自然に実装できる
・新しい操作を追加してもCommandを追加するだけで済む
・Document本体のコードが複雑にならない

活用場面としてはテキストエディタや画像編集ソフトがあると思いました。

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-今の　InsertText　は　Documentに強く依存していると思います。

self.document.content[...]
self.document.insert(...)
self.document.delete(...)
　という形で編集対象がDocument以外に広げにくい構造になっています。もっと汎用性をあげるなら
・文字列編集用のインターフェースを定義する
・あるいはエディタ的な役割のクラスを挟んでCommandはそこにだけ依存するといった形にしたほうが、再利用性やテストのしやすさは上がると思いました。
-
-

----
----

### **チャレンジ問題5: 非同期処理の実装と分析**　（骨太コース必須）
- **概要**: `async`と`await`を使用した非同期処理の設計と実装を通じて、非同期プログラミングを理解する
- **課題内容**:
  1. `async`と`await`を使用して複数の非同期タスクを並列実行するコード（例：Webスクレイピング、ファイル入出力）のサンプル例を実行し、動作原理を理解する
  2. 実行時間の比較実験を行い、同期処理との差を示す
  3. 非同期処理の利点、欠点、適用例を分析し、同期処理との違いを考察して，自分の言葉で説明せよ

----
---
**作成したpythonコード、特に空欄やポイントの説明**
<!--
   - 動作するコードを完成させて、変数・関数・クラスの役割やロジックを簡潔に説明する
   - 意図的な例外処理・失敗コードはその旨、説明すること
-->

```python

import time
import requests
import asyncio
import aiohttp

URLs = [
    "https://example.com/",
    "https://www.yahoo.co.jp/",
    "https://news.yahoo.co.jp",
]

async def fetch_url(session, url):
    print(f"[async]{url}にアクセス開始")
    start_t = time.time()
    async with session.get(url) as response:
        text = await response.text()
    elapsed = time.time() - start_t

    lines = text.count("\n")
    print(f"[async] {url}の取得完了({elapsed:.2f} 秒, {lines} 行)")
    return text

async def write_to_file(filename, content):
    with open(filename, "w", encoding= "utf-8") as file:
        file.write(content)
    print(f"[async] {filename} に書き込み完了")

async def async_tasks():
    urls = URLs
    async with aiohttp.ClientSession() as session:
        fetch_tasks = [fetch_url(session, url) for url in urls]
        results = await asyncio.gather(*fetch_tasks)

        write_tasks = [
            write_to_file(f"async_result_{i}.text", content)
            for i, content in enumerate(results)
        ]
        await asyncio.gather(*write_tasks)

def sync_tasks():
    urls = URLs
    results = []

    for url in urls:
        print(f"[sync]{url}にアクセス開始")
        start_t = time.time()
        response = requests.get(url)
        elapsed = time.time() - start_t
        text = response.text

        lines = text.count("\n")
        print(f"[sync]{url} の取得完了 ({elapsed:.2f} 秒, {lines} 行)")
        results.append(text)

    for i, content in enumerate(results):
        filename = f"sync_result_{i}.txt"
        with open(filename, "w",encoding="utf-8") as file:
            file.write(content)
        print(f"[sync]{filename}に書き込み完了")


if __name__ == "__main__":
    print("=== 非同期処理開始 ===")
    try:
        start_time = time.time()
        asyncio.run(async_tasks())
        print(f"非同期処理完了: {(time.time() - start_time):.3f} 秒")
    except RuntimeError as re:
        print(f"RuntimeError(非同期): {re}")

    print("\n=== 同期処理開始 ===")
    try:
        start_time = time.time()
        sync_tasks()
        print(f"同期処理完了: {(time.time() - start_time):.3f} 秒") 
    except RuntimeError as re:
        print(f"RuntimeError(同期): {re}")

```


**動作確認結果、確認ポイントの説明**
<!--
   - 実行結果の記述、正常に動作した証跡をもって確認する
   - コードや、コメントテキストはプレインテキストで記述し、
   - 必要に応じて、スクリーンキャプチャを添付してよい
-->

```

PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ> python prog-2.py
=== 非同期処理開始 ===
[async]https://example.com/にアクセス開始
[async]https://www.yahoo.co.jp/にアクセス開始
[async]https://news.yahoo.co.jpにアクセス開始
[async] https://www.yahoo.co.jp/の取得完了(0.16 秒, 57 行)
[async] https://news.yahoo.co.jpの取得完了(0.23 秒, 400 行)
[async] https://example.com/の取得完了(0.70 秒, 1 行)
[async] async_result_0.text に書き込み完了
[async] async_result_1.text に書き込み完了
[async] async_result_2.text に書き込み完了
非同期処理完了: 0.726 秒

=== 同期処理開始 ===
[sync]https://example.com/にアクセス開始
[sync]https://example.com/ の取得完了 (1.49 秒, 1 行)
[sync]https://www.yahoo.co.jp/にアクセス開始
[sync]https://www.yahoo.co.jp/ の取得完了 (1.01 秒, 57 行)
[sync]https://news.yahoo.co.jpにアクセス開始
[sync]https://news.yahoo.co.jp の取得完了 (0.89 秒, 400 行)
[sync]sync_result_0.txtに書き込み完了
[sync]sync_result_1.txtに書き込み完了
[sync]sync_result_2.txtに書き込み完了
同期処理完了: 3.406 秒
PS C:\Users\uyu06\OneDrive\Desktop\python\PROG-Ⅱ>

```

  
**分析**

<!--
   - 出題者の意図を察知し、どんな概念・技法を理解し、修得する問題か、自分の言葉で簡潔にまとめる
-->

-同期処理と非同期処理の違いを理解し、Pythonのasyncioやaiohttpを使った並行処理の書き方を習得することが目的
-非同期処理ではasync def,await,async withといった構文を使い、さらにasyncio_gather()を用いて複数のコルーチンを並行実行できる点が重要となります。
-aiohttp.ClientSessionを使った非同期HTTP通信と、非同期でのファイル書き込みを実装することで、実際に動く非同期プログラムを構築する経験が得られます。
  
**考察**  
<!--
   - 問題解決の考え方・攻略法を自分の言葉で簡潔にまとめる
   - 用いた技法、例えば、関数プログラミング、プロトコル、オブジェクト指向等の技法の効果、活用場面について、応用シーンを考察する
-->

-同期と非同期の処理モデルの違いを理解した上で、どの場面にどちらを使うべきかを判断できる力をつけるという観点でコード読み解くことだと感じました。
-非同期プログラミングの効果は
・ネットワーク通信のような待ち時間が長い処理を効率化できる
・スレッドを使わずに並行実行ができるため軽量
・負荷の高いサーバーやクローラーで高速化の恩恵が大きい
活用場面としてはWebサーバーやWebスクレイピングがあると思います。
-

**改善点**
<!--
   - この課題を通して、まだ改善できると感じた点や他の解法のアイデアを自分の言葉で論じる
-->

-通信エラーやタイムアウトを全く考慮をしていなくてネットが落ちているときやURLが間違っているときなどに例外が出て終了してしまうかもしれないので失敗しても他のURLは処理を続ける設計が重要だと感じました。
-
-

-----

### 所感・今後の目標課題
 (自由記述欄、多様なご意見を尊重します)

- プログラミングに興味が湧かない原因
- プログラミングには興味がないが、それ以外で没頭していること
- 授業中に質問や意見が言えない理由
- コードや英語や文字、活字が好きにならない理由
- 私とプログラミング
- なぜプログラミングを学ぶのだろうか？
- これまで学んだことを振り返り
- これからの学びに活かすべき知見
 

---
今までプログラミングの勉強のしかたが分からずに授業だけ聞いて曖昧に終わらせてましたがゼミでJSONを使ってなにか作成しないといけない課題があり、最初は意味が理解できずに教科書をそのまま写すことしかできませんでしたがコードの量が多くコードを打っていくことで徐々にですが理解できるようになっていったので先生が言うとおりにとにかく手を動かそうと思いました。

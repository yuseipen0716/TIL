## 同期処理　非同期処理

### 同期処理
---

タスクを実行する際、タスクAを実行→タスクA実行完了→タスクB実行→タスクB実行完了、というように　　
複数のタスクが順番に実行される方式。タスクBはタスクAの実行完了を待って、実行される。

実行の順番としては、プログラムに記載した通りの順番となる

### 非同期処理
---

あるタスクを実行している際に、そのタスクの処理完了を待たず、処理を止めることなく別のタスクを実行できる方式

よく聞くのはAJAX（Asynchronous JavaScript and XML)という技術を用いた非同期通信

それぞれのメリット、デメリットは？↓

#### 同期処理のメリット
---

タスクが順番に実行されるため、処理の全体像の把握がしやすい。

#### 同期処理のデメリット
---

例えばタスクAとBを実行しようとしている時、タスクAの処理実行完了まではタスクBの実行ができないため、ユーザーからすると処理が遅く感じる

#### 非同期処理のメリット
---

タスクを実行する際に、別タスクの実行の順番を待たないため、非同期通信を上手く活用できると、処理の速度が速くなる。

ユーザーのストレス軽減

#### 非同期処理のデメリット
---

処理の全体像は複雑化してしまうため、全体像の把握が容易でなくなる。

プログラムの記述も複雑化してしまう。

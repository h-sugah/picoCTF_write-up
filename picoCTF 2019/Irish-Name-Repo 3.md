# Irish-Name-Repo 3
Author: Xingyang Pan  

### URL
https://play.picoctf.org/practice/challenge/8  

### Description
There is a secure website running at https://jupiter.challenges.picoctf.org/problem/40742/ (link) or http://jupiter.challenges.picoctf.org:40742. Try to see if you can login as admin!  

<br>
<br>
<br>
<br>

## 解答の過程
linkをクリックして指定されたサイトにアクセスします。  

「List 'o the Irish!」のタイトルとともに、6名の人物の写真とコメントが記載されているページが表示されました。  
ページ遷移のリンクがあるのですが、動作しません。  
画面左上にあるハンバーガーメニューを開くと、以下のような3つのメニューが用意されています。  
1. Close Menu
1. Support
1. Admin Login

<br>

Close Menuはメニューを閉じるだけです。  
SupportはFAQのようなページが表示されます。  
Admin Loginを選択するとAdmin Loginページに遷移します。  

Admin Loginページではパスワードを入力する欄とLoginボタンがあります。正しいパスワードを入力すると管理者としてログインできると思われます。このように入力する箇所が用意されているということは、この問題の攻略ポイントになると考えられます。  
パスワードを適当に入力してもログイン失敗になるので、パスワードのチェックを通過させるような文字列を入力する必要があります。  
そこで、まずはSQLインジェクションの脆弱性が存在しているか確かめるために、クエリーになる以下のような文字列を入力してみたところ、エラーページが表示されました。  

> ' or 1=1; --

次に、ブラウザーの開発者モードを使用し、ログイン試行時の通信内容を確認しました。すると、POSTデータにパスワードとともにdebug=0というデータが付いているのを見つけました。  

![challenge-8--figure1.png](./pictures/challenge-8--figure1.png)  

ログインページのソースコードを見たところ、hidden属性でdebugのinput要素が記載されています。このdebugの値を操作することで何かしらの反応が得られると考えられます。  
ここで、Burp Suiteを利用し、hidden属性になっているinput要素のdebugの値を変更してアクセスすることにしました。  

### Burp Suiteの利用
Butp Suiteは、PortSwigger社が開発したWebアプリケーションのセキュリティ診断や侵入テストを実施するたツールです。Webブラウザーとサーバー間の通信を傍受し、リクエストとレスポンスの確認、編集などを行うことができます。  

操作手順は以下のようになります。  
- Burp Suiteを起動し、「Proxy」を選択します。
- 「Intercept」タグの画面内にある「Intercept 」スイッチをONに切り替え、「Open browser」ボタンでブラウザーを開きます。
- ブラウザーが起動したら、対象のサイト（https://jupiter.challenges.picoctf.org/problem/40742/）を開き、Admin Loginページまで遷移します。
- Password欄に「picoCTF」と入力し、「Login」ボタンを押すと、Burp Suiteがリクエストを補足して待機してくれます。
- Burp Suiteが補足したリクエストの内容で、Debugの値を0から1に変更し「Forward」ボタンを押します。このようにして改変したリクエストをサイトに送信します。


![challenge-8--figure2.png](./pictures/challenge-8--figure2.png)  
（改変前のリクエスト。※23行目にdebug=0と記載されています。）  

![challenge-8--figure3.png](./pictures/challenge-8--figure3.png)  
（改変後のリクエスト。※23行目でdebug=1にしています。）  

以上の操作を実施した結果、サイトはログイン失敗となりましたが、入力したパスワードの内容とSQL文の内容が表示されました。  

![challenge-8--figure4.png](./pictures/challenge-8--figure4.png)  


```
password: picoCTF
SQL query: SELECT * FROM admin where password = 'cvpbPGS'
```

<br>
password「picoCTF」の文字列が「cvpbPGS」に変化しており、これがSQLインジェクション攻撃が動作しなかった原因になっていることが分かります。  
[CyberChef](https://gchq.github.io/CyberChef/)を利用し、password文字列の変化を検証してみたところ、ROT13で暗号化していることが分かりました。  

## パスワードチェックの通過
ROT13で暗号化された結果がSQL文になるように文字列を構成すれば良いため、以下のように文字列を構成し、ログインを試行しました。  

> ' be 1=1; --

「be」の文字が暗号化されて「or」になるため、SQLインジェクションが成立してパスワードチェックを通過されるようになります。  
ログインが成功し、以下のようなページが表示されました。  
<br>
![challenge-8--figure5.png](./pictures/challenge-8--figure5.png)  

<br>
<br>
<br>
<br>

## フラグ
picoCTF{3v3n_m0r3_SQL_4424e7af}  


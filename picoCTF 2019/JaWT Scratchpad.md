# JaWT Scratchpad
Author: John Hammond  

#### URL
https://play.picoctf.org/practice/challenge/25  

#### Description
Check the admin scratchpad! https://jupiter.challenges.picoctf.org/problem/58210/ or http://jupiter.challenges.picoctf.org:58210  

<br>
<br>
<br>
<br>

## 解答の過程
問題文に記載されているURLにアクセスすると、「JaWT」というタイトルのページが表示されます。  
ウェルカムメッセージとともに、名前を登録するよう書かれています。  
どのような名前でも良いようですが、adminだけは特別なScratchpadにアクセスするようです。  
試しに「admin」を入力するとログインできないとのメッセージが表示されます。  
自分の名前を入力したくない場合は、Johnを使ってみるように書かれているので、入力欄に「John」と入力してEnterキーを押します。  

似たようなページですが、中央部分に挨拶と共に名前が記載されているページが表示されます。  
テキスト入力欄が用意されており、その下にはログアウトボタンがあります。  

![challenge-25--figure1.png](pictures/challenge-25--figure1.png)

テキスト入力欄にメッセージを適当に記述して、ログアウトボタンを押すと、最初のページに戻ります。  

改めて初期ページを見てみると、「John」がリンクになっていることが分かります。  
クリックすると、GitHubのJohnリポジトリーのページに遷移します。  
READMEには、「John the Ripper」の説明が書かれています。  

John the Ripperはパスワードクラッキングツールであり、このツールを使うことを示しているのでしょう。  

John the Ripperをどのように使っていくかを見極めるために、この問題のヒントを参照すると、Cookieとは何か？と記載されています。  
そのため、ページをアクセスしたときに得られるクッキーを確認してみます。  

ブラウザーの開発者モードを起動させます。Google Chromeであれば、ページ上を右クリックして出てくるポップアップメニューから「検証」を選択します。  
開発者モードの画面では、Applicationタブを選択します。Applicationタブ画面の左ペインから「Strage」＞「Cookies」を選択しておきます。  
初期ページで改めてJohnと入力してScratchpadページに入ると、Applicationタブ画面のクッキーリストに「jwt」が表示されます。  

![challenge-25--figure2.png](pictures/challenge-25--figure2.png)

「John」の名前でアクセスしたときのjwtは以下の値です。  

> eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiSm9obiJ9.K1Omo0Gk5saKwJTkkgT7PUZohD7USknEE0lmT2AYAiM

### JWTのデコード
「JWT」とは、JSON Web Tokenの略で「ジョット」と読みます。データをやり取りするための「署名付きトークン」であり、主にログイン認証やAPIアクセスで使われます。  

JWTはドット（.）で区切られた以下の3つの部分で構成されます。  
1. ヘッダー
2. ペイロード
3. 署名

JWTの値がどのようになっているか確認するため、以下のサイトを利用しました。  

[JWT encoder / decoder](https://logto.io/ja/jwt-decoder)

このサイトは、JWTをエンコード／デコードする機能があります。  
まず、取得したJWTをデコードして、各構成がどのような内容になっているか確認してみます。  
タイトルのようになっている「decoder」をクリックしてJWT decoderのページにします。  
JWT欄にJWTをペーストして、デコードされたヘッダーとペイロードを確認します。  
ヘッダーとペイロードは、以下のようにデコードされました。

```
【デコードされたヘッダー】
{
    "typ": "JWT",
    "alg": "HS256"
}

【デコードされたペイロード】
{
    "user": "John"
}

【署名】
（現時点では不明です）
```

3つ目の署名は、秘密鍵で暗号化された値であり、これを解き明かすことが必要です。  
そのツールとしてJohn the Ripperを使用します。  

### John the Ripperでの作業
John the Ripperは、Kali Linuxを導入していればデフォルトでインストールされているツールのはずです。  
ターミナルで「john」と入力し、インストールされていれば以下のようにバージョン情報とUsageメッセージが表示されます。  
コマンドがないメッセージが出力された場合は、aptコマンドを使ってインストールします。  

![challenge-25--figure3.png](pictures/challenge-25--figure3.png)  

John the RipperによるJWTの署名の秘密鍵クラックは、最近のバージョンではJWTを与えるだけで処理してくれます。  
まず、テキストエディターでJWTの値を記述し、テキストファイルを用意します。  
ここでは、ファイル名を「John.jwt」としました。  
そして、以下のようにJohnコマンドの後にファイル名を指定します。  

```
$ john john.jwt
```

実行した結果は以下のようになります。  

![challenge-25--figure4.png](pictures/challenge-25--figure4.png)

オレンジ色で表示されているのが秘密鍵です。「ilovepico」  ・・・自力で推測できたかもです。  
この秘密鍵が出力されるまで私のマシンでは5時間ほどかかりました。  
早いマシンの使用や、予め辞書データなどを用意して実施すると良いかもしれません。  

### JWTの作成
さて、秘密鍵が分かったので、先ほどのサイト、  

[JWT encoder / decoder](https://logto.io/ja/jwt-decoder)  

でJWTをエンコードします。  

JWTをデコードしたときの画面で、署名キーの欄に「ilovepico」と入力すると、署名が検証されたとメッセージが表示されます。問題なさそうです。  
次に、タイトルのようになっている「encoder」の部分をクリックし、エンコードのページに切り替えます。  
ペイロードの「John」を「admin」に書き換えます。署名キーはそのまま「ilovapico」です。  
すると、署名も含めてadmin用のエンコードされたJWTが出力されます。これをコピーしておきます。  

> ペイロードを「admin」、署名キーを「ilovepico」でエンコードしたJWT
> eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiYWRtaW4ifQ.gtqDl4jVDvNbEe_JYEZTN19Vx6X9NNZtRVbKPBkhO-s

### JaWTサイトでの作業
JaWTサイトのScrachpadページに移ります。  
開発者モードのApplicationタブ画面にて、jwtの値を先ほどのサイトでエンコードしたadmin用のJWTに書き換えます。  

![challenge-25--figure5.png](pictures/challenge-25--figure5.png)

そして、ブラウザーを再読み込みすると、Scrachpadのテキスト入力欄にフラグが出力されます。  
これはクッキー（JWTのトークン）によってadminとしてログインした状態になり、admin用のScrachpadのページに切り替わったことになります。  

<br>
<br>
<br>
<br>

## フラグ
picoCTF{jawt_was_just_what_you_thought_44c752f5}  


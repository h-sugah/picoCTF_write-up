# m00nwalk2
Author: Joon  

#### URL
https://play.picoctf.org/practice/challenge/28  

#### Description
Revisit the last transmission. We think this transmission contains a hidden message. There are also some clues clue 1, clue 2, clue 3.  

<br>
<br>
<br>
<br>

## 解答の過程
「transmission」、「clue 1」、「clue 2」、「clue 3」のリンクをクリックすることでファイルをダウンロードすることができます。  
それぞれファイル名は「message.wav」、「clue1.wav」、「clue2.wav」、「clue3.wav」です。  
wave形式の音声ファイルであり、問題のタイトルから[m00nwalk](https://play.picoctf.org/practice/challenge/26)と同じSSTVの手法を使っていることが分かります。  

そこで、これら4つのファイルを[m00nwalk](https://play.picoctf.org/practice/challenge/26)でも利用した以下のサイトでデコードします。  

[Online SSTV Decoder](https://sstv-decoder.mathieurenaud.fr/)

デコードされた画像はいかのようになりました。  

【message】  
![challenge-28--figure1.png](pictures/challenge-28--figure1.png)

【clue1】  
![challenge-28--figure2.png](pictures/challenge-28--figure2.png)

【clue2】  
![challenge-28--figure3.png](pictures/challenge-28--figure3.png)

【clue3】  
![challenge-28--figure4.png](pictures/challenge-28--figure4.png)

<br>

messageの画像に記載されているフラグは、[m00nwalk](https://play.picoctf.org/practice/challenge/26)のフラグです。今回の問題のフラグではありませんでした。  
そのため、3つのヒント画像で考えていく必要があります。  

clue1は、パスワードが記載されています。「hidden_stegosaurus」このパスワードを使うのでしょう。  
clue2は、「The quieter you are the more you can HEAR」と書かれています。この問題を解くための心構えでも示しているのでしょうか。  
clue3は、「Alan Eliasen the FutureBoy」と書かれています。誰なのか調べるためにGoogle検索を使います。すると、以下のサイトがありました。  

[Alan Eliasen -- Things I Made From Yarn](https://futureboy.us/)

Alan Eliasen氏は、主にプログラミング言語や計算ツールの開発者として知られており、Frinkというプログラミング言語を設計・実装しています。  
上記のサイトは、彼が開発した様々なツールが公開されています。  
これらのツールの中に手掛かりになるものがあると考えられるため、ページ上を「picture」をキーワードに検索をかけるとSteganographyというツールが見つかります。  
Steganographyは、Hide secret messages or files in pictures.と説明されており、今回の問題を解くツールになりそうです。  

当該ツールのページへ移ると、以下の3つの機能が用意されています。  
- Encode an image.
- Decode an image.
- Compare two images.

今回は、デコードしてメッセージを取り出したいため、2番目の「Decode an image」を選択します。  

「ファイルを選択」ボタンを押して、「message.wav」をアップロードします。（SSTVでデコードした図のファイルではなく、ダウンロードしたwavファイルです。）  
「password」欄には「hidden_stegosaurus」を入力します。  
「View raw output as MIME-type」を選択し、「text/plain」と記述しておきます。  
これで「送信」ボタンを押して、しばらく待つとフラグが表示されます。  

<br>
<br>
<br>
<br>

## フラグ
picoCTF{the_answer_lies_hidden_in_plain_sight}  


# m00nwalk
Author: Joon  

#### URL
https://play.picoctf.org/practice/challenge/26  

#### Description
Decode this message from the moon.  

<br>
<br>
<br>
<br>

## 解答の過程
問題文の「message」のリンクをクリックすると、ファイルをダウンロードすることができます。  
ファイル名は「message.wav」です。  
音声形式のファイルなので、再生してみます。  

1分55秒に渡り、ピーヒャラピーヒャラと機械的な音声が流れます。  
この音声を何かしら処理してフラグを取得するのでしょう。  

ヒントは2つ、  
- How did pictures from the moon landing get sent back to Earth? （月面着陸の写真は、どのようにして地球に送られたか？）
- What is the CMU mascot?, that might help select a RX option （CMUのマスコットは何か？RXオプションを選ぶと良いかもしれない。）

月面着陸の写真に関するヒントは、ダウンロードしたファイルを処理するツールへと導くものになりそうです。  
CMUのマスコットとは、カーネギーメロン大学のマスコットでScottyのことを言っているのでしょう。ツールを使うためのヒントでしょうか。  

### 調査
月面着陸したのはアポロ11号なので、Wikipediaでアポロ11の記事を参照します。  

[Wikipedia: Apollo 11](https://en.wikipedia.org/wiki/Apollo_11)

「picture」をキーワードにして検索すると、以下の文章が見つかります。  

> Apollo 11 used slow-scan television (TV) incompatible with broadcast TV, so it was displayed on a special monitor and a conventional TV camera viewed this monitor (thus, a broadcast of a broadcast), significantly reducing the quality of the picture.

この記述にある「slow-scan television」（SSTV）は、無線通信で画像を送る手法のようです。  

[Wikipedia: Slow-scan television](https://en.wikipedia.org/wiki/Slow-scan_television)

音声形式のファイルをSSTVで扱えるツールはあるのでしょうか。探してみます。  

### SSTVのツール
Google検索で「sstv wav decode」のキーワードで検索すると、オンラインで変換してくれるサイトが見つかります。  
今回は以下のサイトを使います。  

[Online SSTV Decoder](https://sstv-decoder.mathieurenaud.fr/)

ダウンロードした「message.wav」をアップロードして、「decode SSTV」ボタンを押すだけです。  
少し待つことで、ページ上にデコードされた画像が表示されます。また、画像はダウンロードすることもできます。  
表示された画像にはフラグが書かれているので、それをテキストにします。  

<br>
<br>
<br>
<br>

## フラグ
picoCTF{beep_boop_im_in_space}  

## この問題は
・・・CMUのマスコットについては分からないまま問題を解いてしまいました。  

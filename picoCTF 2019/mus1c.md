# mus1c
Author: Danny  

### URL
https://play.picoctf.org/practice/challenge/15  

### Description
I wrote you a song. Put it in the picoCTF{} flag format.  

<br>
<br>
<br>
<br>

## 解答の過程
songをクリックするとファイルをダウンロードすることができます。  
ファイル名は「lyrics.txt」です。  
テキスト形式のファイルなので、エディターで開いて中身を確認します。  

Picoと叫びつつ、セキュリティの重要性を訴える歌詞になっています。  
まったく意味が分かりません。  
ヒントを見ると、「Do you think you can master rockstar?」と書かれています。  
rockstarをマスターできると思うか？ということで、rackstarとは何かを調べてみます。  

プログラミング言語として存在していることが分かりました。  
[https://codewithrockstar.com/](https://codewithrockstar.com/)  

サイトを確認すると、オンラインで実行する環境が用意されています。  
[https://codewithrockstar.com/online](https://codewithrockstar.com/online)  

上記サイトに、ダウンロードしたrockstarのコードをコピー＆ペーストして「Rock」ボタンを押したところ、以下のような出力が得られました。  

```
114 114 114 111 99 107 110 114 110 48 49 49 51 114
```

この数値をASCIIコードとして文字に変換すればフラグになります。  

<br>
<br>
<br>
<br>

#フラグ
picoCTF{rrrocknrn0113r}  


# Tapping
Author: Danny  

#### URL
https://play.picoctf.org/practice/challenge/21  

#### Description
Theres tapping coming in from the wires. What's it saying `nc jupiter.challenges.picoctf.org 48247`.  

<br>
<br>
<br>
<br>

## 解答の過程
「tapping」とは、叩く、という意味です。  
トントンと叩いているので何と言っているのか調べましょうという問題になっています。  
ncコマンドによるアクセス先が記載されているので、Webshell、またはLinuxのターミナルでコマンドを実行します。  

```
$ nc jupiter.challenges.picoctf.org 48247
.--. .. -.-. --- -.-. - ..-. { -- ----- .-. ... ...-- -.-. ----- -.. ...-- .---- ... ..-. ..- -. .---- ..--- -.... .---- ....- ...-- ---.. .---- ---.. .---- }
```

モールス信号で間違いないでしょう。  
「.（ドット）」と「-（ダッシュ）」の組み合わせでスペースを区切りにして、どのような文字を表しているのか調べます。  

モールス信号のコードを文字に変換するサイトは、検索すると数多く存在しています。  
今回は、以下のサイトを使わせていただきました。  

[Morse Code World!](https://morsecode.world/international/translator.html)  

「Input:」の枠内に得られたモールス信号の文字列を入力することで、Output:の枠内に変換された文字列が出力されます。「Clean up」ボタンを押すことで「{」と「}」の記号など余計な文字を削除してくれます。  
処理してみた結果、以下の出力が得られました。  

```
PICOCTFM0RS3C0D31SFUN1261438181
```

「{」と「}」を適切な箇所に挿入し、フラグになります。  
ヒントにも書かれているように、フラグは大文字で記載することを求められていることが分かります。  
なので、全部大文字で構成します。  

<br>
<br>
<br>
<br>

## フラグ
PICOCTF{M0RS3C0D31SFUN1261438181}  


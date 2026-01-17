# where are the robots
Author: zaratec/Danny  

### URL
https://play.picoctf.org/practice/challenge/4  

### Description
Can you find the robots? https://jupiter.challenges.picoctf.org/problem/60915/ (link) or http://jupiter.challenges.picoctf.org:60915  

<br>
<br>

## 解答の過程
問題文に記載されているリンクからページを開くと、シンプルなWelcomeページが表示されます。  
そのページには、「Where are the robots?」と記述されています。  
robotsを見つければ、フラグが得られると推測できます。  

robotsは、ブラウザーのURL欄に、robots.txtを入力することで参照できます。  

ここでは、  
https://jupiter.challenges.picoctf.org/problem/60915/  
にアクセスしたので、  
https://jupiter.challenges.picoctf.org/problem/60915/robots.txt  
と記述することでrobots.txtの内容を確認できます。  

当該サイトのrobots.txtには、以下のように記述されていました。  
```
User-agent: *
Disallow: /8028f.html
```
8028f.htmlがDisallowと記述されているので、ロボットに8028f.htmlのページを参照しないようにしていることが分かります。  

そこで、8028f.htmlのページにアクセスしてみます。URL欄に次のように入力します。  
> https://jupiter.challenges.picoctf.org/problem/60915/8028f.html


アクセスしたページに、フラグが記載されていました。

<br>
<br>
<br>
<br>

## フラグ
picoCTF{ca1cu1at1ng_Mach1n3s_8028f}  

<br>
<br>

## 参考
[robots.txt の概要](https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=ja)  

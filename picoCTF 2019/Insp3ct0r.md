# Insp3ct0r
Author: zaratec/danny  

#### URL
https://play.picoctf.org/practice/challenge/18  

#### Description
Kishor Balan tipped us off that the following code may need inspection: https://jupiter.challenges.picoctf.org/problem/9670/ or http://jupiter.challenges.picoctf.org:9670  

<br>
<br>
<br>
<br>

## 解答の過程
問題のヒントには、「ブラウザーでウェブコードを検査するにはどうすればよいか？」と「3パートある」ことが書かれています。  
問題に記載されているURLへアクセスすると、「Inspect Me」とタイトルの付いたページが表示されます。  

「What」と「How」がボタンになっており、「What」では「I made a website」のメッセージが、「How」では「I used these to make this site:  HTML CSS JS (JavaScript)」のメッセージが記載されています。  

このページがHTML、CSS、JSで構成されており、「ウェブコードを検査」するとあるので、ブラウザーの開発者ツールを使います。  

下図はChromeの画面を示しています。  
開発者ツールを起動し「Sources」タブを選択すると、左ペインのファイルツリーの中にHTML、CSS、JSファイルが存在しています。  
![challenge-18-figure1](pictures/challenge-18-figure1.png)  

それぞれのソースコード下部にコメントがあり、フラグの断片が記述されています。  
繋げることでフラグになります。  

<br>
<br>
<br>
<br>

## フラグ
picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?2e7b23e3}  


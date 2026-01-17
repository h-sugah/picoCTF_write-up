# Based
Author: Alex Fulton/Daniel Tunitis  

#### URL
https://play.picoctf.org/practice/challenge/35  

#### Description
To get truly 1337, you must understand different data encodings, such as hexadecimal or binary. Can you get the flag from this program to prove you are on the way to becoming 1337?  

Additional details will be available after launching your challenge instance.  

<br>
<br>
<br>
<br>

## 解答の過程
「Launch Instance」ボタンを押すことで、追加の説明が表示されます。  

```
To get truly 1337, you must understand different data encodings, such as hexadecimal or binary. Can you get the flag from this program to prove you are on the way to becoming 1337?Connect with nc fickle-tempest.picoctf.net 60625.
```

ncコマンドでの接続先情報が表示されました。  
接続してみます。  

```
$ nc fickle-tempest.picoctf.net 60625
Let us see how data is stored
test
Please give the 01110100 01100101 01110011 01110100 as a word.
...
you have 45 seconds.....

Input:

```

45秒以内に表示されたバイナリーで表記されたデータを文字にして入力する必要があります。  
制限時間が経過すると、「Too slow!」と言われて終了します。  
終了してしまった場合は、接続しなおして再挑戦することになります。そのとき、別のデータになるので注意してください。  

バイナリー表記のデータを文字にして答えると、次はオクタルで表記されたデータでの質問になります。  

```
Please give me the  o143 o157 o155 o160 o165 o164 o145 o162 as a word.
Input:

```

同様に、制限時間が経過すると、「Too slow!」と言われて終了します。  
終了してしまった場合は、接続しなおして再挑戦することになります。しかし、バイナリー表記のデータからやり直しになるので注意してください。  

オクタル表記のデータに答えると、次はヘキサデシマルで表記されたデータでの質問になります。  

```
Please give me the 636f6d7075746572 as a word.
Input:

```

同様に、制限時間が経過すると、「Too slow!」と言われて終了します。
終了してしまった場合は、接続しなおして再挑戦することになります。しかし、バイナリー表記のデータからやり直しになるので注意してください。  

ヘキサデシマル表記のデータに答えると、フラグが表示されます。  

```
You've beaten the challenge
Flag: picoCTF{learning_about_converting_values_aeBEA593}
```

<br>
<br>
<br>
<br>

## フラグ
picoCTF{learning_about_converting_values_aeBEA593}  

<br>
<br>

## 補足
[CyberChef](https://gchq.github.io/CyberChef/)などを活用して各表記のデータを文字に変換すると良いでしょう。  

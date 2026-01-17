# what's a net cat?
Author: Sanjay C/Danny Tunitis  

#### URL
https://play.picoctf.org/practice/challenge/34  

#### Description
Additional details will be available after launching your challenge instance.  

<br>
<br>
<br>
<br>

## 解答の過程
「Launch Instance」ボタンを押すことによって追加の説明が表示されます。  

```
Using netcat (nc) is going to be pretty important. Can you connect to fickle-tempest.picoctf.net at port 53072 to get the flag?
```

netcatコマンド（ncコマンド）の重要性を伝えるメッセージで、fickle-tempest.picoctf.netにポート53072で接続するように記載されています。  

記載されている通り、ncコマンドで接続します。  
Linuxでコンソールを起動し、以下のように、ncに続けて接続先のFQDNとポート番号を記述します。  

```
$ nc fickle-tempest.picoctf.net 53072
You're on your way to becoming the net cat master
picoCTF{nEtCat_Mast3ry_aC66D475}
```

接続するとメッセージとフラグが表示されます。  
netcatコマンドのマスターになれました。  

<br>
<br>
<br>
<br>

#＃ フラグ
picoCTF{nEtCat_Mast3ry_aC66D475}  


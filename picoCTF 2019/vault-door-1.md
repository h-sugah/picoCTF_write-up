# vault-door-1
Author: Mark E. Haase  

### URL
https://play.picoctf.org/practice/challenge/12  

### Description
This vault uses some complicated arrays! I hope you can make sense of it, special agent. The source code for this vault is here: [VaultDoor1.java](https://jupiter.challenges.picoctf.org/static/87e103a8db01087de9ccf5a7a022ddf8/VaultDoor1.java)  

<br>
<br>

## 解答の過程
VaultDoor1.javaをクリックすると、同名のファイルをダウンロードすることができます。  
エディターで開くと、Javaのソースコードであることが分かります。  
問題文にも書いてある通り、配列が大量に記述されています。  

ソースコードを読むと、動作は次のようになっています。  
- main()でフラグになるパスワードを入力させ、inputにセットしています。
- inputを引数にしてcheckPassword()を呼び出します。
- checkPassword()では、受け取ったパスワードの文字数と、charAt()メソッドのインデックスの文字が指定された文字と一致しているかをチェックしています。
- &&の論理演算で接続されているので、全部の条件を満たすことでアクセスを獲得できます。
これにより、checkPassword()の条件を判断しているcharAt()メソッドのインデックスを順番に並べることでパスワードを知ることができ、それがフラグになることが分かります。
checkPassword()で文字を判定している部分で、インデックスを昇順に並べ替えると、以下のようになります。  
```
password.charAt(0)  == 'd' &&
password.charAt(1)  == '3' &&
password.charAt(2)  == '5' &&
password.charAt(3)  == 'c' &&
password.charAt(4)  == 'r' &&
password.charAt(5)  == '4' &&
password.charAt(6)  == 'm' &&
password.charAt(7)  == 'b' &&
password.charAt(8)  == 'l' &&
password.charAt(9)  == '3' &&
password.charAt(10) == '_' &&
password.charAt(11) == 't' &&
password.charAt(12) == 'H' &&
password.charAt(13) == '3' &&
password.charAt(14) == '_' &&
password.charAt(15) == 'c' &&
password.charAt(16) == 'H' &&
password.charAt(17) == '4' &&
password.charAt(18) == 'r' &&
password.charAt(19) == '4' &&
password.charAt(20) == 'c' &&
password.charAt(21) == 'T' &&
password.charAt(22) == '3' &&
password.charAt(23) == 'r' &&
password.charAt(24) == '5' &&
password.charAt(25) == '_' &&
password.charAt(26) == 'f' &&
password.charAt(27) == '6' &&
password.charAt(28) == 'd' &&
password.charAt(29) == 'a' &&
password.charAt(30) == 'f' &&
password.charAt(31) == '4';
```

各文字を繋げて、所定のフォーマットにすることでフラグになります。  

<br>
<br>
<br>
<br>

## Flag
picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_f6daf4}  


### Easy1
Author: Alex Fulton/Danny  

#### URL
https://play.picoctf.org/practice/challenge/43  

#### Description
The one time pad can be cryptographically secure, but not when you know the key. Can you solve this?  
We've given you the encrypted flag, key, and a table to help UFJKXQZQUNB with the key of SOLVECRYPTO. Can you use this table to solve it?.  

<br>
<br>
<br>
<br>

## 解答の過程
tableをクリックするとファイルをダウンロードできます。  
ファイル名は「table.txt」です。  
エディターでファイルを開いて見てみると、以下のテーブルが記載されています。ヴィジュネル表です。  

```
    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 
   +----------------------------------------------------
A | A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
B | B C D E F G H I J K L M N O P Q R S T U V W X Y Z A
C | C D E F G H I J K L M N O P Q R S T U V W X Y Z A B
D | D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
E | E F G H I J K L M N O P Q R S T U V W X Y Z A B C D
F | F G H I J K L M N O P Q R S T U V W X Y Z A B C D E
G | G H I J K L M N O P Q R S T U V W X Y Z A B C D E F
H | H I J K L M N O P Q R S T U V W X Y Z A B C D E F G
I | I J K L M N O P Q R S T U V W X Y Z A B C D E F G H
J | J K L M N O P Q R S T U V W X Y Z A B C D E F G H I
K | K L M N O P Q R S T U V W X Y Z A B C D E F G H I J
L | L M N O P Q R S T U V W X Y Z A B C D E F G H I J K
M | M N O P Q R S T U V W X Y Z A B C D E F G H I J K L
N | N O P Q R S T U V W X Y Z A B C D E F G H I J K L M
O | O P Q R S T U V W X Y Z A B C D E F G H I J K L M N
P | P Q R S T U V W X Y Z A B C D E F G H I J K L M N O
Q | Q R S T U V W X Y Z A B C D E F G H I J K L M N O P
R | R S T U V W X Y Z A B C D E F G H I J K L M N O P Q
S | S T U V W X Y Z A B C D E F G H I J K L M N O P Q R
T | T U V W X Y Z A B C D E F G H I J K L M N O P Q R S
U | U V W X Y Z A B C D E F G H I J K L M N O P Q R S T
V | V W X Y Z A B C D E F G H I J K L M N O P Q R S T U
W | W X Y Z A B C D E F G H I J K L M N O P Q R S T U V
X | X Y Z A B C D E F G H I J K L M N O P Q R S T U V W
Y | Y Z A B C D E F G H I J K L M N O P Q R S T U V W X
Z | Z A B C D E F G H I J K L M N O P Q R S T U V W X Y
```

問題文には、SOLVECRYPTOをキーにして、UFJKXQZQUNBを復号するように記載されています。上記テーブルを参照しながら解読していきます。  
テーブルは、横（列）の方向が平文で、縦（行）の方向がキーになります。横と縦の交点が暗号文として扱われる文字です。  
ここでは、キーと暗号文が分かっているので、以下のように対応させていきます。  

| キー        | 暗号文           | 平文                    |
| --------- | ------------- | --------------------- |
| 縦（行）の列で文字を見る | 文字がある箇所まで右へ進む | 文字の箇所から最上部の横（列）の文字を見る |
| S         | U             | C                     |
| O         | F             | R                     |
| L         | J             | Y                     |
| V         | K             | P                     |
| E         | X             | T                     |
| C         | Q             | O                     |
| R         | Z             | I                     |
| Y         | Q             | S                     |
| P         | U             | F                     |
| T         | N             | U                     |
| O         | B             | N                     |

CRYPTOISFUNになります。  
「暗号は楽しい」ですね！  

<br>
<br>
<br>
<br>

## フラグ
picoCTF{CRYPTOISFUN}  

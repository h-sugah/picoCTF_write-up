### vault-door-8
Author: Mark E. Haase  

### URL
https://play.picoctf.org/practice/challenge/10  

### Description
Apparently Dr. Evil's minions knew that our agency was making copies of their source code, because they intentionally sabotaged this source code in order to make it harder for our agents to analyze and crack into! The result is a quite mess, but I trust that my best special agent will find a way to solve it. The source code for this vault is here: VaultDoor8.java  

<br>
<br>

## 解答の過程
ちょっとしたスパイ風のシナリオになっており、VaultDoor8.javaをクリックするとJavaのソースコードをダウンロードすることができます。  
エディターでファイルを開くと、以下のように読みにくい状態になっていました。  

```
// These pesky special agents keep reverse engineering our source code and then
// breaking into our secret vaults. THIS will teach those sneaky sneaks a
// lesson.
//
// -Minion #0891
import java.util.*; import javax.crypto.Cipher; import javax.crypto.spec.SecretKeySpec;
import java.security.*; class VaultDoor8 {public static void main(String args[]) {
Scanner b = new Scanner(System.in); System.out.print("Enter vault password: ");
String c = b.next(); String f = c.substring(8,c.length()-1); VaultDoor8 a = new VaultDoor8(); if (a.checkPassword(f)) {System.out.println("Access granted."); }
else {System.out.println("Access denied!"); } } public char[] scramble(String password) {/* Scramble a password by transposing pairs of bits. */
char[] a = password.toCharArray(); for (int b=0; b<a.length; b++) {char c = a[b]; c = switchBits(c,1,2); c = switchBits(c,0,3); /* c = switchBits(c,14,3); c = switchBits(c, 2, 0); */ c = switchBits(c,5,6); c = switchBits(c,4,7);
c = switchBits(c,0,1); /* d = switchBits(d, 4, 5); e = switchBits(e, 5, 6); */ c = switchBits(c,3,4); c = switchBits(c,2,5); c = switchBits(c,6,7); a[b] = c; } return a;
} public char switchBits(char c, int p1, int p2) {/* Move the bit in position p1 to position p2, and move the bit
that was in position p2 to position p1. Precondition: p1 < p2 */ char mask1 = (char)(1 << p1);
char mask2 = (char)(1 << p2); /* char mask3 = (char)(1<<p1<<p2); mask1++; mask1--; */ char bit1 = (char)(c & mask1); char bit2 = (char)(c & mask2); /* System.out.println("bit1 " + Integer.toBinaryString(bit1));
System.out.println("bit2 " + Integer.toBinaryString(bit2)); */ char rest = (char)(c & ~(mask1 | mask2)); char shift = (char)(p2 - p1); char result = (char)((bit1<<shift) | (bit2>>shift) | rest); return result;
} public boolean checkPassword(String password) {char[] scrambled = scramble(password); char[] expected = {
0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xE0, 0x95, 0xF1, 0xE1, 0xE0, 0xA4, 0xC0, 0x94, 0xA4 }; return Arrays.equals(scrambled, expected); } }
```

このソースコードを構造を意識しながら整形すると、以下のような内容になります。  

```
// These pesky special agents keep reverse engineering our source code and then
// breaking into our secret vaults. THIS will teach those sneaky sneaks a
// lesson.
//
// -Minion #0891

import java.util.*;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.security.*;

class VaultDoor8
{
    public static void main(String args[])
    {
        Scanner b = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String c = b.next();
        String f = c.substring(8,c.length()-1);
        VaultDoor8 a = new VaultDoor8();
        if (a.checkPassword(f))
        {
            System.out.println("Access granted.");
        }
        else
        {
            System.out.println("Access denied!");
        }
    }

    public char[] scramble(String password)
    {
        /* Scramble a password by transposing pairs of bits. */
        char[] a = password.toCharArray();

        for (int b=0; b<a.length; b++)
        {
            char c = a[b];
            c = switchBits(c,1,2);
            c = switchBits(c,0,3);
            /* c = switchBits(c,14,3); c = switchBits(c, 2, 0); */
            c = switchBits(c,5,6);
            c = switchBits(c,4,7);
            c = switchBits(c,0,1);
            /* d = switchBits(d, 4, 5); e = switchBits(e, 5, 6); */
            c = switchBits(c,3,4);
            c = switchBits(c,2,5);
            c = switchBits(c,6,7);
            a[b] = c;
        }

        return a;
    }

    public char switchBits(char c, int p1, int p2)
    {
        /* Move the bit in position p1 to position p2, and move the bit
        that was in position p2 to position p1. Precondition: p1 < p2 */
        char mask1 = (char)(1 << p1);
        char mask2 = (char)(1 << p2);

        /* char mask3 = (char)(1<<p1<<p2); mask1++; mask1--; */
        char bit1 = (char)(c & mask1);
        char bit2 = (char)(c & mask2);

        /* System.out.println("bit1 " + Integer.toBinaryString(bit1));
        System.out.println("bit2 " + Integer.toBinaryString(bit2)); */
        char rest = (char)(c & ~(mask1 | mask2));
        char shift = (char)(p2 - p1);
        char result = (char)((bit1<<shift) | (bit2>>shift) | rest);

        return result;
    }

    public boolean checkPassword(String password)
    {
        char[] scrambled = scramble(password);
        char[] expected = {0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xE0, 0x95, 0xF1, 0xE1, 0xE0, 0xA4, 0xC0, 0x94, 0xA4 };

        return Arrays.equals(scrambled, expected);
    }
}
```

ChatGPTなどの生成AIを使えば、どのような結果になるかすぐ分かるのですが、正攻法はコードを解析してフラグを見出すことになります。  

#### main関数
「Enter vault password:」を表示して、パスワードを入力するようになっています。  
パスワードを入力すると、入力したパスワードを引数にしてcheckPassword関数を呼び出します。  
関数の結果がtrueであれば「Access granted.」を表示し、falseであれば「Access denied!」を表示します。  

#### checkPassword関数
パスワードを文字列として受け取り、scramble関数に渡してscrambledの配列に設定しています。  
expectedという配列では、以下の16進数コードを設定しています。  

```
0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xE0, 0x95, 0xF1, 0xE1, 0xE0, 0xA4, 0xC0, 0x94, 0xA4
```

scrambledとexpectedが一致するかどうかを比較して、main関数に戻っています。一致すればture、一致しなければfalseです。  
これにより、expectedの16進数コードになるように、scrambledの16進数コードを設定すれば良く、それがフラグになると判断できます。  

#### scramble関数
パスワードを文字列として受け取り、配列にして1文字ずつ処理しています。  
1文字に対して引数を少しずつ変えながら、8回switchbit関数を呼び出しています。  

#### switchbit関数
与えられた文字と、2つの数値に従い、以下の処理を実施しています。  

```
char mask1 = (char)(1 << p1);
char mask2 = (char)(1 << p2);
```
- 1ビット目を左にずらし、2つのマスク（mask1とmask2）を用意しています。

```
char bit1 = (char)(c & mask1);
char bit2 = (char)(c & mask2);
```
- 文字と2つのマスクでAND演算して、マスクした箇所のビット（bit1とbit2）を用意しています。

```
char rest = (char)(c & ~(mask1 | mask2));
```
- mask1とmask2をOR演算してビットを反転させ、文字とAND演算しています。つまり、マスクしたビット位置だけを0にしています。

```
char shift = (char)(p2 - p1);
char result = (char)((bit1<<shift) | (bit2>>shift) | rest);
```
- p2とp1の差をshiftとして求めて、bit1をshift分だけ左にずらし、bit2をshift分だけ右にずらし、restとともにそれぞれOR演算しています。それが結果となる文字です。

これで、どのような処理をしているか分かりました。  

#### ではどうするか？
パスワードをscrambleしてexpectedと比較しているので、expectedを逆にscrambleしてやれば、パスワードが分かります。  
つまり、scramble関数で繰り返し呼び出しているswitchbits関数の実行順序を逆にして実行します。  

Pythonでその処理を記述すると、以下のようなコードになります。  

```
def switch_bits(c, p1, p2):
    """Bit-swap implementation identical to the Java code."""
    mask1 = 1 << p1
    mask2 = 1 << p2
    bit1 = c & mask1
    bit2 = c & mask2
    rest = c & ~(mask1 | mask2)
    shift = p2 - p1
    return (bit1 << shift) | (bit2 >> shift) | rest

def unscramble(char_list):
    result = []
    for c in char_list:
        x = c
        x = switch_bits(x, 6, 7)
        x = switch_bits(x, 2, 5)
        x = switch_bits(x, 3, 4)
        x = switch_bits(x, 0, 1)
        x = switch_bits(x, 4, 7)
        x = switch_bits(x, 5, 6)
        x = switch_bits(x, 0, 3)
        x = switch_bits(x, 1, 2)
        result.append(x)
    return result

# expected配列
expected = [
    0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4,
    0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86,
    0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xE0,
    0x95, 0xF1, 0xE1, 0xE0, 0xA4, 0xC0, 0x94, 0xA4
]

unscrambled = unscramble(expected)
password = ''.join(chr(c) for c in unscrambled)
print("password:", password)
```

このコードを「unscramble.py」ファイルとして保存し、実行した結果が以下になります。  

```
> python unscramble.py
password: s0m3_m0r3_b1t_sh1fTiNg_2e762b0ab
```

picoCTF{ }の{ }内に得られたパスワードを入れてフラグになります。

<br>
<br>
<br>
<br>

## フラグ
> picoCTF{s0m3_m0r3_b1t_sh1fTiNg_2e762b0ab}


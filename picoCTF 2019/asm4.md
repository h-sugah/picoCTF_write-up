# asm4
Author: Sanjay C  

### URL
https://play.picoctf.org/practice/challenge/5  

### Description
What will asm4("picoCTF_724a2") return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format. Source  

<br>
<br>
<br>
<br>

## 解答の過程

"source"のリンクをクリックするとソースコードをダウンロードできます。ファイル名は「test.S」になっていました。  
エディターでtest.Sを開くと、以下のようなアセンブリー言語のソースコードが記載されていました。  

```
asm4:
	<+0>:	push   ebp
	<+1>:	mov    ebp,esp
	<+3>:	push   ebx
	<+4>:	sub    esp,0x10
	<+7>:	mov    DWORD PTR [ebp-0x10],0x252
	<+14>:	mov    DWORD PTR [ebp-0xc],0x0
	<+21>:	jmp    0x518 <asm4+27>
	<+23>:	add    DWORD PTR [ebp-0xc],0x1
	<+27>:	mov    edx,DWORD PTR [ebp-0xc]
	<+30>:	mov    eax,DWORD PTR [ebp+0x8]
	<+33>:	add    eax,edx
	<+35>:	movzx  eax,BYTE PTR [eax]
	<+38>:	test   al,al
	<+40>:	jne    0x514 <asm4+23>
	<+42>:	mov    DWORD PTR [ebp-0x8],0x1
	<+49>:	jmp    0x587 <asm4+138>
	<+51>:	mov    edx,DWORD PTR [ebp-0x8]
	<+54>:	mov    eax,DWORD PTR [ebp+0x8]
	<+57>:	add    eax,edx
	<+59>:	movzx  eax,BYTE PTR [eax]
	<+62>:	movsx  edx,al
	<+65>:	mov    eax,DWORD PTR [ebp-0x8]
	<+68>:	lea    ecx,[eax-0x1]
	<+71>:	mov    eax,DWORD PTR [ebp+0x8]
	<+74>:	add    eax,ecx
	<+76>:	movzx  eax,BYTE PTR [eax]
	<+79>:	movsx  eax,al
	<+82>:	sub    edx,eax
	<+84>:	mov    eax,edx
	<+86>:	mov    edx,eax
	<+88>:	mov    eax,DWORD PTR [ebp-0x10]
	<+91>:	lea    ebx,[edx+eax*1]
	<+94>:	mov    eax,DWORD PTR [ebp-0x8]
	<+97>:	lea    edx,[eax+0x1]
	<+100>:	mov    eax,DWORD PTR [ebp+0x8]
	<+103>:	add    eax,edx
	<+105>:	movzx  eax,BYTE PTR [eax]
	<+108>:	movsx  edx,al
	<+111>:	mov    ecx,DWORD PTR [ebp-0x8]
	<+114>:	mov    eax,DWORD PTR [ebp+0x8]
	<+117>:	add    eax,ecx
	<+119>:	movzx  eax,BYTE PTR [eax]
	<+122>:	movsx  eax,al
	<+125>:	sub    edx,eax
	<+127>:	mov    eax,edx
	<+129>:	add    eax,ebx
	<+131>:	mov    DWORD PTR [ebp-0x10],eax
	<+134>:	add    DWORD PTR [ebp-0x8],0x1
	<+138>:	mov    eax,DWORD PTR [ebp-0xc]
	<+141>:	sub    eax,0x1
	<+144>:	cmp    DWORD PTR [ebp-0x8],eax
	<+147>:	jl     0x530 <asm4+51>
	<+149>:	mov    eax,DWORD PTR [ebp-0x10]
	<+152>:	add    esp,0x10
	<+155>:	pop    ebx
	<+156>:	pop    ebp
	<+157>:	ret    
```

「asm4("picoCTF_724a2")は何を返すか？」という説明なので、上記コードの動作を解析して、"picoCTF_724a2"という文字がどのような出力になるかを調べれば良いことになります。  

まず、使われているレジスター名と命令の形式から、x86の32ビットスタイルであることが推測できます。  

しかし、このコードを1行ずつ解析していくのは大変手間がかかる作業になるため、各種サイトを調べてみたところ、GCCにasm関数があることが分かりましたた。  
アセンブリー言語のソースコードに少し手を加えることでC言語のコードとして実行できるものです。  
<br>
参考：[GCCのドキュメントサイト](https://gcc.gnu.org/onlinedocs/gcc/Extended-Asm.html)  
<br>
以上の内容をもとにして、C言語で書いたコードが以下になります。  

```
#include <stdio.h>
#include <stdlib.h>

int asm4(char* in)
{
    int value;

    asm (
        "nop;"
        "nop;"
        "nop;"
        //"push   ebp;"
        //"mov    ebp,esp;"
        "push   ebx;"
        "sub    esp,0x10;"
        "mov    DWORD PTR [ebp-0x10],0x252;"
        "mov    DWORD PTR [ebp-0xc],0x0;"
        "jmp    _asm_27;"
    "_asm_23:"
        "add    DWORD PTR [ebp-0xc],0x1;"
    "_asm_27:"
        "mov    edx,DWORD PTR [ebp-0xc];"
        "mov    eax,DWORD PTR [%[pInput]];"
        "add    eax,edx;"
        "movzx  eax,BYTE PTR [eax];"
        "test   al,al;"
        "jne    _asm_23;"
        "mov    DWORD PTR [ebp-0x8],0x1;"
        "jmp    _asm_138;"
    "_asm_51:"
        "mov    edx,DWORD PTR [ebp-0x8];"
        "mov    eax,DWORD PTR [%[pInput]];"
        "add    eax,edx;"
        "movzx  eax,BYTE PTR [eax];"
        "movsx  edx,al;"
        "mov    eax,DWORD PTR [ebp-0x8];"
        "lea    ecx,[eax-0x1];"
        "mov    eax,DWORD PTR [%[pInput]];"
        "add    eax,ecx;"
        "movzx  eax,BYTE PTR [eax];"
        "movsx  eax,al;"
        "sub    edx,eax;"
        "mov    eax,edx;"
        "mov    edx,eax;"
        "mov    eax,DWORD PTR [ebp-0x10];"
        "lea    ebx,[edx+eax*1];"
        "mov    eax,DWORD PTR [ebp-0x8];"
        "lea    edx,[eax+0x1];"
        "mov    eax,DWORD PTR [%[pInput]];"
        "add    eax,edx;"
        "movzx  eax,BYTE PTR [eax];"
        "movsx  edx,al;"
        "mov    ecx,DWORD PTR [ebp-0x8];"
        "mov    eax,DWORD PTR [%[pInput]];"
        "add    eax,ecx;"
        "movzx  eax,BYTE PTR [eax];"
        "movsx  eax,al;"
        "sub    edx,eax;"
        "mov    eax,edx;"
        "add    eax,ebx;"
        "mov    DWORD PTR [ebp-0x10],eax;"
        "add    DWORD PTR [ebp-0x8],0x1;"
    "_asm_138:"
        "mov    eax,DWORD PTR [ebp-0xc];"
        "sub    eax,0x1;"
        "cmp    DWORD PTR [ebp-0x8],eax;"
        "jl     _asm_51;"
        "mov    eax,DWORD PTR [ebp-0x10];"
        "add    esp,0x10;"
        "pop    ebx;"
        //"pop    ebp;"
        //"ret    ;"
        "nop;"
        "nop;"
        "nop;"
            :"=r"(value)
            : [pInput] "m"(in)
    );
    
    return value;
}

int main(int argc, char** argv)
{
    printf("0x%x\n", asm4("picoCTF_724a2"));
    
    return 0;
}
```

大まかな内容としては、ジャンプ命令をラベルに変更し、入力パラメータの名前もレジスター名から変更しています。関数のセットアップと解放処理はコンパイラーによって処理されるため、アセンブリー内ではコメントアウトしています。  
GCCのドキュメントや各種サイトの情報にもとづいて、お作法通りに記述したものになります。  

このC言語コードをsolver.cとして保存し、以下のようにgccコマンドでコンパイルしました。  

```
$ gcc -masm=intel -m32 solver.c -o solver
```

しかし、以下のエラーが出てしまいました。  

```
$ gcc -masm=intel -m32 solver.c -o solver
In file included from solver.c:1:
/usr/include/stdio.h:28:10: fatal error: bits/libc-header-start.h: No such file or directory
   28 | #include <bits/libc-header-start.h>
      |          ^~~~~~~~~~~~~~~~~~~~~~~~~~
compilation terminated.
```

調べてみたところ、32bit向けのコードをコンパイルするためのライブラリーが足りないようです。  
gcc-multilibが必要ということが分かったので、インストールします。  

```
$ sudo apt install gcc-multilib
```

gcc-multilibをインストール後、無事にコンパイルできました。  
作成された実行ファイルsolverを実行します。  

```
$ ./solver
0x20c
```

この「0x20c」がフラグになります。  
今回の問題は、「pciCTF{}」を付けず、この値をそのまま入力する内容になっています。  

<br>
<br>
<br>
<br>

## フラグ
0x20c  

<br>
<br>

## 補足

生成AIにアセンブリー言語のソースコードを解析してもらい、併せてC言語やPythonで動作するコードも作成してもらいました。  
Pythonのコードでは以下のようになりました。この処理をアセンブリー言語から読み取って解析できることを求めている問題ということになります。  

```
def asm4(s: str) -> int:
    result = 0x252  # 初期値：594

    length = len(s)
    for i in range(1, length - 1):
        diff1 = ord(s[i]) - ord(s[i - 1])
        diff2 = ord(s[i + 1]) - ord(s[i])
        result += diff1 + diff2

    return result

input_str = "picoCTF_724a2"
output = asm4(input_str)

print(f"Flag (hex): 0x{output:x}")
```


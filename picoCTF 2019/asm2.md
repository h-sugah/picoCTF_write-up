# asm2
Author: Sanjay C  

#### URL
https://play.picoctf.org/practice/challenge/16  

#### Description
What does asm2(0x4,0x21) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format. Source  

<br>
<br>
<br>
<br>

## 解答の過程
Sourceがリンクになっており、クリックするとファイルをダウンロードすることができます。  
ファイル名は「test.S」です。  
ファイルの種類を確認するとASCII textと表示されるので、アセンブリーのソースコードであることが分かります。  

```
$ file test.S
test.S: ASCII text
```

エディターでファイルの内容を確認します。  
以下のコードが書かれていました。  

```
asm2:
	<+0>:	push   ebp
	<+1>:	mov    ebp,esp
	<+3>:	sub    esp,0x10
	<+6>:	mov    eax,DWORD PTR [ebp+0xc]
	<+9>:	mov    DWORD PTR [ebp-0x4],eax
	<+12>:	mov    eax,DWORD PTR [ebp+0x8]
	<+15>:	mov    DWORD PTR [ebp-0x8],eax
	<+18>:	jmp    0x509 <asm2+28>
	<+20>:	add    DWORD PTR [ebp-0x4],0x1
	<+24>:	add    DWORD PTR [ebp-0x8],0x74
	<+28>:	cmp    DWORD PTR [ebp-0x8],0xfb46
	<+35>:	jle    0x501 <asm2+20>
	<+37>:	mov    eax,DWORD PTR [ebp-0x4]
	<+40>:	leave  
	<+41>:	ret    
```

最初の行に「asm2」とラベル付けされているので、asm2関数です。  
問題文から、0x4と0x21を引数として与えてasm2関数を実行したときに何が出力されるかを問うているので、アセンブリーコードの動作を解析する問題です。  

コードに書かれている処理を確認していきます。  
まず、+0、+1、+3の行は、関数プロローグで、スタックポインターを16バイト分引き、変数の領域を確保しています。  
この時点で1番目の引数の値（0x4）は[ebp+0x8]に、2番目の引数の値（0x21）は[ebp+0xc]にセットされています。  

```
<+6>:	mov    eax,DWORD PTR [ebp+0xc]
<+9>:	mov    DWORD PTR [ebp-0x4],eax
```
2番目の引数の値（0x21）を受け取り、[ebp-0x4]にセットしています。  
<br>

```
<+12>:	mov    eax,DWORD PTR [ebp+0x8]
<+15>:	mov    DWORD PTR [ebp-0x8],eax
```
1番目の引数の値（0x4）を受け取り、[ebp-0x8]にセットしています。  
<br>

```
<+18>:	jmp    0x509 <asm2+28>
```
<asm+28>へジャンプします。つまり、<+28>の行へ進みます。  
<br>

```
<+28>:	cmp    DWORD PTR [ebp-0x8],0xfb46
<+35>:	jle    0x501 <asm2+20>
```
[ebp-0x8]を0xfb46と比較します。つまり、1番目の引数の値（0x4）と0xfb46を比較します。  
比較した結果が「以下（le = less or equal）」なら、<asm2+20>へジャンプします。  
1番目の引数の値が0xfb46より大きければ、そのまま次の行へ進みます。  
<br>

```
<+20>:	add    DWORD PTR [ebp-0x4],0x1
<+24>:	add    DWORD PTR [ebp-0x8],0x74
```
[ebp-0x4]を+1します。つまり、2番目の引数の値を+1します。  
そして、[ebp-0x8]を+0x74 = 116します。つまり、1番目の引数の値を+116します。  

再び、<+28>と<+35>で比較とジャンプの処理に入り、条件が満たされるまで繰り返されます。  
<br>

```
<+37>:	mov    eax,DWORD PTR [ebp-0x4]
<+40>:	leave  
<+41>:	ret    
```
[ebp-0x4]がeaxレジスターにセットされて、関数が終了します。ポインター類も元に戻しているようです。  
関数から返ってくる値はeaxレジスターなので、[ebp-0x4]の2番目の引数の値になります。  

以上より、ループしている箇所の条件を考慮しながら足し算してけば良いことになります。  
1番目の引数の値は、0x4 = 4です。0xfb46 = 64326より大きくなるまで+1を繰り返すので、(64326 - 4) / 116 = 554.5 となり、555回繰り返すことになります。  

2番目の引数の初期値は0x21なので、0x21 = 33 + 555 = 588 = 0x24cです。  

これがフラグになります。  

<br>
<br>
<br>
<br>

## フラグ
0x24c  

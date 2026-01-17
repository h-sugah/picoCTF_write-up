# asm1
Author: Sanjay C  

#### URL
https://play.picoctf.org/practice/challenge/20  

#### Description
What does asm1(0x6fa) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format. Source  

<br>
<br>
<br>
<br>

## 解答の過程
「Source」をクリックするとファイルをダウンロードできます。  
ファイル名は「test.S」です。  
エディターでファイルを開き、内容を確認します。  
以下のアセンブリーコードが記載されていました。  

```
asm1:
	<+0>:	push   ebp
	<+1>:	mov    ebp,esp
	<+3>:	cmp    DWORD PTR [ebp+0x8],0x3a2
	<+10>:	jg     0x512 <asm1+37>
	<+12>:	cmp    DWORD PTR [ebp+0x8],0x358
	<+19>:	jne    0x50a <asm1+29>
	<+21>:	mov    eax,DWORD PTR [ebp+0x8]
	<+24>:	add    eax,0x12
	<+27>:	jmp    0x529 <asm1+60>
	<+29>:	mov    eax,DWORD PTR [ebp+0x8]
	<+32>:	sub    eax,0x12
	<+35>:	jmp    0x529 <asm1+60>
	<+37>:	cmp    DWORD PTR [ebp+0x8],0x6fa
	<+44>:	jne    0x523 <asm1+54>
	<+46>:	mov    eax,DWORD PTR [ebp+0x8]
	<+49>:	sub    eax,0x12
	<+52>:	jmp    0x529 <asm1+60>
	<+54>:	mov    eax,DWORD PTR [ebp+0x8]
	<+57>:	add    eax,0x12
	<+60>:	pop    ebp
	<+61>:	ret    
```

説明文には、0x6fa（10進数で1786）を与えたときにどのような値を返すか？ということなので、上から順に処理を確認していくこととします。  

```
<+0>:	push   ebp
<+1>:	mov    ebp,esp
```
この部分はプロローグになるので、特に気にしないで先に進みます。  

```
<+3>:	cmp    DWORD PTR [ebp+0x8],0x3a2
<+10>:	jg     0x512 <asm1+37>
```
[ebp+0x8]に0x6faがセットされており、0x3a2と比較します。そして、[ebp+0x8]が0x3a2より大きければ、<+37>へジャンプします。  
0x6fa > 0x3asなので、<+37>へジャンプすることになります。  

```
<+37>:	cmp    DWORD PTR [ebp+0x8],0x6fa
<+44>:	jne    0x523 <asm1+54>
```
[ebp+0x8]と0x6faを比較します。そして、不一致していれば<+54>へジャンプします。  
0x6fa = 0x6faなので、一致してジャンプせずに次の処理へ進むことになります。  

```
<+46>:	mov    eax,DWORD PTR [ebp+0x8]
<+49>:	sub    eax,0x12
<+52>:	jmp    0x529 <asm1+60>
```
[ebp+0x8]をeaxレジスターにセットし、eaxレジスターにセットされた値から0x12引きます。  
eaxレジスターにセットされた値は、0x6fa - 0x12 = 0x6e8になります。  
そして、<+60>へジャンプします。  

```
<+60>:	pop    ebp
<+61>:	ret    
```
関数のエピローグとして、値が返されることになります。  
eaxレジスターの値は、0x6e8です。これがフラグになります。  

<br>
<br>
<br>
<br>

## フラグ
0x6e8  

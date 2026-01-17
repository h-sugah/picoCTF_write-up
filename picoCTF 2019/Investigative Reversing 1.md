# Investigative Reversing 1
Author: Danny Tunitis  

#### URL
https://play.picoctf.org/practice/challenge/27  

#### Description
We have recovered a binary and a few images: image, image2, image3. See what you can make of it. There should be a flag somewhere.  

<br>
<br>
<br>
<br>

## 解答の過程
「binary」、「image」、「image2」、「image3」がそれぞれリンクになっており、クリックするとファイルをダウンロードすることができます。  
ファイル名は、順に「mystery」、「mystery.png」、「mystery2.png」、「mystery3.png」です。  
ファイルを確認すると、バイナリーファイルとPNGファイルであることが分かります。  

```
$ file mystery
mystery: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=1b08f7a782a77a6eeb80d7c1d621b4f16f76200a, not stripped

$ file mystery*.png
mystery.png:  PNG image data, 1411 x 648, 8-bit/color RGB, non-interlaced
mystery2.png: PNG image data, 1411 x 648, 8-bit/color RGB, non-interlaced
mystery3.png: PNG image data, 1411 x 648, 8-bit/color RGB, non-interlaced
```

3つのPNGファイルを開いてみると、どれも同じメッセージが描かれています。  
メッセージを見ても特に手がかりはなさそうなので、バイナリーエディターでファイルを確認してみます。  
macOSのHex Fiendというバイナリーエディターでファイルを確認しました。このバイナリエディターは、ファイルの構造を把握しやすくするためのテンプレートが用意されています。PNGのテンプレートを使って確認します。  
各ファイルのPNGデータ終了を示すIENDの後に、余計な塊のデータが存在しています。テキスト部分を見るとフラグのような文字列に見えます。  

【mystery.png】
![challenge-27--figure1](pictures/challenge-27--figure1.png)
このファイルでは、以下の文字列を確認することができます。  
> CF{An1_9a47141}`

<br>

【mystery2.png】
![challenge-27--figure2](pictures/challenge-27--figure2.png)
このファイルでは、以下のバイナリー値を確認することができます。  
> 0x85 0x73

<br>

【mystery3.png】
![challenge-27--figure3](pictures/challenge-27--figure3.png)
このファイルでは、以下の文字列を確認することができます。
> icT0tha_

<br>

これらの文字列からフラグを得るには、mysteryが鍵になると言えます。  
そこで、mysteryがどのような処理をしているのか、Ghidraを使って解析します。  

### Ghidraでの解析
Ghidraでmysteryをインポートし、解析させ、コードブラウザーでmain関数のデコンパイル結果を確認します。  
私の環境では、以下のような内容でした。  

```
void main(void)
{
  FILE *__stream;
  FILE *__stream_00;
  FILE *__stream_01;
  FILE *__stream_02;
  long in_FS_OFFSET;
  char local_6b;
  int local_68;
  int local_64;
  int local_60;
  char local_38 [4];
  char local_34;
  char local_33;
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  __stream = fopen("flag.txt","r");
  __stream_00 = fopen("mystery.png","a");
  __stream_01 = fopen("mystery2.png","a");
  __stream_02 = fopen("mystery3.png","a");
  if (__stream == (FILE *)0x0) {
    puts("No flag found, please make sure this is run on the server");
  }
  if (__stream_00 == (FILE *)0x0) {
    puts("mystery.png is missing, please run this on the server");
  }
  fread(local_38,0x1a,1,__stream);
  fputc((int)local_38[1],__stream_02);
  fputc((int)(char)(local_38[0] + '\x15'),__stream_01);
  fputc((int)local_38[2],__stream_02);
  local_6b = local_38[3];
  fputc((int)local_33,__stream_02);
  fputc((int)local_34,__stream_00);
  for (local_68 = 6; local_68 < 10; local_68 = local_68 + 1) {
    local_6b = local_6b + '\x01';
    fputc((int)local_38[local_68],__stream_00);
  }
  fputc((int)local_6b,__stream_01);
  for (local_64 = 10; local_64 < 0xf; local_64 = local_64 + 1) {
    fputc((int)local_38[local_64],__stream_02);
  }
  for (local_60 = 0xf; local_60 < 0x1a; local_60 = local_60 + 1) {
    fputc((int)local_38[local_60],__stream_00);
  }
  fclose(__stream_00);
  fclose(__stream);
  if (local_10 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return;
}
```

<br>

デコンパイルされたコードの変数などを整理して、処理の流れを把握しやすくします。  
しかし、Ghidraが適切にデコンパイルしていないことが分かりました。  
以下の箇所が該当し、正しい処理と思われる内容に修正する必要が生じました。  

```
  char local_38 [4];
  char local_34;
  char local_33;

～ 略 ～

  fputc((int)local_33,__stream_02);
  fputc((int)local_34,__stream_00);

```
まず、配列の要素数が4になっています。後半のコードを確認すると配列の要素数は26が正しいことが分かります。  
次に、値が不明なままlocal_33とlocal_34を使用しています。
コード全体を見て考えてみると、配列を扱う処理部分があり、次にfor文の処理部分があります。for文の初期値は6から始まっており、これは配列の7要素目を扱うことが分かります。  
これにより、前段の配列を扱う処理部分は6要素分を処理する内容であることが分かります。  
変数定義の順序から考えると、local_34は配列の5要素目（つまり、local_38[4]として見る）、local_33は配列の6要素目（つまり、local_38[5]として見る）にするものと考えられます。  

<br>

以上のような推測をもとにして整理した結果が以下のコードになります。  

```
void main(void)
{
  FILE *__stream_flag;
  FILE *__stream_mystery;
  FILE *__stream_mystery2;
  FILE *__stream_mystery3;
  char temp;
  int i;
  int j;
  int k;
  char buffer_flag[26];
  
  __stream_flag = fopen("flag.txt","r");
  __stream_mystery = fopen("mystery.png","a");
  __stream_mystery2 = fopen("mystery2.png","a");
  __stream_mystery3 = fopen("mystery3.png","a");

  if (__stream_flag == (FILE *)0x0) {
    puts("No flag found, please make sure this is run on the server");
  }

  if (__stream_mystery == (FILE *)0x0) {
    puts("mystery.png is missing, please run this on the server");
  }

  fread(buffer_flag,26,1,__stream_flag);
    // 「flag.txt」から26バイト読み込み、buffer_flagにセットする。
  
  fputc((int)buffer_flag[1],__stream_mystery3);
    // buffer_flag[1]をmystery3.pngに追記する。
  
  fputc((int)(char)(buffer_flag[0] + '\x15'),__stream_mystery2);
    // buffer_flag[0] + '0x15'をmystery2.pngに追記する。
  
  fputc((int)buffer_flag[2],__stream_mystery3);
    // buffer_flag[2]をmystery3.pngに追記する。
  
  temp = buffer_flag[3];
    // buffer_flag[3]をtempにセットする。
  
  fputc((int)buffer_flag[5],__stream_mystery3);
    // buffer_flag[5]をmystery3に追記する。
  
  fputc((int)buffer_flag[4],__stream_mystery);
    // buffer_flag[4]をmystery.pngに追記する。

  for (i = 6; i < 10; i = i + 1) {
    temp = temp + '\x01';
      // temp += 0x01

    fputc((int)buffer_flag[i],__stream_mystery);
      // buffer_flag[i]をmystery.pngに追記する。
  }
  fputc((int)temp,__stream_mystery2);
    // tempをmystery2.pngに追記する。

  for (j = 10; j < 15; j = j + 1) {
    fputc((int)buffer_flag[j],__stream_mystery3);
      // buffer_flag[j]をmystery3.pngに追記する。
  }

  for (k = 15; k < 26; k = k + 1) {
    fputc((int)buffer_flag[k],__stream_mystery);
      // buffer_flag[k]をmystery.pngに追記する。
  }

  fclose(__stream_mystery);
  fclose(__stream_flag);

  return;
}
```

これで全体的な処理の流れが把握できました。  
mysteryでフラグを分解して、いくつかの文字を置換し、各PNGファイルの末尾にフラグの断片を追記したということになります。  

### フラグを構成する
処理内容を把握できたので、フラグの断片を元に戻すためのプログラムを作成します。  
Pythonを使い、以下のようなコードを作成しました。  
各PNGファイルから得られたフラグの断片を文字列とバイナリー値として扱い、デコンパイルされたコードの流れに従って文字をフラグに戻していきます。  
mystery1、mystery2、mystery3の文字列は、配列として扱い、使うごとに参照する要素を増やしていきます。  

```
mystery1 = "CF{An1_9a47141}`"
mystery2 = b"\x85\x73"
mystery3 = "icT0tha_"

flag = [0] * 26

flag[1] = mystery3[0]
flag[0] = chr(mystery2[0] - 0x15)
flag[2] = mystery3[1]
temp = mystery2[1]
flag[5] = mystery3[2]
flag[4] = mystery1[0]

for i in range(6, 10):
    temp = temp - 0x01
    flag[i] = mystery1[i - 5]

flag[3] = chr(temp)

for j in range(10, 15):
    flag[j] = mystery3[j - 7]

for k in range(15, 26):
    flag[k] = mystery1[k - 10]

print(''.join(flag))
```

<br>

このコードを実行することで、フラグが出力されます。  
なお、mystery1の最後の文字はバッククォートです。これはフラグには含まれないので注意してください。  

```
$ python3 solver.py
picoCTF{An0tha_1_9a47141} 
```

<br>
<br>
<br>
<br>

## フラグ
picoCTF{An0tha_1_9a47141}  


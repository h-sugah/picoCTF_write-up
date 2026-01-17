# strings it
Author: Sanjay C/Danny Tunitis  

#### URL
https://play.picoctf.org/practice/challenge/37  

#### Description
Can you find the flag in file without running it?  

<br>
<br>
<br>
<br>

## 解答の過程
「file」をクリックするとファイルをダウンロードすることができます。  
ファイル名は「strings」です。  

Linuxでターミナルを開き、fileコマンドを実行するとELF形式の実行ファイルであることが分かります。  
```
$ file strings
strings: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=f2a0ca8fb0864f86d1acc094c0793148e0b00b14, for GNU/Linux 3.2.0, not stripped
```

試しに実行してみます。  
```
$ ./strings
Maybe try the 'strings' function? Take a look at the man page
```

stringsコマンドを使うように提案するメッセージが出力されました。  
問題のタイトルやファイル名から判断できるように、ここはstringsコマンドを使います。  

ただ、以下のように実行すると大量のテキストが出力されます。  
```
$ strings strings
```

大量のテキストの中にフラグが存在することを確認できるので、grepコマンドと組み合わせます。  
```
$ strings strings |grep pico
picoCTF{5tRIng5_1T_A1b9ECAa}
```

<br>
<br>
<br>
<br>

## フラグ
picoCTF{5tRIng5_1T_A1b9ECAa}  

<br>
<br>

## 補足
stringsコマンド  

```
$ strings --help
Usage: strings [option(s)] [file(s)]
 Display printable strings in [file(s)] (stdin by default)
 The options are:
  -a - --all                Scan the entire file, not just the data section [default]
  -d --data                 Only scan the data sections in the file
  -f --print-file-name      Print the name of the file before each string
  -n <number>               Locate & print any sequence of at least <number>
    --bytes=<number>         displayable characters.  (The default is 4).
  -t --radix={o,d,x}        Print the location of the string in base 8, 10 or 16
  -w --include-all-whitespace Include all whitespace as valid string characters
  -o                        An alias for --radix=o
  -T --target=<BFDNAME>     Specify the binary file format
  -e --encoding={s,S,b,l,B,L} Select character size and endianness:
                            s = 7-bit, S = 8-bit, {b,l} = 16-bit, {B,L} = 32-bit
  --unicode={default|show|invalid|hex|escape|highlight}
  -U {d|s|i|x|e|h}          Specify how to treat UTF-8 encoded unicode characters
  -s --output-separator=<string> String used to separate strings in output.
  @<file>                   Read options from <file>
  -h --help                 Display this information
  -v -V --version           Print the program's version number
strings: supported targets: elf64-x86-64 elf32-i386 elf32-iamcu elf32-x86-64 pei-i386 pe-x86-64 pei-x86-64 elf64-little elf64-big elf32-little elf32-big pe-bigobj-x86-64 pe-i386 pdb srec symbolsrec verilog tekhex binary ihex plugin
Report bugs to <https://sourceware.org/bugzilla/>
```

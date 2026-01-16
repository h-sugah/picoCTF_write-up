# WebNet1
Author: Jason  

#### URL
https://play.picoctf.org/practice/challenge/42  

#### Description
We found this packet capture and key. Recover the flag.  

<br>
<br>
<br>
<br>

## 解答の過程
packet captureとkeyを選択するとこで、ファイルをダウンロードできます。  
それぞれ、ファイル名は「capture.pcap」と「picopico.key」です。  
ネットワークキャプチャーファイルとPEM形式のプライベートキーファイルです。  

Wiresharkでpcapファイルを開きます。  

![[challenge-42--figure1.png]]

Protocol欄にTLSv1.2の記載があるので、プライベートキーで暗号化を解除することができそうです。  
まず、ダウンロードしたpicopicp.keyをWiresharkに登録します。  
Preferencesのダイアログを表示し、左側ペインからRSA Keysを選択します。  

![[challenge-42--figure2.png]]

RSA Keys画面では、「Add new key file...」ボタンを押して「picopico.key」ファイルを登録します。  

![[challenge-42--figure3.png]]

ファイルを登録したら「OK」ボタンを押します。  
ダイアログが閉じられ、メイン画面に戻ったらパケットキャプチャーファイルをリロードします。  
すると、TLSで暗号化されたパケットが復号されて、HTTPのパケットが現れます。  

![[challenge-42--figure4.png]]

暗号化されたパケットのデータが復号されたので、まずはパケットのデータにフラグがないか確認してみます。  
Wiresharkのメニューから Edit ＞ Find Packet と選択し、以下を設定して検索欄に「picoCTF」と入力して検索します。  
- Packet details  
- String  
- Multiple Occurrences にチェックを入れる  

![[challenge-42--figure5.png]]

囮のフラグを検索しますが、何回か検索を続けると正規のフラグがヒットします。  

<br>
<br>
<br>
<br>

## フラグ
picoCTF{honey.roasted.peanuts}  

<br>
<br>

なお、HTTPのパケットデータからhtmlページに関するファイル一式を取得できますが、フラグとは関係ないものでした。  

# WebNet0
Author: Jason  

#### URL
https://play.picoctf.org/practice/challenge/32  

#### Description
We found this packet capture and key. Recover the flag.  

<br>
<br>
<br>
<br>

## 解答の過程
「packet capture」と「key」をクリックするとファイルをダウンロードすることができます。  
ファイル名は、それぞれ「capture.pcap」と「picopico.key」です。  

capture.pcapは、パケットキャプチャーされたpcapファイルなので、Wiresharkで開きます。  
以下のようなデータでした。  

![challenge-32--figure1.png](pictures/challenge-32--figure1.png)

picopico.keyをテキストエディターで開くと以下の内容を確認できます。プライベートキーのようです。  
```
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCwKlFPNKjseJF5
puCJU5x38XcT1eQge5zOKNahAlYudvGVOEs61TnIgvcER4ko8i3OCwak2/atcGk3
oz9jFKep7XFEYNP31IwwD9j/YazlKy4DRLGObOyIZUU1f2WRA7Uhf0POQXsDT1oU
X32jMKZkQSSDW4MRZd9trJYdO2TrcEPMsBiZQlFlvgnNwl3QlawozTHLAJKI36j1
cPwSMMeNca1e0Zi1s7R5IxfhpNXOBF0FmxiWvmeOHbaspyHg8UEmGBrkd4k4wXSK
GQvrc8QjycP4ScEdquxJiYnDT8iEbAq70/7f/5NIN1DE9YoGJqKYjTS9nRPB4Yvj
JN/SJnhvAgMBAAECggEACCnd3LrG/TZVH3sROqvqO1CwQPYPfUXdLVyNHab7EWon
pc+XBOHurJENG2CpRYF7h+nQ5ADhfIYSCicBf/jsEB7VueJ20CxEVtHVL3h6R6Bp
oHMle0Em8OcofuMpdL/kO+om3T8BkVSzCvCl5NMTUuAF7iRmfX7oDLALwM0IzzQv
2un+2UmT15rgAZfl3IL1PGvJhbhLxfeeyPE9MBy1SqBjQ9rNFn8sQv959J6BHz4b
EpK//ErtNP2yh7oiVBBgKEQ1gEuOjQC/4oxoqCFfZaf9XNRCxB/zY1nUprvJyz09
NMQWNF2EmvmBVGfoTxmuut5N0GbVr2UyHxWMKm2sOQKBgQDpb2+AWgWlGtetuLKJ
fJs8dnd6LhnafbKCOXMOT68qMBRoTpBtVTLRVSNvWCm8m4TTEazX4+ZA+bJFwUFw
aATDmHcr6lMI3tNKrcsnY2F7o5I4z6mwuRuSeszq/ndxZqCzwCu4nKixh3cznp7j
JiElNG0d8Lu5eQgmVAK1AhWXfQKBgQDBMa9ga7VJUP4pzcHnWAoi34OpfjvQYeGl
IKL3AKO4OedaHdH9qid41PQHnL7O3xzN669SkLZ5s0d88A/LFLk4oZNMKdkSTQIQ
+AMbXH01HGFvnCOuPg/FbNp1wS7zJEg5u5HFQWyMPNJLr/hZ6g2Yp+UGpAcGTwM/
RCPVAPhLWwKBgQDAB0OaOnPaVjKGXiHAqBirrGiswa/S5QQrzEaxxys5cUPYaoi0
6BldysPTnJr45JZna2rcTkXjvYTBjTDf3zHMFWgzYBfefC8kh8NPK5nNs8ldorbd
AemEnjBkP+DSELKyK6vLulOrdtzAQgRCp+MsT+xTbO2ArefeX826SXSpoQKBgC2v
nDOHBQXje1dTawlUToFUrgQE8AwlOYEdKKyUoCLOvqEW8DO2a0MtyM+MB6tQI7Wm
iH1T73L0LHGlK3bw3aRAwV5/fu/O+jAdFk8AHjPTFE+acu2fi4c6aKb0GjAxYksU
yjIFeK/pKinv4SESMkjpW0WowGiDgtcRPBAA/LaFAoGAfEM1rfM0v3UmB7PS6u0m
P3ckP2CFCdaryXPfC52GBcJ3Q46YpsQvLTVotM+teHvTjNw2jwwZxIl4NenGSEj3
KDhQoOiQC9BrDD+DB4I9+T9nxT3g7R6MrgITghB4We7TVhL/PljnJTyDqpjNA4kY
TveAJPv6Xq1ERt5PUtX3BqQ=
-----END PRIVATE KEY-----
```


ヒントを参照すると、2番目のヒントが「TLSストリームを複合するにはどうするか？」と記載されています。  
これにより、プライベートキーを用いて、PCAPデータからTLSデータを復号すれば良いことが分かります。  

WiresharkのwikiにTLSの復号について解説されているので、内容を確認します。  
https://wiki.wireshark.org/TLS#tls-decryption  

このページの解説には、Wireshark 3.0以降で新しいRSA鍵ダイアログが追加されていることが書かれています。そして、従来のRSA秘密鍵を設定するダイアログは非推奨になり、将来削除される可能性があると記載されているので、新しいRSA鍵ダイアログを利用してみます。  

メニューから「編集」→「設定」→「RSA鍵」と選択し、RSA鍵設定ダイアログを表示します。  
次に、「新規鍵ファイルを追加」ボタンを押して、ダウンロードしたpicopico.keyを登録します。  

![challenge-32--figure2.png](pictures/challenge-32--figure2.png)

鍵ファイルを登録したら「OK」ボタンを押して、メインの画面に戻ります。  
そして、Protocol欄に「TLSv1.2」と表示されているパケットを1つ選択して右クリックし、ポップアップメニューから「...としてデコード」を選択します。  

![challenge-32--figure3.png](pictures/challenge-32--figure3.png)

Wireshark・...としてデコードダイアログが表示されます。対象になるデコード形式を選択して「OK」ボタンを押します。  

![challenge-32--figure7.png](pictures/challenge-32--figure4.png)

メインのキャプチャー画面では、デコードされたパケットが緑色で強調表示されます。Protocol欄において、TLSv1.2からHTTPに変化しました。  

![challenge-32--figure8.png](pictures/challenge-32--figure5.png)

このデコードされたHTTPのパケットデータから、レスポンス（Source 172.31.22.220 → Destination 128.237.140.23）になっているデータを選択して詳細を見ると、ヘッダーの部分にフラグを記載されているのが見つかります。  

![challenge-32--figure9.png](pictures/challenge-32--figure6.png)

<br>
<br>
<br>
<br>

## フラグ
picoCTF{nongshim.shrimp.crackers}  


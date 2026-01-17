# waves over lambda
Author: invisibility/Danny  

#### URL
https://play.picoctf.org/practice/challenge/38  

#### Description
We made a lot of substitutions to encrypt this. Can you decrypt it?  
Additional details will be available after launching your challenge instance.  

<br>
<br>
<br>
<br>

## 解答の過程
「Launch Instance」ボタンを押すことで追加の説明が表示されます。  
```
We made a lot of substitutions to encrypt this. Can you decrypt it?
Connect with nc fickle-tempest.picoctf.net 54602.
```

netcatコマンドで指定のサイトに接続する指示が記載されているので、接続してみます。  
```
$ nc fickle-tempest.picoctf.net 54602
-------------------------------------------------------------------------------
sgowqbpu jkqk nu cgtq eibw - eqkytkosc_nu_s_gvkq_ibalmb_8003e343
-------------------------------------------------------------------------------
jbvnow jbm ugak pnak bp ac mnuhgubi djko no igomgo, n jbm vnunpkm pjk lqnpnuj atukta, bom abmk ukbqsj bagow pjk lggxu bom abhu no pjk inlqbqc qkwbqmnow pqboucivbonb; np jbm upqtsx ak pjbp ugak egqkxogdikmwk ge pjk sgtopqc sgtim jbqmic ebni pg jbvk ugak nahgqpbosk no mkbinow dnpj b oglikabo ge pjbp sgtopqc. n enom pjbp pjk mnupqnsp jk obakm nu no pjk krpqkak kbup ge pjk sgtopqc, ftup go pjk lgqmkqu ge pjqkk upbpku, pqboucivbonb, agimbvnb bom ltxgvnob, no pjk anmup ge pjk sbqhbpjnbo agtopbnou; gok ge pjk dnimkup bom ikbup xogdo hgqpngou ge ktqghk. n dbu ogp blik pg inwjp go boc abh gq dgqx wnvnow pjk krbsp igsbinpc ge pjk sbupik mqbstib, bu pjkqk bqk og abhu ge pjnu sgtopqc bu ckp pg sgahbqk dnpj gtq gdo gqmobosk utqvkc abhu; ltp n egtom pjbp lnupqnpz, pjk hgup pgdo obakm lc sgtop mqbstib, nu b ebnqic dkii-xogdo hibsk. n ujbii kopkq jkqk ugak ge ac ogpku, bu pjkc abc qkeqkuj ac akagqc djko n pbix gvkq ac pqbvkiu dnpj anob.
```

暗号文が出力されました。  
どのように解読していくべきでしょうか。  
ヒントらしきキーワードになるのは、タイトルか問題文中の「We made a lot of substitutions to encrypt this.」です。タイトルで使われているlambdaは、暗号に関するものではなさそうです。  
「substitution」をキーワードに調べると、substitution cipher（換字式暗号）があります。  

Wilipedia：https://ja.wikipedia.org/wiki/%E6%8F%9B%E5%AD%97%E5%BC%8F%E6%9A%97%E5%8F%B7  

換字式暗号だとすると、どの文字が、元の文字に変換されたのか調べていく必要があります。変換ルールを導き出すために長い文章が用意されているのでしょうが、調べていく手間が大きく、大変です。  

そこで、以下のサイトで自動的に処理させます。  

dCode：https://www.dcode.fr/monoalphabetic-substitution  

手順は以下のようになります。  
1. Monoalphabetic Substitution DecoderのAlphabetic substitution ciphertext欄に暗号文を丸ごとコピーして貼り付ける。  
2. 「DECRYPT AUTOMATOCALLY」ボタンを押す。  

これで自動的に暗号文を解読してくれます。また、換字ルールも出力してくれます。  
解読された文は以下のようになります。  
```
-------------------------------------------------------------------------------
congrats here is your flag - frequency_is_c_over_lambda_8003f343
-------------------------------------------------------------------------------
having had some time at my disposal when in london, i had visited the british museum, and made search among the books and maps in the library regarding transylvania; it had struck me that some foreknowledge of the country could hardly fail to have some importance in dealing with a nobleman of that country. i find that the district he named is in the extreme east of the country, just on the borders of three states, transylvania, moldavia and bukovina, in the midst of the carpathian mountains; one of the wildest and least known portions of europe. i was not able to light on any map or work giving the exact locality of the castle dracula, as there are no maps of this country as yet to compare with our own ordnance survey maps; but i found that bistritz, the post town named by count dracula, is a fairly well-known place. i shall enter here some of my notes, as they may refresh my memory when i talk over my travels with mina.
```

ヒントには、フラグの形式は通常のものではないことが記載されています。  
picoCTF{ }のフォーマットを使わないで回答します。  

<br>
<br>
<br>
<br>

## フラグ
frequency_is_c_over_lambda_8003f343  

<br>
<br>

## 補足
dCodeサイトは、様々な種類の暗号解読ツールを提供しています。  

dCode：https://www.dcode.fr/en  

トップページ左上のSearch for a toolで検索するか、Browse the [full dCode tools' list](https://www.dcode.fr/tools-list)からツール一覧を表示することができます。  


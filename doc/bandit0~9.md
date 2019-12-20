# CTF練習(コマンド) CTFサークルで使うためにdoc残す。  

https://overthewire.org/wargames/bandit/bandit0.html  

wsl2のUbuntu環境で行っています。インストール方法は以下参照    
https://qiita.com/Aruneko/items/c79810b0b015bebf30bb  

ssh接続ができるように設定する。  
https://qiita.com/hnishi/items/5dec4c7fca9b5121430f  


わからないコマンドはmanや-hで調べる。かググる。  
```$ man ls```みたいに  

コマンド一覧みたいな  
https://hydrocul.github.io/wiki/commands/  


## 0  
sshで接続する。ssh user@ipaddress ポートは-pで2220  
ssh username@ipaddress ssh接続　方法で調べる  

```$ ssh bandit0@bandit.labs.overthewire.org -p 2220```  

```$ ssh bandit.labs.overthewire.org -p2220 -l bandit0```  
でも接続できる。  
vi /etc/ssh/sshd_configvi /etc/ssh/sshd_config

```bandit0@bandit.labs.overthewire.org's password:bandit0```  
パスにbandit0を入力パスは見えない  

## 0->1  

lsでホームディレクトリにあるファイルの表示  

```
$ ls  
readme  
```  
catで中見みる。
```
$ cat readme  
boJ9jbbUNNfktd78OOpsqOltutMc3MY1  
```  
$ less readme  
でもおけ
これが暗号。次のレベルのパスワードになるため、コピーしておく。  
```$ exit```  
で抜ける。  


## 1->2  

```$ ssh bandit1@bandit.labs.overthewire.org -p 2220```  

```
bandit0@bandit.labs.overthewire.org's password:先ほどの暗号を入力  
```
```
$ ls
-
```  
```
$ cat -
```  
だとでない。  
「-」がオプションを指定するための特別なものだから。  
つまり、catコマンドはオプションの入力を待つ状態になり、ファイル名として認識しない。  
このような時は相対パスや絶対パスで指定する。  

```
$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```  

```
$ cat < -
```  
でもよい。この記号<は標準入力をキーボードからファイルに切り替えるもの  

もっと簡単に言うなら、「キーボードから入力された情報じゃなくて、ファイルの内容を使うよ」というもの  

```
$ exit
```
で抜ける。  

## 2->3  

```
$ ssh bandit2@bandit.labs.overthewire.org -p 2220
```   
パスワードを入力(省略)  

ホームディレクトリの"spaces in this filename"というファイルにパスワードがある。
ファイル名に空白が入っているので、ファイル名指定の際に空白を"\"(Windowsでは"￥")でエスケープするか、  
ダブルクォーテーション""で囲む必要がある。
実際にはshの補完機能を使えば一発なのでtabで解決。
```
$ cat spaces\ in\ this\ filename  
$ cat "spaces in this filename"
```  
```
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```  
$ exitで抜ける。  

## 3->4  

```
$ ssh bandit3@bandit.labs.overthewire.org -p 2220
```  

ホームディレクリの"inhere"というディレクトリにパスワードがあるらしい。
$ cd inhere/で移動して$ lsでファイルを確認するも何も見つからない。
これはパスワードのあるファイルが"."から始まる隠しファイルになっているから。
lsで隠しファイルを表示する-aオプションをつけてファイル名を確認したら中身を確認して終わり。
$ ls -a
```
$ ls
$ inhere
$ cd inhere
$ ls -a
```  
```
pIwrPrtPN36QITSp3EQaw936yaFoFgAB  
```  

$ exitで抜ける。  

## 4->5  


```
$ ssh bandit4@bandit.labs.overthewire.org -p 2220
```  
```$ cd inhere```  
先頭が"-"なのでLevel1〜2と同様にパスを指定する。  
```$ cat ./-file07```  
workディレクトリ内のファイルすべての中からaという文字を検索する場合は、次のコマンドだ。  
```$ file inhere/*```  
人が読めるので  
ASCII text  
ディレクトリ内であれば  
```file ./* ```  


koReBOKuIDDepwhWk7jZC0RTdopnAYKh

## 5->6  

```
$ ssh bandit5@bandit.labs.overthewire.org -p 2220
``` 
```
$ find -ls |grep 1033  
$ cd maybehere07
$ ls -a
$ cat .file2  
```  

```
find ./ -size 1033c
./カレントディレクトリ
-size ファイルサイズ　1033 cがバイト kがキロバイと
```  

-type f
ファイルを対象として検索
-exec cat {} \;
検索結果に対してコマンドを実行する。
は\でエスケープする。また、対象となる全ファイルという意味で{}を記述している
ワンライナーてきなのできる。

```DXjZPULLxYr17uwoI01bNLQbtFemEgo7```

## 6->7
```
$ ssh bandit6@bandit.labs.overthewire.org -p 2220
```   

```$ find / -ls |grep -w 33|grep bandit7 |grep bandit6```    
permission deniedが出る。  
こういうときは、```2>```とやるとエラー出力先を画面じゃなくて特定のファイルに変更できる。  
```/dev/null```は書き込んでも何も起こらないブラックホール。こうすることで目的の出力だけ出る。
```$ find / -ls 2>/dev/null|grep -w 33|grep bandit7 |grep bandit6```  
execする。  
```$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null -exec cat {} \;```  

```HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs```  


## 7->8  

```$ ls``` 
data.txtがあるのでcatで中身を見る。  
```$ cat data.txt```  
ファイルサイズが大きい。  
more data.txtで中身を見ると、何かしらの単語の隣にパスっぽいのがあることがわかる。  
今回はmillionthの隣にあるので  
```$ grep millionth data.txt```  
grepコマンドでdata.txtの中のmillionthを検索している。  
millionthの横にパスワードがでる。  
```cvX2JJa4CFALtqS87jk27qwqGhBM9plV```  

```|```パイプを使うと  
```$ cat data.txt|grep millionth```  

## 8->9  

テキスト行は重複しており、唯一重複していない行が本物のパスワード。
```uniq```は隣り合った行が同じなら削るという仕様のため、隣あうように先にソートしてする必要がある。  
```sort```したあとに```uniq```コマンドに-uオプション(重複していいない行のみを表示)をつけて終わり。

```$ sort data.tx|uniq -u```  

```UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR```
## 9->10  

```grep "=" data.txt```  

テキストファイルをgrepしたつもりなのに、Binary File と言われてしまったら、オプション -a を付ける。  

-a の代わりに --text や --binary-files=text でも同じ。  
```
grep -a == data.txt
grep -a "=" data.txt
```  
は確認できていない。できると思う。  
stringsコマンドでやる。　　
stringsコマンドは、バイナリファイルやデータファイルの可読部分を表示するコマンドです。　　
バイナリファイルは「テキスト形式ではないファイル」なので、通常はその内容を読むことができません。  

```$ strings data.txt |grep "="```

```truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk```  

次は[10~19](https://github.com/hatsuki0111/Bandit/blob/master/doc/bandit10~19)を書く。  



#CTF練習(コマンド) CTFサークルで使うためにdoc残す。  

https://overthewire.org/wargames/bandit/bandit0.html  

wsl2のUbuntu環境で行っています。  


## 0  
sshで接続する。ssh user@ipaddress ポートは-pで2220  
ssh username@ipaddress ssh接続　方法で調べる  

```$ ssh bandit0@bandit.labs.overthewire.org -p 2220```  

```$ ssh bandit.labs.overthewire.org -p2220 -l bandit0```  
でも接続できる。  


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
これが暗号。次のレベルのパスワードになる。  
```$ exit```  
で抜ける。  


## 1->2  

```$ ssh bandit1@bandit.labs.overthewire.org -p 2220```  

```  
```bandit0@bandit.labs.overthewire.org's password:先ほどの暗号を入力```  

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
```$ exit```  
で抜ける。  

## 2->3  
```$ ssh bandit2@bandit.labs.overthewire.org -p 2220```   
パスワードを入力(省略)  



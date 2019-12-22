# natas  
https://overthewire.org/wargames/natas/  

## 0
ブラウザに以下を入力する。  
http://natas0.natas.labs.overthewire.org/  
username:natas0  
password:natas0  
usernameの数字はレベルごとに置き換える。  
passwordは今後、探したものを使用する。URLも  

右クリックでページのソースを表示する。  
html内にパスワードがあるので取得する。  
```gtVrDuiDfck831PqWsLEZy5gyDz1clto```  

## 0->1  
username:natas1  
http://natas1.natas.labs.overthewire.org/   
先ほどのパスワードを入力する。  

右クリックが禁止されているので、デベロッパーツールを使用する。  
html内にパスワードがあるのでコピーする。  
```ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi```  

## 1->2  
natas2  
右クリックでページのソースを表示  
このページにパスワードはないといわれるが、<img src="files/pixel.png">とある。  
http://natas2.natas.labs.overthewire.org/files  
とURLを変更するとuser.txtがあるのでクリックする。  
パスワードがあるのでコピーする。  
```sJIJNW6ucpu6HPZ1ZAchaDtwd7oGrD14```  

## 2->3  
natas3
robots.txtをURLの最後に入れる。
/s3cr3t/をURLの最後に入れる。  
users.txtがあるのでクリックするとパスワードがあるのでコピーする。  

```Z9tkRkWmpt9Qr7XrR5jWRkgOU901swEZ```

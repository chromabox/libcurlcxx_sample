# libcurlcxx_samp

これは[libcurlcxx](https://github.com/chromabox/libcurlcxx)を外部プロジェクトから使うときのサンプルです。  
CMakeを使った場合の方法になります

## ビルド方法:

まず、このプロジェクトをcloneします。
```bash
$ git clone https://github.com/chromabox/libcurlcxx_sample.git --recursive
```
ついうっかり`--recursive`を忘れてcloneしてしまった場合は以下を行います。  
```bash
$ cd libcurlcxx_sample
$ git submodule update --init --recursive
```
また、ビルドにcmakeを使っているのでcmakeは必須です  
gcc は c++20が通るバージョンが必要になります。  
(c++17でも通るとは思いますがCMakeの設定で20にしているので、c++17でどうしても…な方は`CMakeLists.txt`を適時変更してください)  
  
ubuntu 22.04の場合は次のようになります。(OpenSSL版。こちらを推奨)  
```bash
$ sudo apt install build-essential cmake cpplint libssl-dev 
```
か、もしくは(gnuTLS版)
```bash
$ sudo apt install build-essential cmake cpplint libgnutls28-dev
```
か、あるいは(NSS版)
```bash
$ sudo apt install build-essential cmake cpplint libnss3-dev 
```
を実行してsslの開発用ライブラリを入れて下さい。  
(sslライブラリ系が3つに別れているのはcurl内で使用しているSSL用ライブラリが色々選択可能なためで、基本的に提供される機能に差は無いので好きなのを入れてください…と言いたいところですが、openssl版でしか確認してないのでできればlibssl-devにしてください)  
  
その後
```bash
$ ./build.sh
```
でサンプルがビルドできます。
`build/sample`が引数付きGETリクエストを送るサンプルとなります。  




# libcurlcxx_samp

これは[libcurlcxx](https://github.com/chromabox/libcurlcxx)を外部プロジェクトから使うときのサンプルです。  
CMakeを使った場合の方法になります

## このサンプルのビルド方法:

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

## すでにあるプロジェクトからのlibcurlcxx取り込み方法:

gitの`submodule`を使う方法が楽です。
例では、`external_libs`ディレクトリに外部ライブラリのコードが入るとします。

まず、以下のようにプロジェクトの`CMakeLists.txt`に追記・変更します。
例ではプロジェクト名を `sample` とします。  
```
add_subdirectory(external_libs/libcurlcxx)
.....
target_include_directories(sample PUBLIC ${LIBCURLCXX_INC_DIRS})
.....
target_link_libraries(sample PRIVATE curlcxx )

```
その後、以下コマンドで取り込みます
```
$ cd external_libs
$ git submodule add https://github.com/chromabox/libcurlcxx.git
$ cd libcurlcxx/
$ git checkout v0.1.0
$ cd ..
$ git submodule update --init --recursive
$ git commit -a -m "libcurlcxxを適用"
```
特に、`git submodule update --init --recursive`は必ず行ってください。  
内部でcurlをsubmoduleとしているためです。  
例では`v0.1.1`を取り込んでいますが、必要に応じて適時変更してください。

あとはいつものように`cmake`でビルドを行ってください。

## libcurlcxxのアップデート方法:

libcurlcxxのバージョンアップに追従する場合は以下のようにします。
```
$ git submodule foreach git fetch
$ cd external_libs/libcurlcxx
$ git reset --hard origin/master
$ cd ..
$ git commit -a -m "libcurlcxxを最新に"
```
`origin/master`にすると現状の最新版が反映されます。この部分は適時変更してください。  


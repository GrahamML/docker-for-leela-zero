# docker-for-leela-zero
[Leela-zero](https://github.com/leela-zero/leela-zero)の実行環境を構築するためのdockerfileを提供します。

Leela-zero is :
>A Go program with no human provided knowledge. Using MCTS (but without Monte Carlo playouts) and a deep residual convolutional neural network stack.  
This is a fairly faithful reimplementation of the system described in the Alpha Go Zero paper "Mastering the Game of Go without Human Knowledge". For all intents and purposes, it is an open source AlphaGo Zero.  

_Click [here](https://github.com/GrahamML/docker-for-leela-zero/blob/master/README.md) for the README in English._

# 1. 前提条件  
Leela-zeroの動作要件に対する追加、制約要件は以下の通りです。 
+ Linux x86_64
+ NVIDIA GPU
+ Docker
+ nvidia-docker2 (Docker >= 1.12) or nvidia-container-toolkit (Docker >= 19.03)  

_[Docker](https://github.com/Microsoft/MMdnn/blob/master/docs/InstallDockerCE.md) と [nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-docker#quickstart)のインストール方法に関する情報_

# 2. インストール方法
## 2.1. ダウンロード
本リポジトリをクローンしてください。  
```console
$ git clone https://github.com/GrahamML/docker-for-leela-zero.git
```
## 2.2. Dockerイメージのビルド

```console
$ cd ./docker-for-leela-zero/dockerfile
$ docker build --tag=[image_name:tag] .
```  
+ このdockerfileは [NVIDIA OpenCL 公式dockerイメージ](https://hub.docker.com/r/nvidia/opencl)の上に依存ライブラリをインストールし、leela-zeroのランタイムを生成します
+ 以下はビルドコマンドの一例です  
    ```
    $ docker build --tag=leela-zero:latest . 
    ```
+ 生成されるコンテナ内のUbuntuとOpenCLのバージョンは以下です

    | Ubuntu | OpenCL              |
    |--------|---------------------|
    | 16.04  | OpenCL 1.2 CUDA     |

# 3. 実行方法
## 3.1. Dockerイメージの起動
生成したDockerイメージを以下のように起動してください。  
```console
$ docker run \
    -it \
    --net host \
    --runtime nvidia \
    --entrypoint /bin/bash \
    --name [container_name] \
    [image_name:tag]
```  
+ 上の例は`nvidia-docker2`の場合です。`nvidia-container-toolkit`の場合は適宜変更してください  

## 3.2. Leela-zeroの起動
コンテナ内でleela-zeroを起動してください。以下はtestモードとAutoGTPモードで起動する例です。
### Test モード
```console
$ cd leela-zero/build
$ ./tests
Running main() from gtest_main.cc
[==========] Running 13 tests from 2 test cases.
[----------] Global test environment set-up.
BLAS Core: built-in Eigen 3.3.7 library.
Detecting residual layers...v1...8 channels...1 blocks.
Initializing OpenCL (autodetecting precision).
Detected 1 OpenCL platforms.
Platform version: OpenCL 1.2 CUDA 10.1.152
Platform profile: FULL_PROFILE
Platform name:    NVIDIA CUDA
Platform vendor:  NVIDIA Corporation
Device ID:     0
:
[----------] Global test environment tear-down
[==========] 13 tests from 2 test cases ran. (145254 ms total)
[  PASSED  ] 13 tests.
```
### AutoGTP モード
```console
$ cd leela-zero/build/autogtp
$ ./autogtp
AutoGTP v18
Using 1 game thread(s) per device.
Starting tuning process, please wait...
:
Starting GTP commands sent.
1 (B Q16) 2 (W Q3) 3 (B C16) 4 (W H11) 5 (B D4) 6 (W Q6) 7 (B E16) 8 (W R14) 9 (B C6) 10 (W O17)...
:
412 (W K6) 413 (B pass) 414 (W pass) Game has ended.
Score: B+31.5
Winner: black
```  
+ オプションや起動モードの詳細は [leela-zero's readme](https://github.com/leela-zero/leela-zero/blob/master/README.md) を参照ください

## 3-3. ネットワークの更新  
ネットワーク・パラメータを更新するにはコンテナ内で以下を実行してください。
```console
$ cd leela-zero/build
$ curl -O https://zero.sjeng.org/best-network
```

# 4. Lizzieとの連携  
この[wiki](https://github.com/GrahamML/docker_for_AQ/wiki/Communitacion-with-Lizzie)を参照ください。  

![Select](https://github.com/GrahamML/docker_for_AQ/wiki/images/Communitacion-with-Lizzie/Fig9.png)

# 5. ライセンス  
[MIT](https://github.com/GrahamML/docker_for_leela-zero/blob/master/LICENSE)

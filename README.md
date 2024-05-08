## Dockerfile テンプレート
このリポジトリはDockerfileを共有して、環境構築の時短を願うものです。
参考になれば幸いです。

現在公開しているDockerfileの環境
|name|os|CUDA|Python|pip or conda|Library Type|
|----|----|----|----|:--:|----|
|none|ubuntu20.04|CUDA 11.8|Python 3.10.13|pip|PyTorch 2.0.1|

## Dockerfile の利用方法
`dockerfile`に移動して、`requirements.txt`にPytorch以外の必要なライブラリを記入します。<br>
以下のコマンドでimageを作成します。`-t`で指定するタグ名は任意です。
```
sudo docker build -t mytorch:v1.2-12.2.0-devel-ubuntu20.04 .
```
コンテナを起動して接続する例です。`-p`で指定するポートはTensorBoardで使用する番号を指定してください。<br>
`--shm-size`で指定するサイズは大きい方がDataLoaderのnum_workersを大きく出来るので幸せになれるかもしれません。
```
sudo docker create --name mytorch -it --gpus all -p 127.0.0.1:6006:6006 --shm-size=2G --mount type=bind,src="$(pwd)"/my-repo,dst=/my-repo mytorch:v1.2-12.2.0-devel-ubuntu20.04 bash
sudo docker start mytorch
sudo docker exec -it mytorch bash
sudo docker stop mytorch
```
imageとコンテナを削除する例です。
```
sudo docker stop mytorch
sudo docker rm mytorch
sudo docker rmi mytorch:v1.2-12.2.0-devel-ubuntu20.04
```

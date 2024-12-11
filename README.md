## Dockerfile テンプレート
このリポジトリはDockerfileを共有して、環境構築の時短を願うものです。
参考になれば幸いです。

現在公開しているDockerfileの環境
|name|os|CUDA|Python|pip or conda|Library Type|
|----|----|----|----|:--:|----|
|none|ubuntu22.04|CUDA 12.4|Python 3.10.12|pip|PyTorch:latest 2.5.1|

## Docker Compose を利用する
Docker Composeを利用することで、Dockerfileを直接操作するより簡単に仮想環境を用意できます。<br>
`requirements.txt`にPytorch以外の必要なライブラリを記入します。<br>
`docker-compose.yml`の1行目`&DOCKER_NAME ""`を編集して仮想環境の名前を付けてください。例: `&DOCKER_NAME "xyz-method"` <br>
YAMLのアンカーとエイリアスを使用しているだけなので直接書き換えることもできます。
> [!CAUTION]
> 名前を付けずに進めるとエラーの原因となります。
<details><summary> docker-compose.ymlの内容について </summary>


[公式ドキュメント](https://docs.docker.com/reference/compose-file/services/) で基本的に解決すると思います。<br>
影響が大きいもの<br>
### volumes
```
volumes:
  - type: bind         # 基本的にbindで良いと思います
    source: "./"       # Docker内から参照するディレクトリの場所
    target: "/python"  # Docker内でマウントされる場所、絶対パスのみ
```
複数のディレクトリをマウントすることもできます。
```
volumes:
  - type: bind
    source: "./src"
    target: "/src"
  - type: bind
    source: "./hoge"
    target: "/container_huga"
```
</details>
<br>

`-d`バックグラウンドでコンテナを実行する。 `--build`コンテナを開始前にイメージを構築する。新規なら無くても同じです。
```
docker compose up -d --build
docker compose exec mytorch bash
# コンテナを停止・削除
docker compose down
```

## Dockerfile の利用方法
<details><summary> 非推奨です </summary>

`dockerfile`に移動して、`requirements.txt`にPytorch以外の必要なライブラリを記入します。<br>
以下のコマンドでimageを作成します。`-t`で指定するタグ名は任意です。
```
sudo docker build -t mytorch:v1.2-12.2.0-devel-ubuntu20.04 .
```
コンテナを起動して接続する例です。`-p`で指定するポートはTensorBoardで使用する番号を指定してください。<br>
`--shm-size`で指定するサイズは大きい方がDataLoaderのnum_workersを大きく出来るので幸せになれるかもしれません。<br>
`--mount`でディレクトリを指定するとDocker内から参照できます。`/my-repo`を変更して使ってください。
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
</details>

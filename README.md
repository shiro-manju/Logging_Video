# Gstremer_Logging_Video
docker上で操作を確認

## Docker　環境
- Ubuntu 18.04
- Python 3.7.3

## Docker　環境構築手順
- Docker イメージファイルをpull <br>
  - `$ docker pull nethacker/ubuntu-18-04-python-3`

- Docker コンテナを作成 <br>
  - `$ docker run -d -p 6080:80 nethacker/ubuntu-18-04-python-3:latest`
- Docker コンテナIDを確認 <br>
  - `$ docker ps`

```console
CONTAINER ID   IMAGE                                    COMMAND             CREATED         STATUS         PORTS     NAMES
7bcd7794c484   nethacker/ubuntu-18-04-python-3:latest   "/bin/sh -c bash"   2 minutes ago   Up 2 minutes             magical_gould
```

- Docker コンテナ起動 <br>
  - `$ docker start 7bcd7794c484`
  - `$ docker attach 7bcd7794c484`

- Docker コンテナ停止 <br>

```console
root@7bcd7794c484:/# exit
exit
```


## Gstremerのインストール
- `$ apt update`
- `$ apt install git`
- `$ git clone https://github.com/shiro-manju/Logging_Video.git`


- Gstreamer関係のパッケージをinstall
```console
$ apt install libgstreamer1.0-0 gstreamer1.0-tools
$ apt install -y gstreamer1.0-alsa gstreamer1.0-plugins-bad gstreamer1.0-plugins-base-dbg gstreamer1.0-plugins-ugly gstreamer1.0-rtsp gstreamer1.0-x \
gstreamer1.0-clutter-3.0 gstreamer1.0-libav gstreamer1.0-plugins-bad-dbg gstreamer1.0-plugins-base-doc gstreamer1.0-plugins-ugly-dbg gstreamer1.0-rtsp-dbg \
gstreamer1.0-crystalhd gstreamer1.0-libav-dbg gstreamer1.0-plugins-bad-doc gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly-doc gstreamer1.0-tools \
gstreamer1.0-doc gstreamer1.0-nice gstreamer1.0-plugins-base gstreamer1.0-plugins-good-dbg gstreamer1.0-pocketsphinx \
gstreamer1.0-espeak gstreamer1.0-packagekit gstreamer1.0-plugins-base-apps gstreamer1.0-plugins-good-doc gstreamer1.0-pulseaudio
```

- webカメラ表示用のパッケージ
  - `$ apt install v4l-utils`

## Webカメラの表示と保存

表示

```console
$ gst-launch-1.0 v4l2src ! videoconvert ! ximagesink
```

保存

```console
$ gst-launch-1.0 v4l2src num-buffers=500 \
! queue \
! x264enc \
! mp4mux \
! filesink location=video.mp4
```

## その他

### カメラ接続（VMware）

VMwareでカメラが接続できない場合は、接続設定を行った上で再起動を行うと接続できた。（はじめから接続設定がON状態の状態でVMwareを起動する。）

### 参考URL

- [http://littlewing.hatenablog.com/entry/2016/02/24/200129](http://littlewing.hatenablog.com/entry/2016/02/24/200129)


## PythonからGStreamerを呼び出す

動的にパイプを組み替えたい（逆走検知時に別に動画を保存したい）ために、gst-launchを直接使うのではなくPythonのコードからGStreamerを使用したい。

### インストール

```console
$ sudo apt install -y libgirepository1.0-dev
$ pip install pygobject
```

### サンプルコード実行

```console
$ python hello.py
```

### USB画像層をMP4に保存するコード

```console
$ python usbcam_save.py
```

### その他

#### 参考URL

- [http://brettviren.github.io/pygst-tutorial-org/pygst-tutorial.html](http://brettviren.github.io/pygst-tutorial-org/pygst-tutorial.html)  
- [https://lazka.github.io/pgi-docs/Gst-1.0/classes.html](https://lazka.github.io/pgi-docs/Gst-1.0/classes.html)

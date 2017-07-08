# UbuntuデスクトップDockerイメージ

このプロジェクトは、日本人向けのUbuntuデスクトップ環境を、Dockerコンテナとして提供することを目的とします。  
Ubuntuデスクトップ環境を、ブラウザ上で利用することが出来ます。

## デスクトップ環境の構成

- xvfbによる仮想ディスプレイ
- xfceによる軽量デスクトップ環境
- x11vncによるリモートデスクトップ(ポートはコンテナ外に非公開)
- noVNCによるブラウザによるリモートデスクトップ

## 日本人向け対応

- 日本語言語パックのプレインストール
- 日本語入力(Anthy)のプレインストール
- 日本のタイムゾーン設定

## 使い方

### お試し実行

まずはお試しで実行してみます。

```sh
$ docker run --rm -p 8080:8080 uphy/ubuntu-desktop-jp:16.04
```

コンテナが立ち上がったら、ブラウザで[http://localhost:8080](http://localhost:8080)にアクセスしてください。
以下のような画面が表示されたら成功です。  
![novnc](https://raw.githubusercontent.com/uphy/ubuntu-desktop-jp/images/novnc.png)

Connectをクリックすると、デスクトップが利用できます。  
![desktop](https://raw.githubusercontent.com/uphy/ubuntu-desktop-jp/images/desktop.png)

※  
環境によってはディスプレイサイズの違いから、全体が表示できないかもしれません。  
左側のバーをクリックして、「Full Screen」ボタンをクリックすると、画面サイズに合わせて拡大・縮小表示してくれます。  
サーバー側の解像度を変えたい場合には、/etc/supervisord/conf.d/desktop.confの以下の行を環境に合わせて変更して下さい。

```
[program:X11]
command=/usr/bin/Xvfb :0 -screen 0 1280x800x16
```

### データの永続化について

このイメージは、特にVOLUME設定を行っていないため、デフォルトではコンテナを再構築するとデスクトップに対して行った設定等はクリアされます。  
必要に応じて設定を永続化してください。

例:

```
$ docker run \
    -v "$(pwd)/data/config:/root/.config" \
    -v "$(pwd)/data/Desktop:/root/Desktop" \
    -p 8080:8080 \
    uphy/ubuntu-desktop-jp:16.04
```
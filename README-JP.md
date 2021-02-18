[English](https://github.com/aromajoin/controller-http-api) / [日本語](README-JP.md)
# アロマシューター制御用HTTP APIs
[![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/aromajoin-software/)

アロマジョインのHTTP APIは「ASN2」から始まるアロマシューターに対して使えます。このAPIを使うにはWi-Fiネットワークが必要です。


## I. Wi-Fi 接続設定

アロマシュータをWi-Fiネットワークに接続する方法として2つあります。それはアロマジョインの純正iOSアプリを使う方法とウェブブラウザを使う方法です。


### 方法１- アロマシューターiOSアプリを使う (お勧め)

アロマシューターiOSアプリはアップルのApp Storeに公開されています。

https://apps.apple.com/app/aroma-shooter/id1477144583

アプリをインストールした後、アプリを立ち上げて、画面の左下にある「Settings」ボタンを押して、「Others」→「Setup AromaShooter's Wi-Fi」で画面上の案内通り進めます。「Aroma Shooter Wi-Fi Connection」という画面で接続したいWi-Fiネットワークを選んで、ユーザー名とパスワードなど入力して接続します。成功したというメッセージが出たら「Done」ボタンを押して設定メニューに戻ります。


### 方法２- ウェブブラウザを使う

iOSデバイスを持っていない方に別な方法です。

アロマシューター立ち上げて、パソコンやスマホなどのWi-Fi設定画面からアロマシューターが作ったWi-Fiネットワークを選んで接続します。そのWi-FiネットワークのSSIDはアロマシューターのシリアル番号のままで、例えば「ASN2A5216」です。

- 接続するためのパスワード：aromajoin@1003

そのパソコンもしくはスマホのウェブブラウザ、例えばSafariやGoogle Chromeなど、で「 http://192.168.1.1/index.html 」を開きます。発見できるWi-Fiネットワーク一覧が出てきて、その中から接続したいネットワークを選んで、必要に応じてユーザー名やパスワードなどを入力して、接続します。30秒程度（スマホを使った方が速いと体験しています）で接続ができます。尚、成功したメッセージが出るまで待って、そのページから出ない必要があります。終わったら、そのパソコンやスマホなどを他のネットワークに接続しても問題ありません。ここでアロマシューターを制御するコマンドを送れるようになりました。


## II. APIs

アロマシューターが接続しているWi-Fiネットワークと同じネットワークに接続すれば、パソコンやスマホ等からそのアロマシューターに対する制御コマンドを送れます。次の形のRESTリクエストを送って制御します。

**ホスト名:** `http://[アロマシューターのIPアドレス]` 　もしくは 　`http://[アロマシューターのシリアル番号].local`

**ポート:** 1003

ホスト名は上記形式のいずれかにする必要があります。【アロマシューターのIPアドレス】と【アロマシューターのシリアル番号】のところには本当のIPアドレスとシリアル番号を置き換えて利用してください。下記の例を参考してください。

- IPアドレス: http://192.168.1.10:1003 (こちらの形式は処理が速いので **お勧め**です。)

- シリアル番号: http://ASN2A00001.local:1003 (こちらの形式は直観的ですが、遅い方です。そして、Android装置なら使用できない場合があります。)


### リクエスト例:


1. **ファームウェア情報取得**

*Path:* /as2/firmware

*Method:* GET

*Header:* “Content-Type: application/json”

*レスポンス例:*

```javascript
{

"current": "1.0.0",

"latest": "1.0.1",

"internet": "true"

}
```


2. **噴射**

*Path:* /as2/diffuse

*Method:* POST

*Header:* “Content-Type: application/json”

*リクエスト中身:*


```javascript
  // For the latest firmware version
  {
    "channels": [number, ...], // The cartridge number. Range: 1 ~ 6
    "intensities": [number, ...], // The cartridge intensity as a percentage. Range: 0 ~ 100
    "durations": [number, ...], // Diffusion time in milliseconds. Range: 0 ~ 10000
    "booster": boolean // Set to true to activate the Aroma Shooter's booster fan. Default value is false.
  }

  // For the firmware lower than v2.0.0
  {
    "duration": Int, // ミリ秒単位の噴射時間、値レンジ：0 ~ 10000。
    "channel": Int, // カートリッジ番号、値レンジ: 1 ~ 6。
    "intensity": Int, // パーセンテージでカートリッジの強度、値レンジ: 0 ~ 100。
    "booster": Boolean // 真（True）に設定したら、アロマシューターのブースタファン（無臭ファン）が有効になります。デフォルトは偽（False）です。
  }
```

例えば:


```javascript
  // For the latest firmware version
  {
      "channels": [1,3,5],
      "intensities": [100,50,25],
      "durations": [1000,2000,3000],
      "booster": true
  }

  // For the firmware lower than v2.0.0
  {
    "duration": 3000, // アロマシューターが3秒で噴射します。
    "channel": 3, // 3番カートリッジが噴射します。
    "intensity": 100, // 100％の強度で噴射します。
    "booster": true // 無臭のブースタのファンが有効になります。
  }
```

*レスポンス例:*


```javascript
{

"status": "done"

}

```


3. **噴射停止**

*Path:* /as2/stop_all

*Method:* POST

*Header:* “Content-Type: application/json”

*Request body:* None

*レスポンス例:*


```javascript
{

"status": "done"

}
```

----------
Copyright 2020 Aromajoin Corporation under [CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/) license.

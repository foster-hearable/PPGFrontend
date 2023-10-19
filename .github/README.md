# Hearable PPGFrontend
---
## Overview
Hearable PPGFrontendはヒアラブルデバイス（フォスター電機 RN002）に実装されているハートレート機能のフロントエンドアプリケーションです。（デモ用Webサイト [https://foster-hearable.github.io/PPGFrontend/](https://foster-hearable.github.io/PPGFrontend/)）

WebBluetoothを用いてヒアラブルデバイスRN002からデータ取得を行い、ハートレート情報の表示やWebSocketサーバーへのデータ送出を行います。\
WebSocketのデータは加速度脈波のRAWデータで構成されているため、WebSocketサーバー側での加工や解析に用いることができます。

## 対応ブラウザ
WebBluetoothに対応したブラウザ　参考：[ブラウザー互換性一覧表 Mozilla.org](https://developer.mozilla.org/ja/docs/Web/API/Web_Bluetooth_API#ブラウザーの互換性)

#### 動作することを確認しているブラウザ
- Chrome（Windows,Mac）
- Edge（Windows,Mac）
  
#### 動作しないことを確認しているブラウザ
- Safari（Mac,iOS）



## 操作インターフェース
<img src="Panel.png" width="450">

#### ① Object
ヒアラブルデバイスより送られてくるデータに合わせて回転します。\
接続直後や大きく回転した直後にはフィルタによる補正が動作し、ヒアラブルデバイスが動いていない場合であっても適切な位置まで徐々に回転することがあります。

#### ② Controls
- Start Notify：クリックするとヒアラブルデバイスとの接続ウィンドウが表示されます（接続後、データが送られてくるまで20秒程度かかる場合があります）
- Stop Notify：ヒアラブルデバイスとの接続を解除する場合にクリックします。再度接続したい場合にはブラウザをリロードしてください。
- Reposition：Objectの向きを正面に戻します。ヒアラブルデバイスの実際の向きとObjectの向きを合わせる場合にクリックしてください。

#### ③ WebSocket
- WebSocket URL：WebSocketで接続するURLを指定してください。
- Connect：WebSocket URLとの接続/切断を行います。本ページがロードされた直後に自動的に接続を試みます。
  
#### ④ Status
- 接続しているデバイス名や受信したデータの種別やデータ数を表示します

## WebSocketでの拡張
WebSocketで送られてきたヒアラブルデバイスの回転データを各製品のAPIに渡すことにより、任意のアバターやロボットなどの制御を行うことが可能になります。

<img src="Expand.png" width="450">

#### WebSocketデータフォーマット
コンマ区切りで下記順序のデータ列を送出します。

|   | データ種別 | フォーマット |
|-|-|-|
| 1 | ステータス | bit7-2:使用せず、bit1:タッチ操作(1:ON、0:OFF)、bit0:装着(1:ON、0:OFF) |
| 2 | W成分 | Float値によるQuaternionのW成分データ |
| 3 | X成分 | Float値によるQuaternionのX成分データ |
| 4 | Y成分 | Float値によるQuaternionのY成分データ |
| 5 | Z成分 | Float値によるQuaternionのZ成分データ |


## 注意事項
- ヒアラブルデバイスに接続してからオブジェクトが回転するまで最大で20秒程度の時間がかかる場合があります。
- 左右のヒアラブルデバイスを使用している場合、状況に応じて自動的にどちらか片側のヒアラブルデバイスからのセンサー情報を取得します。\
  左右の任意のデバイスからデータ取得を行いたい場合は、使用しないヒアラブルデバイスをチャージケースに収納してからヒアラブルデバイスに接続してください。
  
※ワイヤレス通信は周囲の状況によりデータ欠落や再送・遅延が発生します。センサーデータはこれらの事象を考慮したうえでご利用ください。\
※このアプリケーションはヒアラブルデバイス（フォスター電機 RN002）の評価を目的として[MITライセンス](https://github.com/foster-hearable/HeadTracker/blob/e59c1e2fe2de506fb53649f6b3cb550f1e6ca852/LICENSE.txt)の下に公開しております。

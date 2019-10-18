# 【iOS】プッシュ通知の受信に必要な証明書の作り方(開発用)
*2016/09/28作成(2019/10/15更新)*
<br><br>
アプリにプッシュ通知を組み込む際、必要な証明書の作り方を解説します。

<center><img src="readme-img/001.png" alt="画像1" width="500px"></center>

## 事前準備
* Apple Developer Program アカウント（有料）が必要です<br> https://developer.apple.com/account/
* Mac と以下の環境が必要です
  * 秘密鍵書き出しのため、 __キーチェーンアクセス__ を利用します
  * 端末のUDID確認のため、__iTunes__ または __Xcode__ を使用します

<div style="page-break-before:always"></div>

## 作業手順

* 下図の内容を①〜⑦の順番でそれぞれ作成していきます

<center><img src="readme-img/i002.png" alt="画像i2" width="450px"></center>

* __【Monaca利用時のみ】__：上記に加え、キーチェーンアクセスにて②開発用証明書(.cer)に紐付く「__⑧開発用証明書(.p12)__」を書き出す必要があります
* 作成したファイルはダウンロードし、同じフォルダにまとめておきましょう
  * デスクトップなどにフォルダを用意すると便利です
* 作業は少し複雑です。 __作り方を誤るとプッシュ通知が動作しない可能性があります__ ので、１つ１つ丁寧に作業していきましょう！

### ①「CSRファイル」の作成
* CSRファイルを作成します　__※初回利用時のみ__
  * 既にある場合は作成不要です
* 「キーチェーンアクセス」を開いて、メニューバーの「キーチェーンアクセス」＞「証明書アシスタント」＞をクリックします

<center><img src="readme-img/i003.png" alt="画像i3" width="450px"></center>

* 「鍵ペア情報」を確認して「続ける」をクリックすると、「設定結果」が出るので「完了」をクリックします
* 保存場所を選択して「保存」をクリックします

<center><img src="readme-img/i004.png" alt="画像i4" width="100px"></center>

### ②「開発用証明書(.cer)」の作成


左メニューのCertificatesを開き、Certificatesの隣にある+ボタンをクリックすると、
Create a New Certificateの画面が開きます。

<img src="readme-img/setupapns03.png" width="90%" alt="Member Centerを開く">

証明書の作成画面が表示されるので、下の方にあるServicesの項目にある必要なAPNs証明書を選択してください。

 - iOS Apple Push Notification service SSL (Sandbox)：開発用のプッシュ通知証明書
 - Apple Push Notification service SSL (Sandbox & Production)：本番用のプッシュ通知証明書

なお、リリースされるアプリ(本番用アプリ)に対して開発用証明書を設定した場合、invalidTokenが発生してしまいますのでご注意ください。
※invalidTokenについては[こちら](https://lp.mbaas.nifcloud.com/mb_push-trouble-shooting_thanks.html)

- 開発用：Apple Push Notification service SSL (Sandbox) を選択

<img src="readme-img/setupapns04_dev.png" width="90%" alt="開発用証明書を選択">

- 本番用：Apple Push Notification service SSL (Sandbox & Production) を選択
 - 本来は開発用と本番用を兼ねた証明書ですが、ニフクラ mobile backendに設定する場合は本番用として設定してください。

<img src="readme-img/setupapns04_pro.png" width="90%" alt="本番用証明書を選択">

どのアプリに紐づいた証明書を作成するのか選択する必要があります。
下のプルダウンメニューにAppleに登録されたアプリの一覧が表示されるので、
証明書を作成するアプリを選択してください。

<img src="readme-img/setupapns05.png" width="90%" alt="Member Centerを開く">

次の画面では、CSRファイルを選択する必要がありますので、CSRファイルについて説明して行きます。

CSRファイルは、開発者証明書を登録する際に既に作成済みの場合、再度作成する必要がありません。
まだ作成されていない場合、以下の手順でキーチェーンアクセスから作成します。
キーチェーンアクセスのメニューから証明書アシスタント＞認証局に証明書を要求...を選択します。

<img src="readme-img/setupapns08.png" width="90%" alt="Member Centerを開く">

メールアドレスと通称を設定し、ディスクに保存を選択して、続けるボタンをクリックします。
CSRファイルの保存場所を決めて、保存してください。

<img src="readme-img/setupapns09.png" width="90%" alt="Member Centerを開く">

証明書作成画面に戻り、CSRファイルをアップロードしてください。

<img src="readme-img/setupapns06.png" width="90%" alt="Member Centerを開く">

### ③「AppID」の作成
* AppID を作成します
* 「Identifiers」＞「App IDs」＞右上の「＋」をクリックします

<center><img src="readme-img/i009.png" alt="画像i9" width="400px"></center>
<center><img src="readme-img/i010.png" alt="画像i10" width="450px"></center>
<center><img src="readme-img/i011.png" alt="画像i11" width="400px"></center>
<center><img src="readme-img/i012.png" alt="画像i12" width="400px"></center>

### ④「端末の登録」
* 動作確認で使用する端末の登録をします
  * 既に登録済みの場合、この作業は不要です
* 「Devices」＞「All」＞右上の「＋」をクリックします

<center><img src="readme-img/i013.png" alt="画像i13" width="400px"></center>

* UDIDは下記のいずれかの方法で調べることができます

<center><img src="readme-img/i014.png" alt="画像i14" width="500px"></center>
<center><img src="readme-img/i015.png" alt="画像i15" width="500px"></center>

* 先ほどの入力欄に調べたUDIDをコピーして貼り付け、「Continue」をクリックします

<center><img src="readme-img/i016.png" alt="画像i16" width="400px"></center>

<div style="page-break-before:always"></div>

### ⑤「プロビジョニングプロファイル」の作成
* プロビジョニングプロファイルを作成します
* 「Provisioning Profiles」＞「All」＞右上の「＋」をクリックします

<center><img src="readme-img/i017.png" alt="画像i17" width="450px"></center>
<center><img src="readme-img/i018.png" alt="画像i18" width="450px"></center>
<center><img src="readme-img/i019.png" alt="画像i19" width="300px"></center>
<center><img src="readme-img/i020.png" alt="画像i20" width="400px"></center>

### ⑥「APNs用証明書(.cer)s」の作成
* APNs用証明書(.cer)を作成します
  * ここで「CSRファイル」が必要となります。①「CSRファイル」が手元にない場合は、改めて「CSRファイル」を作成し、準備してください。（作成手順は、『①「CSRファイル」の作成』を参照）
* 「Certificates」＞「All」＞右上の「＋」をクリックします

<center><img src="readme-img/i021.png" alt="画像i21" width="300px"></center>
<center><img src="readme-img/i022.png" alt="画像i22" width="400px"></center>

* CSRファイルは①開発用証明書(.cer)作成時と同じものを使用します

<center><img src="readme-img/i023.png" alt="画像i23" width="400px"></center>
<center><img src="readme-img/i024.png" alt="画像i24" width="300px"></center>

### ⑦「APNs用証明書(.p12)」の作成
* APNs用証明書(.p12)を書き出します
* キーチェーンアクセスを開きます
* ⑥で作成した「APNs用証明書(.cer)」をダブルクリックしてキーチェーンアクセスを開きます
* 三角のアイコンをクリックして、開きます
  * 重要：下図の作業を誤るとプッシュ通知を正しく配信することができなくなりますので注意して書き出してください

  <center><img src="readme-img/i027.png" alt="画像i27" width="400px"></center>
  <center><img src="readme-img/i028.png" alt="画像i28" width="500px"></center>

### 【Monaca利用時のみ】<br>⑧「開発用証明書(.p12)」の作成
* 開発用証明書(.p12)を書き出します
* キーチェーンアクセスを開きます
* ②で作成した「開発用証明書(.cer)」をダブルクリックしてキーチェーンアクセスを開きます
* 証明書を開かずに（三角のアイコンをクリックしないで）開発用証明書で右クリックをして書き出します

<center><img src="readme-img/i029.png" alt="画像i29" width="300px"></center>
<center><img src="readme-img/i030.png" alt="画像i30" width="400px"></center>

### プッシュ通知の実装に使用するファイルの確認
* 作成したものの中で、以下の３点を使用します
  * ②開発用証明書(.cer)
    * Monaca利用時は、⑧開発用証明書(.p12)
  * ③AppID
  * ⑤プロビジョニングプロファイル
  * ⑦APNs用証明書(.p12)

## 参考
* Swift3(iOS10以上)用プッシュ通知サンプル<br>https://github.com/NIFTYCloud-mbaas/Swift3PushApp
* Swift2(iOS8,9)用プッシュ通知サンプル<br>https://github.com/NIFTYCloud-mbaas/SwiftPushApp
* ObjectiveC(iOS10以上)用プッシュ通知サンプル<br>https://github.com/NIFTYCloud-mbaas/ObjcPushApp_ios10
* ObjectiveC(iOS8,9)用プッシュ通知サンプル<br>https://github.com/NIFTYCloud-mbaas/ObjcPushApp
* Monacaプッシュ通知サンプル<br>https://github.com/NIFTYCloud-mbaas/MonacaPushApp
* Unityプッシュ通知サンプル<br>https://github.com/NIFTYCloud-mbaas/unity_push_quickstart

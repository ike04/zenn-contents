---
title: "Google Play StoreにAndroidアプリをリリースする手順2023（令和最新版）"
emoji: "🐕"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Android]
published: false
published_at: 2023-12-11 08:00
---
:::message
この記事は [ZOZO Advent Calendar 2023](https://qiita.com/advent-calendar/2023/zozo) カレンダーのシリーズ3の11日目の記事です。
:::

## はじめに

プライベートでAndroidアプリを作ることはあっても、Play Storeにリリースしたことがなかったので、2023年の11月にリリースをしてみました。当初はアプリといくつかのスクショと説明文があればリリースできると思っていたのですが、リリースするために必要なものや準備するものが思っていたより多くリリースするのに時間がかかってしまったのでその備忘録になります。

## Play Storeへのリリースステップ

1. Googleへのdeveloper登録をする
2. Androidアプリを作る（aabファイルを作る）
3. Google Play Consoleで各種設定をする
4. アプリを審査に出す


## 1. Googleへのdeveloper登録をする

アプリをストアで公開するためにGoogleへ[developer登録](https://developer.android.com/distribute?hl=ja)を行います。developer登録に必要なものは以下のとおりです。

- Googleアカウント
- クレジットカード
- 本人確認に用いる身分証
- $25

アプリを公開するとメールアドレスも公開されるケースがあるのでそれが気になる方は、新しくアカウントを作成するのがおすすめです。

また、本人確認完了までに少し時間がかかるため、事前に登録しておいた方がスムーズにアプリをリリースできます（私は1日半かかりました）

Play Store ConsoleのUIが変わってしまっていますがこちらの記事がわかりやすかったです

https://qiita.com/QRES/items/e236d63689d003e7af67

## 2.Androidアプリを作る（aabファイルを作る）
次にPlay Consoleにアップロードするために作成したアプリのaabファイルを生成します。

1. 「Build」から「Generate Signed Bundle」を選択します
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/99576773-0c84-a190-ce2f-2b878a62046d.png)

2. 「Android App Bundle」を選択する
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/8b087e54-0919-f67f-c921-4ea01dd2eda7.png)

Android App BundleとAPKの違いは以下の通りです
> App Bundle は公開のみを目的としており、Android デバイスにインストールすることはできません。Android パッケージ（APK）は、Android 用のインストール可能かつ実行可能なアプリ形式です。

https://developer.android.com/guide/app-bundle/faq?hl=ja#whats_the_difference_between_aabs_and_apks

3. Create New Keyから鍵を生成する
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/bee2ab8e-00ad-b131-1e22-6dced76b2ff5.png)
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/1f1f5538-e3a4-b71e-d437-0e0f69d55319.png)
    - key store path
        - keyを保存するパス
    - alies
        - デフォルトの`key0`のままでOK
    - Validity
        - keyの有効期限でデフォルトが25年

4. Build Variantsを選択する
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/89d9b6d7-a3fd-9a57-135c-7ae218f45b6d.png)

## 3. Google Play Consoleで各種設定をする

ここからは1で登録したPlay Consoleの設定を行います。アプリを公開するためにこれらのセットアップを完了する必要があります。
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/510fa550-f516-966f-1326-725e3d459ee4.png)

中でもプライバシーポリシーは13歳未満の子どもがターゲットユーザーに含まれている場合は必須で、URLを準備する必要があり大変でした。私は、プライバシーポリシーのWebページを作る余裕がなかったので[App *Privacy Policy* Generator](https://app-privacy-policy-generator.firebaseapp.com/)を利用させていただきました。

次にGoogle Play Storeに掲載するアプリの情報を設定します。設定が必要な項目と準備するものが多く個人的にはここが一番大変でした。

- アプリ名(30文字以内)
- アプリの簡単な説明(80文字以内)（赤枠部分）
    - 検索結果ページおよび商品ページの「このアプリについて」の部分に表示される文のこと
- アプリの詳しい説明(4000文字以内)（緑枠部分）
    - アプリの詳細画面の「このアプリについて」がタップされた時に展開される文章のこと


![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/b1cd9938-f186-fbad-2037-d180a5a005b1.png)

- アイコン
    
    ```
    ・枚数：1枚
    ・形式：.png または .jpeg
    ・各辺：512 x 512 px
    ・容量：最大 1 MB
    ```


- フィーチャーグラフィック(1024×500)
    - Google Play Store全体での広告部分や、類似アプリのページを開いているユーザーに「その他のアプリ」として表示する際の宣伝画像
    - [情報を伝えるフィーチャー グラフィックでアプリをアピールする](https://developer.android.com/distribute/best-practices/launch/feature-graphic?hl=ja)
    - 私は[canva](https://www.canva.com/ja_jp/)を使って作成しました
    
    ```
    ・枚数：1枚
    ・形式：.png または .jpeg
    ・各辺：1024 x 500 px
    ・容量：最大 1 MB
    ```

- スクリーンショット
    - Play Storeの検索結果や詳細画面に表示されるアプリの画像をアップロードする必要があります
    - スマートフォン/タブレット/Chromebookでそれぞれ別の画像を設定することができます
    - ちゃんとしたデザインの画像を生成できるツールをいくつか利用したので参考にしてみてください
        - https://studio.app-mockup.com/
        - https://theapplaunchpad.com/
    
    ```
    ・枚数：2 ~ 8枚
    ・形式：.png または .jpeg
    ・アスペクト比：16:9 または 9:16
    ・各辺：320 ~ 3840 px
    ・容量：スクリーンショット1枚につき最大 8 MB
    ```
    

## 4. アプリを審査に出す
上記のストア設定が完了したら、アプリを公開することができます。

1. Play Consoleの「製品版」タブを選択し「新しいリソースを作成」します
![スクリーンショット 2023-12-06 14.02.53.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/b6999ad5-9d1e-2fd6-a8ab-b0e9f074a60e.png)

2. App Bundleに作成したaabファイルをアップロードし、リリースノートを記入します。その後、右下の「次へ」ボタンを押下します
![スクリーンショット 2023-12-06 14.04.28.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/12a0c103-568f-c79d-7f62-92770d58f9ee.png)

初回のリリースは審査の時間が長く、大体3日くらいかかりました。

## まとめ

最後におさらいでAndroidアプリをPlay Storeにリリースするには以下のものを準備する必要があります。

- developer登録するために必要なもの
    - Googleアカウント
    - クレジットカード
    - 本人確認に用いる身分証
    - $25
- アプリを審査に出すために必要なもの
    - aabファイル
    - アプリ名
    - アプリの説明文
    - アイコン画像
    - フィーチャーグラフィック
    - スクリーンショット
    - プライバシーポリシーのURL

今回はAndroidアプリをリリースする手順とそれに必要なものをまとめてみました。リリースする際の参考にしていただければと思います！

## 宣伝

今回リリースしたアプリはこちらです！

https://play.google.com/store/apps/details?id=com.ike04.fixedcost&hl=ja

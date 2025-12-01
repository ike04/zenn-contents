---
title: "Flutter 3.38からIDEサポートされたFlutter Widget Previewerを試してみた！"
emoji: "📌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Flutter,Dart]
published: true
---
[Flutter 3.38](https://docs.flutter.dev/release/release-notes/release-notes-3.38.0)からIDEサポートされた[Widget Preview機能](https://docs.flutter.dev/tools/widget-previewer)を実際に触って色々試してみました。
AndroidのJetpack Composeにある`@Preview`がとても便利だったのでFlutterで使えるようになるの楽しみにしてました👏

:::message
experimentalの機能のため、今後修正が入る可能性があります
>Please be aware that this is an experimental feature available in the Flutter stable channel. The APIs are not stable and will change. This guide is for the current early access version, and you should expect future updates to introduce breaking changes.
:::

## Flutter Widget Previewerとは
実装中のコードのウィジェットがリアルタイムでIDE上にレンダリングされて表示内容の確認ができる機能です。
AndroidのJetpack Composeにある`@Preview`と[同様の機能](https://developer.android.com/develop/ui/compose/tooling/previews?hl=ja)でコンポーネント単位でUIの確認ができるようになるので実装効率の向上が期待できます。

## 検証環境
```
Flutter 3.38.0
Dart SDK version: 3.10.0-290.4.beta
VSCode: Version: 1.105.1 
```

## プレビューを表示してみる
公式ドキュメントを参照にプレビューを表示してみます。
https://docs.flutter.dev/tools/widget-previewer

ファイルを追加しサンプルコードを実装し、Flutter Widget Previewタブを選択するとプレビューが表示されました。
最初、私の環境ではFlutter Widget Previewタブが表示されていなかったので、VSCodeのアップデートが必要でした

```dart
import 'package:flutter/widget_previews.dart';
import 'package:flutter/material.dart'; // For Material widgets

@Preview(name: 'My Sample Text')
Widget mySampleText() {
  return const Text('Hello, World!');
}
```

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/41151751-bdb2-4069-862b-dd90c0d4d62e.png)

ウィジェットの中身を変更せず、プレビューの表示を変更することも可能です
- name: プレビューの説明的な名前
- group: ウィジェット プレビューアーで関連するプレビューをグループ化するために使用される名前
- size: オブジェクトを使用した人工的なサイズ制約 Size
- textScaleFactor: カスタム フォント スケール
- wrapper: プレビューされたウィジェットを特定のウィジェット ツリーにラップする関数 
- theme: Material および Cupertino テーマ設定データを提供する機能
- brightness: テーマの初期の明るさ
- localizations: ローカリゼーション設定を適用する機能


同じグループにまとめることで見やすく表示できる👏
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/94e3c68f-30d6-4372-92c1-50f3d921ea4f.png)

また、1つのウィジェットに対して複数の`@Preview`を付与することができるのでフォントサイズによるデザイン崩れの確認も容易にできそうです

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/a6d8c777-b416-4c2e-808d-9d2fcf18cc7c.png)


## @Preview アノテーションを自作する
複数のプレビューをまとめて1つのアノテーションで表示することも可能です。
ここでは3パターンのフォントサイズを1つのアノテーションで表示するカスタムプレビューを実装してみました。

```dart
final class MultiTextScaleFactorPreview extends MultiPreview {
  const MultiTextScaleFactorPreview();

  @override
  List<Preview> get previews => const [
        Preview(
          group: 'TextScaleFactor',
          name: 'Example - 1.0',
          textScaleFactor: 1.0
        ),
        Preview(
          group: 'TextScaleFactor',
          name: 'Example - 2.0',
          textScaleFactor: 2.0
        ),
         Preview(
          group: 'TextScaleFactor',
          name: 'Example - 0.5',
          textScaleFactor: 0.5
        ),
      ];
}
```

プレビューを表示したいWidgetに対して上で実装した`@MultiTextScaleFactorPreview`を付与するだけで3パターンのプレビューが表示されます。

```dart
@MultiTextScaleFactorPreview()
Widget mySampleButton () {
  return FilledButton(onPressed: (){}, child: const Text('Tap!!'));
}
```

![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/7d7fcbc1-733a-4a0b-bc8a-6b3e710ab46a.png)

## 色々触ってみた
**プレビューでのインタラクティブな操作**
TextFieldの入力確認はできる
![ezgif-38e2f51a891804e1.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/ccf14dd2-6ed2-494e-833f-8ea7f4903997.gif)

Switchの切り替えもできる
![ezgif-1cde19f0dc74cd10.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/f1e3f908-55f3-4192-b3ce-3f8336405a16.gif)

**ローカルの画像の表示も問題なし**
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/952598b2-7377-4858-8343-352f56af1099.png)

**Network経由で画像を取得して表示するのはできなさそう**
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/484337/7293897a-e832-4866-b9db-df3bfe567067.png)

**プレビューレンダリングのコピー**
Jetpack Composeではプレビューを画像としてコピーできましたがFlutterではできなさそうでした
https://developer.android.com/develop/ui/compose/tooling/previews?hl=ja#preview-copy-render

## まとめ
Flutter Widget Previewerの導入により、コンポーネント単位でのUI実装効率の向上が見込めそうです。Jetpack Composeにある[@Preview と大規模なデータセット](https://developer.android.com/develop/ui/compose/tooling/previews?hl=ja#preview-data)のような引数を渡せる機能が公式ドキュメントに見当たらなかったので今後追加されるのを期待したいです

今回のサンプルリポジトリ
https://github.com/ike04/flutter_preview_learning

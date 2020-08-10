# cloud functions のテスト


## はじめに

下記のスタートガイドを元に実施

https://firebase.google.com/docs/functions/get-started

## 環境設定

1. [firebaseコンソール](https://console.firebase.google.com/)から新規プロジェクトを作成

2. ツールのインストール及び設定

```
$ npm install -g firebase-tools
//shellの再起動
$ firebase -V
//コマンドインストール確認
$ firebase login
$ firebase init functions
//existing projectから、1.で作ったプロジェクトを選択
```

## サンプルプログラムの作成

index.jsを参照。以下、参考ドキュメントからの変更点。

1. ignoreUndefinedPropertiesの有効化


```
admin.initializeApp();
//以下追加
admin.firestore().settings({ ignoreUndefinedProperties: true });
```

参考：https://stackoverflow.com/questions/61969722/how-to-enable-ignoreundefinedproperties-in-node-js


2. firestoreエミュレータの追加

firebase.jsonを編集してfirestoreのエミュレータを記述(ポート番号は適当につけた）

```
{
  "emulators": {
    "firestore": {
      "port": "5002"
    }
  }
}
```

参考：https://stackoverflow.com/questions/56158010/firestore-firebase-emulator-not-running


## ローカル環境テスト

以下コマンドでローカルテスト実行

```
$ firebase emulators:start
```

1. addMessageのテスト

```
http://localhost:5001/<プロジェクト名>/us-central1/addMessage
```

以下URLでFirestoreの中身を確認できる。うまくいっていれば"messages"というcollectionができている。

```
http://localhost:4000/firestore/
```

2. makeUppercaseのテスト

```
http://localhost:5001/<プロジェクト名>/us-central1/addMessage?text=uppercaseme
```

originalとuppercaseというfieldを持つdocumentが追加されている。


## デプロイ

以下コマンドでデプロイ。ただし、blazeプランの契約？が必要。

```
$ firebase deploy --only functions
```

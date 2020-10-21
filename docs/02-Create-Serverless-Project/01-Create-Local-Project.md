# 手順1: ローカル環境にプロジェクトを作成する

この手順では、インストールしたServerless Toolkitを使ってTwilio Runtimeにデプロイ可能なプロジェクトをローカル開発環境に作成する方法を体験します。

Twilio Runtimeを利用することで、開発者が自分自身でWebアプリケーションをホスティングする必要がなくなります。

## この手順を進めるための前提条件
- Twilio CLIとServerless Toolkitがインストールされていること

## 2-1. プロジェクトの初期化

次のコマンドでプロジェクトを作成します。

```
twilio serverless:init serverless-handsOn --template never-gonna-give-you-up
```

上記コマンドは、`serverless-handson`という名前のプロジェクトを`never-gonna-give-you-up`というテンプレートをベースとして作成します。

```
╭──────────────────────────────────────────────────────────────────────────────╮
│                                                                              │
│   Success!                                                                   │
│                                                                              │
│   Created serverless-handsOn at /Users/dikehara/VSCode/labs-projects         │
│                                                                              │
│   Inside that directory, you can run the following command:                  │
│                                                                              │
│   npm start                                                                  │
│     Serves all functions in the ./functions subdirectory and assets in the   │
│     ./assets directory                                                       │
│                                                                              │
│   Get started by running:                                                    │
│                                                                              │
│   cd serverless-handsOn                                                      │
│   npm start                                                                  │
│                                                                              │
╰──────────────────────────────────────────────────────────────────────────────╯
```

この指示に従い次のコマンドを実行してみましょう。

```
cd serverless-handson
npm start
```

この段階で[http://localhost:3000/never-gonna-give-you-up]()をブラウザーで開くと、[TwiML](https://jp.twilio.com/docs/voice/twiml/pay) が表示されます。

このプロジェクトフォルダーの中にある`/functions/never-gonna-give-you-up.js`を開くと、先ほどのTwiMLを出力するためのコードを確認できます。

```js
exports.handler = function(context, event, callback) {
  const twiml = new Twilio.twiml.VoiceResponse();
  twiml.play('https://demo.twilio.com/docs/classic.mp3');
  callback(null, twiml);
};
```

Twilio Funcionsでは関数呼び出しを終えた場合に必ず`callback`関数を呼び出します。

## 2-2. 音楽の再生前にメッセージを流す

いきなり音楽が流れてしまうと発信元が驚いてしまいます。そこで[<Say>動詞](https://jp.twilio.com/docs/voice/twiml/say)を再生前に追加し、案内メッセージを流します。

関数を次のように変更しましょう。

```js
exports.handler = function(context, event, callback) {
  const twiml = new Twilio.twiml.VoiceResponse();
  twiml.say('お電話ありがとうございます。一曲お聞きください。', {language: 'ja-JP'});
  twiml.play('https://demo.twilio.com/docs/classic.mp3');
  callback(null, twiml);
};
```

次の手順ではこのプロジェクトをローカルで実行する方法を学習します。

---
**参考: どんなテンプレートがあるかを確認する**

テンプレートの一覧を確認したい場合は、次のコマンドを実行してください。

```
twilio serverless:list-tampltes
```

これらのテンプレートはGitHubの[twilio-labs/function-templates](https://github.com/twilio-labs/function-templates)で管理されています。

---

## 関連リソース

- [Create a Project](https://www.twilio.com/docs/labs/serverless-toolkit/general-usage#create-a-project)



## 次の手順
[手順2: ローカル環境でデバッグを開始する](02-Local-Debug.md)
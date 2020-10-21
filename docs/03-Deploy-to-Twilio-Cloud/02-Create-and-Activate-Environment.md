# 手順2: デプロイ環境の切り替え

この手順ではServiceに別の環境を作成しデプロイする方法を学習します。

## 2-1. その1 プロジェクトを別の環境にデプロイ

Serverless Toolkitはデフォルトで`dev-environment`という環境にデプロイします。この環境を切り替える場合は`--environment`フラグを利用します。このフラグで指定した環境が存在していない場合は自動的に作成します。

次のコードを実行し、ステージング環境にデプロイしましょう。

```
twilio serverless:deploy --environment=staging
```

出力ログの`Environment`およびURLの文字列に`staging`が含まれていることを確認します。（一部表示を変更しています）

```
Account         SK******************************
Token           Yd******************************
Service Name    serverless-handsOn
Environment     staging
Root Directory  /******/labs-projects/serverless-handsOn
Dependencies
Env Variables   TWILIO_HANDSON_NAME

✔ Serverless project successfully deployed

Deployment Details
Domain: *****-staging.twil.io
Service:
   serverless-handsOn (ZS*****************************)
Environment:
   dev (ZE*****************************) 
Build SID:
   ZB*****************************
View Live Logs:
   https://www.twilio.com/console/assets/api/ZS*****************************/environment/ZE*****************************
Functions:
   [protected] https://*****-staging.twil.io/my-new-function/hello-world
   https://*****-staging.twil.io/never-gonna-give-you-up
Assets:

```

## 2-2. その2 別の環境に既存の環境をプロモート

`deploy`コマンドはローカルコードをアップロードするため、コードを変更した場合、2つの環境で異なるコードベースとなってしまう可能性があります。この状態を避けるため、`promote` またはエイリアスの `activate`を利用し別のコードベースをコピーできます。

次のコマンドを実行し、ベータ環境を作成してみましょう。

```
twilio serverless:promote --source-environment=staging --environment=beta --create-environment
```

出力ログは以下の通りです。（一部表記を変更しています。）

```
Account SK******************************
Token   Yd******************************

✔ Activated new build from staging on beta

Active build available at:
******-beta.twil.io

View Live Logs:
  https://www.twilio.com/console/assets/api/ZS****************************/environment/ZE****************************
```

## 2-3. デプロイ、プロモート時に別の環境変数ファイルを利用する

開発用、テスト用、ステージング用とデータベースや資格情報を切りかえる目的で環境変数に値を保持している場合は、デプロイ、プロモート双方で読み込む環境変数ファイルを`--env`で指定できます。

`.env`ファイルを複製し`.env.beta`と名前を変更してください。このファイルの`TWILIO_HANDSON_NAME`変数を変更します。

```
TWILIO_HANDSON_NAME=Twilioサーバーレスハンズオン（ベータ）
```

Beta環境に`--env`フラグを利用しデプロイしてみましょう。

```
twilio serverless:deploy --environment=beta --env=.env.beta
```

コンソールでこのBeta環境の関数を利用するように設定し、メッセージを確かめてください。

最後のハンズオンは不要になったサービスを削除する方法を学習します。

## 次のハンズオン

[ハンズオン: 不要なサービスの削除](/docs/04-Remove-Service/00-Overview.md)
# circleci-demo

## ローカル開発用準備

### circleci cliのインストール

* brewでもインストール可能だけど、`circleci update` が使えなかったり、最新版がインストールされなかったりするのでbashでインストールがおすすめ

```bash
curl -fLSs https://circle.ci/cli | bash
```

* トークンを取得して、`circleci setup` で初期設定する。詳細は[コチラ](https://circleci.com/docs/2.0/local-cli/#configuring-the-cli)

### gcpサービスアカウントキーの取得

* ローカル開発用アカウントキーを取得するか、だれか持ってると思うからもらうかする。

## ローカル確認手順

* 今のところ、実行コマンドがcircleci 2.1に対応してないので、ymlを2.0向けに変換する。

```bash
circleci config process .circleci/config.yml > .circleci/config-2.0.yml
```

* ローカル実行コマンド。セキュリティの観点からcontextがローカルでは展開されないのでオプションで指定する。詳細はコチラの[Environment Variables](https://circleci.com/docs/2.0/local-cli/#limitations-of-running-jobs-locally)の項を参照。

```bash
circleci local execute -c .circleci/config-2.0.yml --job gcp-cli/install_and_initialize_cli -e GCLOUD_SERVICE_KEY="$(cat ./あなたのサービスアカウントキー.json)" -e GOOGLE_PROJECT_ID=hogehoge -e GOOGLE_COMPUTE_ZONE=asia-northeast1-a
```
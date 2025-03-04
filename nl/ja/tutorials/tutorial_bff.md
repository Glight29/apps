---

copyright:
  years: 2016, 2017, 2018
lastupdated: "2018-06-26"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# backend-for-frontend API の作成
{: #tutorial}

backend-for-frontend スターターからアプリを作成できます。 これらのスターターを使用して、Express.js、MicroProfile/ Java EE、Kitura、Spring のようなさまざまなフレームワークによって、Node.js、Java、または Swift で backend-for-frontend API をビルドできます。 必要なツールをインストールして、アプリをビルドしてローカルで実行し、クラウドにデプロイする方法を確認できます。
{: shortdesc}

## ステップ 1: ツールのインストール
{: #before-you-begin}

[開発者ツール ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/IBM-Bluemix/ibm-cloud-developer-tools){: new_window} をインストールします。

## ステップ 2: アプリの作成
{: #create-devex}

{{site.data.keyword.cloud}} {{site.data.keyword.dev_console}}でアプリを作成します。

1. {{site.data.keyword.dev_console}}の[「スターター・キット」![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.ng.bluemix.net/developer/appservice/starter-kits/) ページから、ご使用の言語に対応するスターター・キットを選択します。 例えば、Node.js アプリケーションの場合は、**「Express.js バックエンド (Express.js Backend)」**に移動し、**「スターター・キットの選択」**をクリックします。

2. アプリ名を入力します。 このチュートリアルでは、`ExpressBackend` を使用します。

3. 自分のイニシャルに `-devhost` を付けたものなど、固有のホスト名を入力します。 例えば次のようにします。

   ```
   abc-devhost
   ```

   このホスト名がアプリの経路になります。 例えば、`abc-devhost.mybluemix.net` です。

4. ご使用の言語プラットフォームを選択します。 このチュートリアルでは、`Node.js` を使用します。

5. **「作成」**をクリックします。

## ステップ 3: オプション - リソースの追加
{: #add-services}

1. **「アプリ詳細」**ビューから、**「リソースの追加」**をクリックします。

2. 必要なサービスの種類を選択します。 このチュートリアルでは、**「データ」**>**「次へ」**>**「Cloudant NoSQL DB」**>**「次へ」**を選択します。

3. **「作成」**をクリックします。

## ステップ 4: オプション - DevOps ツールチェーンの作成
{: #add-toolchain}

ツールチェーンを有効にすると、アプリ用のチーム・ベースの開発環境が作成されます。 ツールチェーンの作成時に、アプリ・サービスによって Git リポジトリーがプロビジョンされます。このリポジトリーでは、ソース・コードの表示、アプリの複製、および問題の作成と管理を行うことができます。 また、専用の Gitlab 環境と、Kubernetes や Cloud Foundry などの選択したデプロイメント・プラットフォームに合わせてカスタマイズされた継続的 Delivery Pipeline にアクセスすることもできます。

一部のアプリケーションでは継続的デリバリーは有効になっています。 Delivery Pipeline と GitHub を使用してビルド、テスト、およびデプロイメントを自動化するために、継続的デリバリーを有効にすることをお勧めします。

1. **「アプリ」**ページでアプリを選択します。

2. **「クラウドにデプロイ (Deploy to Cloud)」**をクリックします。

3. デプロイメント方式を選択します。 以下のいずれかを選択できます。

	* Kubernetes クラスターにデプロイします。 ワーカー・ノードというホストのクラスターをプロビジョンして、高可用性のアプリケーション・コンテナーをデプロイして管理します。 クラスターを作成したり、既存のクラスターにデプロイしたりすることができます。

	* Cloud Foundry を使用してデプロイします。この場合、基礎となるインフラストラクチャーを管理する必要はありません。

## ステップ 5: アプリ・コードの生成
{: #generate-code}

前のステップでツールチェーンを作成した場合は、アプリの Git リポジトリーが作成されたため、ここでコードを見つけることができます。 リポジトリーにアクセスするには、以下のステップを実行します。

1. **「アプリ」**ページでアプリを選択します。

2. **「ツールチェーンの表示」**をクリックします。

3. 見出し**「コード」**の下で**「Git」**カードをクリックして、リポジトリーを開きます。ここで、ソース・コードを表示してアプリを複製できます。

ツールチェーンが有効になっていない場合は、「アプリ詳細」ビューからソースを直接ダウンロードすることで、コードにアクセスできます。

1. **「アプリ」**ページでアプリを選択します。

2. **「コードのダウンロード」**をクリックして、アプリ・アーカイブをダウンロードします。

## ステップ 6: アプリでの作業の開始
{: #code}

ダウンロードしたアプリでの作業を開始します。

1. アーカイブ対象ファイルを展開します。

2. アプリを IDE にインポートします。

3. コードを変更します。

4. {{site.data.keyword.dev_cli_notm}} を使用して、コードをビルドしてローカルで実行します。

## ステップ 7: アプリのビルドおよびローカルでの実行
{: #build-run}

独自のコードを追加して、アプリをビルドして実行します。 必要なビルド・ツールをインストールした場合、または {{site.data.keyword.dev_cli_notm}} で使用可能なコンテナー・サポートを使用することで、ホスト・システム上でローカルにアプリケーションを実行することができます。

### {{site.data.keyword.dev_cli_short}} の使用

1. 現行のアプリ・ディレクトリーでアプリをビルドするには、以下のコマンドを入力します。

   ```
   ibmcloud dev build
   ```
   {: codeblock}

2. 現行のアプリ・ディレクトリーでアプリを実行するには、以下のコマンドを入力します。

   ```
   ibmcloud dev run
   ```
   {: codeblock}

3. 以下を指定して、サーバーで curl を実行できます。

   ```
   curl http://localhost:3000
   ```
   {: codeblock}

4. サーバーの以下の場所にある API 資料を参照できます。

   ```
   http://localhost:3000/swagger/api
   ```
   {: codeblock}

5. サーバーの以下の場所にある API を探索できます。

   ```
   http://localhost:3000/explorer
   ```
   {: codeblock}

## ステップ 8: クラウドへのデプロイ
{: #deploy}

### ツールチェーンを使用したデプロイ
アプリを {{site.data.keyword.cloud_notm}} にデプロイする方法はいくつもありますが、DevOps ツールチェーンを使用するのが、実動アプリをデプロイするのに最適な方法です。  DevOps ツールチェーンを使用すると、複数の環境へのデプロイメントを簡単に自動化して、成長するアプリの管理に役立つようにモニタリング、ロギング、およびアラートの各サービスを素早く追加することができます。

正しく構成されたツールチェーンを使用すると、リポジトリー内のマスター・ブランチへのマージが行われるたびに、ビルドとデプロイのサイクルが自動的に開始されます。 {{site.data.keyword.cloud_notm}} 開発者ダッシュボードから作成されたツールチェーンはすべて、自動デプロイメント用に構成されています。

DevOps ツールチェーンからアプリを手動でデプロイすることもできます。

1. **「アプリ」**ページでアプリを選択します。

2. **「ツールチェーンの表示」**をクリックします。

3. ビルドの開始、デプロイメントの管理、およびログと履歴の表示を行うことができる Delivery Pipeline を表示します。

### {{site.data.keyword.dev_cli_short}} を使用したデプロイ
ツールチェーンを使用しないことを選択した場合、{{site.data.keyword.dev_cli_short}} を使用してデプロイすることもできます。

アプリを Cloud Foundry にデプロイするには、以下のコマンドを入力します。

```
ibmcloud dev deploy
```
{: codeblock}

アプリを Kubernetes クラスターにデプロイするには、以下のコマンドを入力します。

```
ibmcloud dev deploy --target <container>
```
{: codeblock}

### アプリケーションが実行中であることの確認
アプリケーションがデプロイされたら、DevOps パイプライン内または端末内 (コマンド・ラインを使用している場合) に `abc-devhost.mybluemix.net` のような URL が表示されます。 ご使用のブラウザーでその URL にアクセスします。

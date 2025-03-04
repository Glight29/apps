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

# 建立 Backend-for-Frontend API
{: #tutorial}

您可以從 Backend-for-Frontend 入門範本建立應用程式。請利用這些入門範本，以各種架構建置用 Node.js、Java 或 Swift 撰寫的 Backend-for-Frontend API，架構包括：Express.js、MicroProfile/ Java EE、Kitura 或 Spring。您可以查看如何安裝所需的工具、在本端建置並執行應用程式，然後將它部署至雲端。
{: shortdesc}

## 步驟 1：安裝工具
{: #before-you-begin}

安裝[開發人員工具 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/IBM-Bluemix/ibm-cloud-developer-tools){: new_window}。

## 步驟 2：建立應用程式
{: #create-devex}

在 {{site.data.keyword.cloud}} {{site.data.keyword.dev_console}} 中建立應用程式。

1. 從 {{site.data.keyword.dev_console}} 的[入門範本套件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.ng.bluemix.net/developer/appservice/starter-kits/) 頁面中，選取您語言的入門範本套件。例如，若為 Node.js 應用程式，請移至 **Express.js 後端**，然後按一下**選取入門範本套件**。

2. 輸入應用程式名稱。針對本指導教學，請使用 `ExpressBackend`。

3. 輸入唯一的主機名稱，例如您的首字再加上 `-devhost`。例如：

   ```
abc-devhost
	```

   這個主機名稱是您應用程式的路徑。例如 `abc-devhost.mybluemix.net`。

4. 選取語言平台。針對本指導教學，請使用 `Node.js`。

5. 按一下**建立**。

## 步驟 3：選用 - 新增資源
{: #add-services}

1. 從**應用程式詳細資料**視圖，按一下**新增資源**。

2. 選取您要的服務類型。針對本指導教學，請選取**資料** > **下一步** > **Cloudant NoSQL DB** > **下一步**。

3. 按一下**建立**。

## 步驟 4：選用 - 建立 DevOps 工具鏈
{: #add-toolchain}

啟用工具鏈會為您的應用程式建立一個團隊型開發環境。建立工具鏈時，應用程式服務會佈建 Git 儲存庫，您可以在其中檢視原始碼、複製應用程式以及建立和管理問題。您也可以存取專用的 Gitlab 環境，以及針對您所選部署平台（例如 Kubernetes 或 Cloud Foundry）自訂的 Continuous Delivery Pipeline。

已針對部分應用程式啟用持續交付。建議您啟用持續交付，以透過 Delivery Pipeline 及 GitHub 自動建置、測試及部署。

1. 在**應用程式**頁面中選取您的應用程式。

2. 按一下**部署至雲端**。

3. 選取部署方法。您可以選擇任一項：

	* 部署至 Kubernetes 叢集。佈建主機的叢集（稱為工作者節點），以部署及管理高可用性的應用程式容器。您可以建立叢集或部署至現有的叢集。

	* 使用 Cloud Foundry 部署，如此便不需要管理基礎的基礎架構。

## 步驟 5：產生應用程式碼
{: #generate-code}

如果您在前一個步驟中建立工具鏈，則會為您的應用程式建立 Git 儲存庫，且您可以在該處找到程式碼。請遵循下列步驟來存取您的儲存庫：

1. 在**應用程式**頁面中選取您的應用程式。

2. 按一下**檢視工具鏈**。

3. 按一下 **CODE** 標題下的 **Git** 卡以開啟儲存庫，您可以在儲存庫裡檢視原始碼及複製應用程式。

如果未啟用工具鏈，您可以直接從「應用程式詳細資料」視圖下載原始檔，以存取程式碼。

1. 在**應用程式**頁面中選取您的應用程式。

2. 按一下**下載程式碼**以下載您的應用程式保存檔。

## 步驟 6：開始使用您的應用程式
{: #code}

開始使用下載的應用程式：

1. 展開保存檔。

2. 將應用程式匯入至 IDE。

3. 修改程式碼。

4. 使用 {{site.data.keyword.dev_cli_notm}} 在本端建置及執行程式碼。

## 步驟 7：在本端建置及執行應用程式
{: #build-run}

新增您自己的程式碼、建置，然後執行應用程式。如果您安裝必要的建置工具，或使用 {{site.data.keyword.dev_cli_notm}} 中可用的容器支援，則可以在主機系統本端上執行應用程式。

### 使用 {{site.data.keyword.dev_cli_short}}

1. 若要在現行應用程式目錄中建置應用程式，請輸入下列指令：

   ```
  ibmcloud dev build
  ```
   {: codeblock}

2. 若要在現行應用程式目錄中執行應用程式，請輸入下列指令：

   ```
  ibmcloud dev run
  ```
   {: codeblock}

3. 您可以使用下列指令在伺服器上執行 curl：

   ```
   curl http://localhost:3000
   ```
   {: codeblock}

4. 您可以檢視伺服器上的 API 文件，網址為：

   ```
   http://localhost:3000/swagger/api
   ```
   {: codeblock}

5. 您可以在伺服器上探索 API，網址為：

   ```
   http://localhost:3000/explorer
   ```
   {: codeblock}

## 步驟 8：部署至雲端
{: #deploy}

### 使用工具鏈部署
有數種方式可以將應用程式部署至 {{site.data.keyword.cloud_notm}}，但使用 DevOps 工具鏈是部署正式作業應用程式的最佳方法。DevOps 工具鏈可讓您輕鬆地自動部署到多個環境，並快速新增監視、記載和警示服務，以協助您在應用程式成長時進行管理。

使用適當配置的工具鏈，建置-部署的循環會自動開始，並且全都合併到您儲存庫的主要分支。從 {{site.data.keyword.cloud_notm}} 開發人員儀表板建立的所有工具鏈都會針對自動部署進行配置。

您也可以從 DevOps 工具鏈手動部署應用程式：

1. 在**應用程式**頁面中選取您的應用程式。

2. 按一下**檢視工具鏈**。

3. 檢視您的交付管線，您可以在其中啟動建置、管理部署，以及檢視日誌和歷程。

### 使用 {{site.data.keyword.dev_cli_short}} 部署
如果您選擇不使用工具鏈，則也可以使用 {{site.data.keyword.dev_cli_short}} 來進行部署。

若要將應用程式部署至 Cloud Foundry，請輸入下列指令：

```
ibmcloud dev deploy
```
{: codeblock}

若要將應用程式部署至 Kubernetes 叢集，請輸入下列指令：

```
ibmcloud dev deploy --target <container>
```
{: codeblock}

### 查看您的應用程式執行
部署應用程式之後，您會在 DevOps 管線或終端機（如果使用指令行）看到類似於 `abc-devhost.mybluemix.net` 的 URL。請在瀏覽器中造訪該 URL。

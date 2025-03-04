---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-26"

---

{: new_window: target="_blank"}
{:shortdesc: .shortdesc}
{: codeblock: .codeblock}

# 將服務新增至應用程式
{: #add_service}

使用 {{site.data.keyword.Bluemix_notm}}{{site.data.keyword.dev_console}} 建立應用程式時，您可以從應用程式概觀頁面新增資源。不過，您也可以從應用程式的環境定義之外，直接從 {{site.data.keyword.Bluemix_notm}} 型錄佈建它們。
{: shortdesc}

您可以要求資源的實例，並單獨於應用程式之外來使用，也可以從應用程式概觀頁面，將資源實例新增至應用程式。您可以直接從 {{site.data.keyword.Bluemix_notm}} 型錄佈建特定類型的資源（服務）。

## 探索服務
{: #discover_services}

您可以用下列方式查看 {{site.data.keyword.Bluemix_notm}} 中可用的所有服務：


* 從 {{site.data.keyword.Bluemix_notm}} 主控台。檢視 {{site.data.keyword.Bluemix_notm}} 型錄。
* 從 ibmcloud 指令行介面。使用 `ibmcloud service offerings` 指令。
* 從您自己的應用程式。請使用 [GET /v2/services Services API ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://apidocs.cloudfoundry.org/197/services/list_all_services.html){: new_window}。

在開發應用程式時，您可以選取需要的服務。選取它之後，{{site.data.keyword.Bluemix_notm}} 會佈建服務。不同服務類型的佈建處理程序可能不同。例如，資料庫服務會建立資料庫，而行動應用程式的推送通知服務則會產生配置資訊。


{{site.data.keyword.Bluemix_notm}} 會透過使用服務實例，將服務資源提供給應用程式。服務實例可以在 Web 應用程式之間共用。

如果在其他地區中管理的服務能用於那些地區，則您也可以使用那些服務。這些服務必須可從網際網路存取，並具有 API 端點。您必須用您編碼外部應用程式或協力廠商工具以使用 {{site.data.keyword.Bluemix_notm}} 服務的相同方式，手動編碼應用程式來使用這些服務。如需相關資訊，請參閱[讓外部應用程式及協力廠商工具能使用 {{site.data.keyword.Bluemix_notm}} 服務](#accser_external)。

## 要求新的服務實例
{: #req_instance}

若要要求新的服務實例，您必須使用 {{site.data.keyword.Bluemix_notm}} 使用者介面或 {{site.data.keyword.Bluemix_notm}} 指令行介面。

**附註：**當您指定服務名稱時，請避免非英文字母或數值字元的字元，因為結果可能無法預期。

如果您使用 {{site.data.keyword.Bluemix_notm}} 使用者介面來要求服務實例，請完成下列步驟：

1. 在 {{site.data.keyword.Bluemix_notm}} 型錄中，按一下您要新增的服務磚。即會開啟「服務詳細資料」頁面。

2. 在**服務名稱**欄位鍵入名稱。會提供預設名稱。您可以在欄位中變更名稱，或是將它保留不變。

3. 完成其他欄位或選擇，然後按一下**建立**。

如果您使用 {{site.data.keyword.Bluemix_notm}} 指令行介面來要求服務實例，請完成下列步驟：

1. 使用 `ibmcloud service offerings` 指令，以尋找所需服務的名稱及方案。

2. 使用下列指令來建立服務實例，其中 service_name 是服務的名稱、service_plan 是服務的方案，而 service_instance 是您要用於此服務實例的名稱。

```
ibmcloud service create service_name service_plan service_instance
```

3. 使用下列指令將服務實例連結至應用程式，其中 *appname* 是應用程式的名稱，而 service_instance 是服務實例的名稱。

```
ibmcloud service bind appname service_instance
```

您可以將服務實例僅連結至位於相同空間或組織中的應用程式實例。然而，使用其他空間或組織中的服務實例的方式，與使用外部應用程式的方式一樣。請使用認證直接配置應用程式實例，而非建立連結。如需外部應用程式如何使用 {{site.data.keyword.Bluemix_notm}} 服務的相關資訊，請參閱[讓外部應用程式能使用 {{site.data.keyword.Bluemix_notm}} 服務 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](#accser_external){: new_window}。

## 配置應用程式
{: #config}

將服務實例連結至應用程式之後，您必須配置應用程式以與服務互動。

每一個服務可能需要不同的機制來和應用程式通訊。
這些機制記載於服務定義的一部分，以在您開發應用程式時作為參考資訊。
為了一致性，必須要有機制，您的應用程式才能與服務互動。


* 若要與資料庫服務互動，請使用 {{site.data.keyword.Bluemix_notm}} 所提供的資訊（例如，使用者 ID、密碼，以及應用程式的存取 URI）。
* 若要與行動後端服務互動，請使用 {{site.data.keyword.Bluemix_notm}} 所提供的資訊（例如，應用程式身分（應用程式 ID）、用戶端特有的安全資訊，以及應用程式的存取 URI）。行動服務通常會彼此合作，以便環境定義資訊（例如，應用程式開發人員的名稱，以及使用應用程式的使用者）能在服務集之間共用。
* 若要與 Web 應用程式或是行動應用程式的伺服器端雲端程式碼互動，請使用 {{site.data.keyword.Bluemix_notm}} 所提供的資訊（例如，應用程式的 *VCAP_SERVICES* 環境變數中的運行環境認證）。*VCAP_SERVICES*
環境變數的值是 JSON 物件的序列化。
變數包含與應用程式所連結之服務互動時所需要的運行環境資料。
不同服務會有不同的資料格式。
您可能需要閱讀服務文件，以瞭解預期的內容，以及如何解譯每一份資訊。


如果您連結至應用程式的服務當機，應用程式可能會停止執行或發生錯誤。{{site.data.keyword.Bluemix_notm}} 不會自動重新啟動應用程式，以從這些問題回復。請考慮將應用程式編碼成可識別運作中斷、異常狀況和連線失敗並從其中回復。如需相關資訊，請參閱[不會自動重新啟動應用程式](/docs/troubleshoot/ts_apps.html#ts_apps_not_auto_restarted)。

## 跨 {{site.data.keyword.Bluemix_notm}} 部署環境存取服務
{: #migrate_instance}

{{site.data.keyword.Bluemix_notm}} 提供了許多部署選項，您可以從某個環境，存取不同環境中執行的服務。例如，如果您的服務是在 Cloud Foundry 中執行，則可以從在 Kubernetes 叢集中執行的應用程式存取該服務。

### 範例：從 Kubernetes Pod 存取 Cloud Foundry 上的 Compose 服務實例

任何 Compose 服務實例（例如 {{site.data.keyword.composeForMongoDB}} 或 {{site.data.keyword.composeForRedis}}）都是付費實例。在您適應使用 Compose 服務實例（例如 Kubernet 中的 {{site.data.keyword.composeForMongoDB}}）之後，您可以在 Cloud Foundry 匯入 Compose 所提供實例的認證。

1. 移至**認證**，並從實例擷取您的認證。

2. 從圖表目錄（例如 `chart/project/`）開啟 `values.yml` 檔。

3. 設定服務環境中參照的值。例如，在 {{site.data.keyword.composeForMongoDB}} 中：

  ```
  services:
    mongo:
       url: {uri}
       dbName: {dbname}
       ca: {ca_certificate_base64}
       username: {username}
       password: {password}
       env: production

  ```

4. 從圖表目錄（例如 `chart/project/`）開啟 `bindings.yml` 檔。

5. 在定義 `env` 區塊的結尾，新增 `values.yml` 檔中所定義的金鑰值參照。

  ```
    env:
      - name: MONGO_URL
        value: {{ .Values.services.mongo.url }}
      - name: MONGO_DB_NAME
        value: {{ .Values.services.mongo.name }}
      - name: MONGO_USER
        value: {{ .Values.services.mongo.username }}
      - name: MONGO_PASS
        value: {{ .Values.services.mongo.password }}
      - name: MONGO_CA
        value: {{ .Values.services.mongo.ca }}
  ```

6. 在應用程式中，使用環境變數來起始提供給您的服務 SDK。

  ```javascript
    const serviceManger = require('./services/serivce-manage.js');
    const mongoURL = process.env.MONGO_URL || 'localhost';
    const mongoUser = process.env.MONGO_USER || '';
    const mongoPass = process.env.MONGO_PASS || '';
    const mongoDBName = process.env.MONGO_DB_NAME || 'comments';
    const mongoCA = [new Buffer(process.env.MONGO_CA || '', 'base64')]

    const options = {
    useMongoClient: true,
        ssl: true,
        sslValidate: true,
        sslCA: mongoCA,
        poolSize: 1,
        reconnectTries: 1
    };

    const mongoDBClient = serviceManger.get('mongodb');
  ```

### 密碼（選用）
{: #migrate_secrets_optional}

請勿在 `deployment.yml` 或 `values.yml` 檔中公開您的認證。您可以使用 base64 編碼字串，或使用金鑰來加密您的認證。如需相關資訊，請參閱 [Creating a Secret Using kubectl create secret ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/concepts/configuration/secret/#creating-your-own-secrets) 及 [How to encrypt your data ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/)

## 啟用外部應用程式
{: #accser_external}

您可能有在 {{site.data.keyword.Bluemix_notm}} 之外建立和執行的應用程式，或是您可能使用協力廠商工具。如果 {{site.data.keyword.Bluemix_notm}} 服務提供可從網際網路存取的服務金鑰，您可以使用那些服務來搭配本端應用程式或協力廠商工具。

下列服務提供您可在外部使用的服務金鑰：

* {{site.data.keyword.amashort_old}} <!--Advanced Mobile Access-->
* {{site.data.keyword.alchemyapishort}} <!--AlchemyAPI-->
* {{site.data.keyword.alertnotificationshort}} <!--Alert Notification-->
* {{site.data.keyword.sparks}} <!--Analytics for Apache Spark-->
* {{site.data.keyword.appseccloudshort}} <!--Application Security on Cloud-->
* {{site.data.keyword.blockchain}} <!--Blockchain-->
* {{site.data.keyword.cloudant}} <!--Cloudant&reg; NoSQL DB-->
* {{site.data.keyword.iotmapinsights_short}} <!--Context Mapping-->
* {{site.data.keyword.conversationshort}} <!--Conversation-->
* {{site.data.keyword.dashdbshort}} <!--dashDB-->
* {{site.data.keyword.discoveryshort}} <!--Discovery-->
* {{site.data.keyword.documentconversionshort}} <!--Document Conversion-->
* {{site.data.keyword.iotdriverinsights_short}} <!--Driver Behavior-->
* {{site.data.keyword.geospatialshort_Geospatial}} <!--Geospatial Analytics-->
* {{site.data.keyword.GlobalizationPipeline_short}} <!--Globalization Pipeline-->
* {{site.data.keyword.appconserviceshort}} <!--IBM&reg; App Connect-->
* {{site.data.keyword.dataworks_short}} <!--IBM&reg; Data Connect-->
* {{site.data.keyword.graphshort}} <!--IBM&reg; Graph-->
* {{site.data.keyword.iotelectronics_full}} <!--IBM&reg; IoT for Electronics-->
* {{site.data.keyword.twittershort}} <!--Insights for Twitter-->
* {{site.data.keyword.iot4auto_short}} <!--IoT for Automotive-->
* {{site.data.keyword.iotinsurance_short}} <!--IoT for Insurance-->
* {{site.data.keyword.languagetranslatorshort}} <!--Language Translator-->
* {{site.data.keyword.dwl_short}} <!--Lift-->
* {{site.data.keyword.messagehub}} <!--Message Hub-->
* {{site.data.keyword.mobileanalytics_short}} <!--Mobile Analytics-->
* {{site.data.keyword.nlclassifiershort}} <!--Natural Language Classifier-->
* {{site.data.keyword.objectstorageshort}} <!--Object Storage-->
* {{site.data.keyword.personalityinsightsshort}} <!--Personality Insights-->
* {{site.data.keyword.HybridConnect_short}} <!--Product Insights-->
* {{site.data.keyword.mobilepush}} <!--Push-->
* {{site.data.keyword.retrieveandrankshort}} <!--Retrieve and Rank-->
* {{site.data.keyword.speechtotextshort}} <!-- Speech to Text-->
* {{site.data.keyword.streaminganalyticsshort}} <!--Streaming Analytics-->
* {{site.data.keyword.texttospeechshort}} <!--Text to Speech-->
* {{site.data.keyword.toneanalyzershort}} <!--Tone Analyzer-->
* {{site.data.keyword.tradeoffanalyticsshort}} <!--Tradeoff Analytics-->
* {{site.data.keyword.weather_short}} <!--Weather Company Data-->
* {{site.data.keyword.workloadscheduler}} <!--Workload Scheduler-->

若要讓外部應用程式或協力廠商工具能使用 {{site.data.keyword.Bluemix_notm}} 服務，請完成下列步驟：

1. 要求服務的實例。
    1. 在 {{site.data.keyword.Bluemix_notm}} 使用者介面的「儀表板」上，按一下**建立資源**。即會顯示型錄。
    2. 從型錄中，按一下服務磚以選取您想要的服務。即會開啟「服務詳細資料」頁面。
    3. 在服務視窗中，將**連接至：**清單選項保留為**維持不連結**。此選項表示服務未連接至 {{site.data.keyword.Bluemix_notm}} 應用程式。
    4. 視需要進行任何其他選擇。然後，按一下**建立**。即會建立服務實例，且會顯示服務「儀表板」。
2. 在服務「儀表板」中，您可以選取**服務認證**，以使用 JSON 格式檢視或新增認證。選取一組認證，然後按一下「動作」直欄中的**檢視認證**。使用顯示為認證的 API 金鑰來連接至服務實例。

在 {{site.data.keyword.Bluemix_notm}} 外執行的應用程式現在可以存取 {{site.data.keyword.Bluemix_notm}} 服務。

**附註：**如果您要刪除服務實例或檢查計費資訊，您必須回到使用者介面中的「儀表板」才能管理服務實例。

## 建立使用者提供的服務實例
{: #user_provide_services}

您的服務可能是在 {{site.data.keyword.Bluemix_notm}} 外部進行管理。如果您有認證可從網際網路存取那些外部服務，則可以建立 {{site.data.keyword.Bluemix_notm}} 使用者提供的服務實例來代表外部服務並與之通訊。

若要建立使用者提供的服務實例，並將它連結至應用程式，請完成下列步驟：

1. 使用 `ibmcloud service user-provided-create` 指令，建立使用者提供的服務實例：
    * 若要建立一般使用者提供的服務實例，請使用 **-p** 選項，並使用逗點區隔參數名稱。`ibmcloud` 指令行介面接著會提示您依次輸入每一個參數。例如：
        ```
        ibmcloud service user-provided-create testups1 -p "host, port, dbname, username, password"
        host> pubsub01.example.com
        port> 1234
        dbname> sampledb01
        username> pubsubuser
        password> p@$$w0rd
        Creating user provided service testups1 in org my-org / space dev as user@sample.com...
        OK
        ```

    * 若要建立將資訊排除至協力廠商日誌管理軟體的服務實例，請使用 `-l` 選項，然後指定協力廠商日誌管理軟體所提供的目的地。例如：

        ```
        ibmcloud service user-provided-create testups2 -l syslog://example.com
        Creating user provided service testups2 in org my-org / space dev as user@sample.com...
        OK
        ```

    如果您要更新使用者提供服務實例的一個以上參數，請使用 `ibmcloud service user-provided-update`。

    * 若要更新一般使用者提供的服務實例，請使用 **-p** 選項，並在 JSON 物件中指定參數索引鍵和值。例如：

        ```
        ibmcloud service user-provided-update testups1 -p "{\"username\":\"pubsubuser2\",\"password\":\"p@$$w0rd2\"}"
        Updating user provided service testups1 in org my-org / space dev as user@sample.com...
        OK
        ```

    * 若要建立將資訊排除至協力廠商日誌管理軟體的服務實例，請使用 `-l` 選項。例如：

        ```
        ibmcloud service user-provided-create testups2 -l syslog://example2.com
        Updating user provided service testups2 in org my-org / space dev as user@sample.com...
        OK
        ```

2. 使用 `ibmcloud service bind` 指令，將服務實例連結至應用程式。例如：

	```
	ibmcloud service bind myapp testups1
	Binding service testups1 to app myapp in org my-org / space dev as user@sample.com...
	OK
	```

您現在可以將應用程式配置成使用外部服務。如需如何配置應用程式與服務互動的相關資訊，請參閱[配置應用程式以與服務互動 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](#config){: new_window}。


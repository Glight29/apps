---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-14"

---

{:shortdesc: .shortdesc}
{:tip: .tip}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

# 建立憑證簽署要求
{: #ssl_csr}

您可以上傳 SSL 憑證並限制應用程式存取，來保護您的應用程式。
{:shortdesc}

您必須先在伺服器上建立憑證簽署要求 (CSR)，才能使用 {{site.data.keyword.Bluemix}} 上傳授與您的 SSL 憑證。

CSR 是傳送給憑證管理中心的訊息，用來要求簽署公開金鑰及相關聯的資訊。最常見的 CSR 是 PKCS #10 格式。CSR 包含公開金鑰，以及通用名稱、組織、城市、州省、國家或地區，以及電子郵件。只接受 CSR 金鑰長度 2048 位元的 SSL 憑證要求。

## 需要的資訊

在產生 CSR 時必須輸入下列資訊，CSR 才會有效。

### 國家或地區名稱

  兩碼的代碼，代表國家或地區。例如，"US" 代表美國。若為其他國家或地區，請先檢查 [ISO 國碼清單 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.iso.org/obp/ui/#search){:new_window}，然後再建立 CSR。

### 州/省（縣/市）

  州/省（縣/市）的完整未縮寫名稱。

### 地區

  城鎮或城市的完整名稱。

### 組織

  企業或公司在您的地區合法登錄的完整名稱，或是人名。若為公司，請務必包含登錄字尾，例如 Ltd.、Inc. 或 NV。

### 組織單位

  貴公司訂購憑證的分支名稱，例如會計或行銷。

### 通用名稱

  您要求 SSL 憑證的完整網域名稱 (FQDN)。

建立 CSR 的方法會視您的作業系統而改變。下列範例顯示如何使用 [OpenSSL 指令行工具 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://www.openssl.org/){:new_window} 建立 CSR：

```
openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout
    privatekey.key
```

OpenSSL SHA-512 實作取決於 64 位元整數類型的編譯器支援。您可以使用 SHA-1 選項來處理具有 SHA-256 憑證相容性問題的應用程式。
{: tip}

憑證由憑證管理中心發行，並由該憑證管理中心執行數位簽署。在建立 CSR 之後，可在公用憑證管理中心產生 SSL 憑證。

## 上傳 SSL 憑證
{: #ssl_certificate}

您可以套用安全通訊協定以提供應用程式的通訊隱私權，以避免竊聽、竄改及訊息偽造。

對於 {{site.data.keyword.Bluemix_notm}} 中的每個組織，當帳戶擁有者具有目前有效的「隨收隨付制」或「訂閱」方案時，您可以進行四次憑證上傳。對於每個組織，當帳戶擁有者具有免費試用帳戶時，您必須升級帳戶，才能上傳憑證。

您必須先建立憑證簽署要求，才能上傳憑證。

當您使用自訂網域來提供 SSL 憑證時，請使用下列地區端點，提供在 {{site.data.keyword.Bluemix_notm}} 中配置給組織的 URL 路徑：

  * US-South：secure.us-south.bluemix.net
  * US-East：secure.us-east.bluemix.net
  * EU-DE：secure.eu-de.bluemix.net
  * EU-GB：secure.eu-gb.bluemix.net
  * AU-SYD：secure.au-syd.bluemix.net


若要上傳應用程式的憑證，請執行下列動作：

1. 移至儀表板。

2. 選取應用程式，以開啟應用程式詳細資料視圖。

3. 按一下**路徑** > **管理網域**。

4. 針對您的組織，在動作直欄中，從其他動作功能表按一下**網域**。

5. 針對您的自訂網域，按一下「SSL 憑證」直欄中的**上傳**。

6. 瀏覽以上傳憑證、私密金鑰，以及選擇性地上傳中繼憑證或用戶端憑證。若要啟用用戶端憑證信任儲存庫，您必須上傳用戶端憑證信任儲存庫檔案，而此檔案定義您自訂網域的容許使用者存取權。

  #### 憑證

    一種數位文件，用來將公開金鑰連結到憑證擁有人的身分，使憑證擁有人能夠接受鑑別。憑證由憑證管理中心發行，並由該憑證管理中心執行數位簽署。

    一般是由憑證管理中心發出及簽署憑證。不過，您可能會使用自簽憑證進行測試及開發用途。

    {{site.data.keyword.Bluemix_notm}} 支援下列類型的憑證：

	* PEM（pem、.crt、.cer 及 .cert）
	* DER（.der 或 .cer）
	* PKCS #7（p7b、p7r、spc）

  #### 私密金鑰

    一種用來加密訊息的演算模式，只有對應的公開金鑰可以解密。另外，私密金鑰也用來將相對應公開金鑰所加密的訊息解密。私密金鑰保存在使用者系統上，受到密碼保護。

    {{site.data.keyword.Bluemix_notm}} 支援下列類型的私密金鑰：

    * PEM（pem、.key）
    * PKCS #8（p8、pk8）

  #### 中繼憑證

    由授信主要憑證管理中心 (CA) 發出的子層憑證，專門用於發出持卡實體伺服器憑證。結果會形成一條憑證鏈，從授信主要憑證管理中心開始、通過中繼憑證，最後以發給組織的 SSL 憑證結束。

    您應該使用中繼憑證來驗證主要憑證的確實性。中繼憑證通常是從信任的協力廠商處取得。
在將應用程式部署至正式作業之前，測試應用程式時不一定需要中繼憑證。


  #### 啟用用戶端憑證的要求

    如果您藉由上傳用戶端憑證信任儲存庫來啟用此選項，則嘗試存取 SSL 保護網域的使用者會被要求提供用戶端憑證。例如，在 Web 瀏覽器中，當使用者嘗試存取 SSL 保護的網域時，Web 瀏覽器會提示使用者提供網域的用戶端憑證。
使用**用戶端憑證信任儲存庫**檔案上傳選項，以定義容許存取您自訂網域的用戶端憑證。

  **附註：**{{site.data.keyword.Bluemix_notm}} 網域管理中的自訂憑證特性取決於「傳輸層安全 (TLS)」通訊協定的「伺服器名稱指示 (SNI)」延伸。因此，存取自訂憑證所保護之 {{site.data.keyword.Bluemix_notm}} 應用程式的用戶端程式碼必須支援 TLS 實作中的 SNI 延伸。如需相關資訊，請參閱 [RFC 4346 的第 7.4.2 節 ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://tools.ietf.org/html/rfc4346#section-7.4.2){:new_window} 及[使用傳輸層安全 (TLS) 保護資料](/docs/get-support/appsectls.html)。

  #### 用戶端憑證信任儲存庫

  用戶端憑證信任儲存庫是一個檔案，其中包含要容許存取您應用程式之使用者的用戶端憑證。如果您啟用要求用戶端憑證的選項，請上傳用戶端憑證信任儲存庫檔案。

   {{site.data.keyword.Bluemix_notm}} 支援下列類型的憑證：

      * PEM（pem、.crt、.cer 及 .cert）
      * PKCS #7（p7b、p7r、spc）

  您可以藉由上傳 meta 資料中包含公開金鑰的用戶端憑證信任儲存庫，來設定交互鑑別。
  {: tip}

如需相關資訊，請參閱[匯入 SSL 憑證](/docs/infrastructure/ssl-certificates/import-ssl-certificate.html#import-an-ssl-certificate)。

若要刪除憑證或以新憑證取代現有憑證，請移至**管理** > **帳戶** > **Cloud Foundry 組織**。然後，按一下**檢視詳細資料** > **編輯組織** > **網域**。在組織的其他動作功能表中，按一下**從組織移除**。

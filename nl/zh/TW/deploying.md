---

copyright:
  years: 2017, 2018
lastupdated: "2018-02-08"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 將 Compose 資料庫部署至 {{site.data.keyword.composeEnterprise}} 叢集

## 從 {{site.data.keyword.cloud_notm}} 主控台部署

如果已使用 {{site.data.keyword.cloud_notm}} 建立一個 {{site.data.keyword.composeEnterprise_full}} 叢集，則您可以指定將新的 Compose {{site.data.keyword.cloud_notm}} 資料庫佈建至該叢集。當您建立新的 Compose {{site.data.keyword.cloud_notm}} 資料庫服務實例時，請從*選取部署位置* 下拉畫面中的可用位置清單選取 {{site.data.keyword.composeEnterprise}} 叢集的名稱。

## 從指令行部署

若要建立 Compose 資料庫服務的新實例，並將它佈建至 {{site.data.keyword.composeEnterprise}} 叢集，首先您將需要從指令行安裝 {{site.data.keyword.cloud_notm}} 指令行介面 (CLI)，然後取得並使用 {{site.data.keyword.composeEnterprise}} 叢集 ID。

## 1. 設定 {{site.data.keyword.cloud_notm}} 指令行介面 (CLI) 

1. 下載並安裝 [{{site.data.keyword.cloud_notm}} CLI](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html) 工具。
2. 登入 {{site.data.keyword.cloud_notm}}。

    ```
    bx login
    ```

3. 切換至您要對新的 Compose 資料庫服務使用的組織及空間。

    ```
    bx target --cf
    ```

## 2. 取得叢集服務實例 ID

1. 您將需要取得 {{site.data.keyword.composeEnterprise}} 實例的叢集服務實例 ID：

    ```
    bx cf service COMPOSE_ENTERPRISE_SERVICE_NAME --guid
    ```

    此指令會傳回字串。這是服務實例的 GUID，而且稍後您將需要它來取得叢集 ID。

## 3. 使用 `create-service` 指令，將資料庫佈建至您的叢集：

既然有了 `cluster_service_instance_id`，您就可以使用 `create-service` 指令，來建立 {{site.data.keyword.cloud_notm}} Compose 資料庫服務，並將它部署至 {{site.data.keyword.composeEnterprise}} 叢集。


```
bx cf create-service service_name service_plan service_instance [-c PARAMETERS_AS_JSON]
```

指令選項如下：

<dl>
<dt>service_name（必要）</dt>
<dd>
Compose 資料庫服務的名稱。此值必須是下列其中一個：
    <ul>
        <li>`compose-for-elasticsearch`</li>
        <li>`compose-for-etcd`</li>
        <li>`compose-for-janusgraph`</li>
        <li>`compose-for-mongodb`</li>
        <li>`compose-for-mysql`</li>
        <li>`compose-for-postgresql`</li>
        <li>`compose-for-rabbitmq`</li>
        <li>`compose-for-redis`</li>
        <li>`compose-for-rethinkdb`</li>
        <li>`compose-for-scylladb`</li>
    </ul>
</dd>
<dt>service_plan（必要）</dt>
<dd>
新服務的定價方案。這裡的值必須是 `Enterprise`，才能將資料庫部署至 {{site.data.keyword.composeEnterprise}} 叢集。
</dd>
<dt>service_instance（必要）</dt>
<dd>
您要佈建之新 Compose 資料庫服務的名稱。</dd>
<dt>[-c PARAMETERS_AS_JSON]（必要）</dt>
<dd>
參數會格式化為 JSON 物件，而且應該包含下列一個值：
    <dl>
    <dt>cluster_service_instance_id</dt>
    <dd>{{site.data.keyword.composeEnterprise}} 叢集的「叢集服務實例 ID」。您可以遵循本手冊的步驟 2 來取得此值。
    </dd>
    </dl>
    <dl>
    <dt>cluster_id</dt>
    <dd>{{site.data.keyword.composeEnterprise}} 叢集的「叢集 ID」。您可以在 {{site.data.keyword.composeEnterprise}} 服務實例的儀表板中找到此值。
    </dd>
    </dl>
</dd>
</dl>

例如，若要將稱為 'myComposeForEnterpriseService' 的 {{site.data.keyword.composeForElasticsearch}} 服務部署至 {{site.data.keyword.composeEnterprise}} 叢集，其中 `cluster_service_instance_id` 是 '12345678-90ab-cdef-1234567890a'，您將使用下列指令。

```
bx cf create-service compose-for-elasticsearch Enterprise myComposeForEnterpriseService -c '{"cluster_service_instance_id": "12345678-90ab-cdef-1234567890a"}'
```

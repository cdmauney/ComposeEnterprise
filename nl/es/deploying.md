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

# Despliegue de una base de datos de Compose en un clúster de {{site.data.keyword.composeEnterprise}}

## Despliegue desde la consola de {{site.data.keyword.cloud_notm}}

Si ya ha creado un clúster de {{site.data.keyword.composeEnterprise_full}} utilizando {{site.data.keyword.cloud_notm}}, puede especificar que se proporcione la nueva base de datos de Compose {{site.data.keyword.cloud_notm}} en el clúster. Seleccione el nombre del clúster de {{site.data.keyword.composeEnterprise}} de la lista de ubicaciones disponibles en el desplegable *Seleccionar ubicación para el despliegue* cuando esté creando la nueva instancia de servicio de base de datos de Compose {{site.data.keyword.cloud_notm}}.

## Despliegue desde la línea de mandatos

Para crear una nueva instancia de un servicio de base de datos de Compose y la suministra en un clúster de {{site.data.keyword.composeEnterprise}}, desde la línea de mandatos tendrá que instalar en primer lugar la interfaz de línea de mandatos (CLI) de {{site.data.keyword.cloud_notm}} y, a continuación, obtener y utilizar el ID de clúster de {{site.data.keyword.composeEnterprise}}.

## 1. Configure la interfaz de línea de mandatos (CLI) de {{site.data.keyword.cloud_notm}} 

1. Descargue e instale la herramienta [CLI de {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/docs/cli/reference/bluemix_cli/download_cli.html).
2. Inicie sesión en {{site.data.keyword.cloud_notm}}

    ```
    bx login
    ```

3. Vaya a la organización y al espacio que desee utilizar para el nuevo servicio de base de datos de Compose.

    ```
    bx target --cf
    ```

## 2. Obtenga el ID de instancia de servicio de clúster

1. Necesitará obtener el ID de instancia de servicio de clúster para la instancia de {{site.data.keyword.composeEnterprise}}:

    ```
    bx cf service COMPOSE_ENTERPRISE_SERVICE_NAME --guid
    ```

    El mandato devuelve una serie. Este es el GUID de la instancia de servicio, y lo necesitará más adelante para obtener el ID de clúster.

## 3. Suministre una base de datos en el clúster con el mandato `create-service`:

Ahora que tiene su `cluster_service_instance_id`, puede utilizar el mandato `create-service` para crear un servicio de base de datos de {{site.data.keyword.cloud_notm}} Compose y desplegarlo en el clúster de {{site.data.keyword.composeEnterprise}}.


```
bx cf create-service service_name service_plan service_instance [-c PARAMETERS_AS_JSON]
```

Las opciones del mandato son:

<dl>
<dt>service_name (necesario)</dt>
<dd>
El nombre del servicio de base de datos de Compose. El valor debe ser uno de los siguientes: 
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
<dt>service_plan (necesario)</dt>
<dd>
El plan de precios para el nuevo servicio. El valor aquí debe ser `Enterprise` para desplegar la base de datos en un clúster de {{site.data.keyword.composeEnterprise}}.
</dd>
<dt>service_instance (necesario)</dt>
<dd>
El nombre del nuevo servicio de base de datos de Compose que desea suministrar.
</dd>
<dt>[-c PARAMETERS_AS_JSON] (necesario)</dt>
<dd>
Los parámetros están formateados como un objeto JSON y deben contener uno de los valores siguientes:
    <dl>
    <dt>cluster_service_instance_id</dt>
    <dd>El ID de instancia de servicio de clúster del clúster de {{site.data.keyword.composeEnterprise}}. Puede obtener este valor siguiendo el paso 2 de esta guía.
    </dd>
    </dl>
    <dl>
    <dt>cluster_id</dt>
    <dd>El ID de clúster del clúster de {{site.data.keyword.composeEnterprise}}. Puede encontrar este valor en el panel de control de la instancia de servicio de {{site.data.keyword.composeEnterprise}}.
    </dd>
    </dl>
</dd>
</dl>

Por ejemplo, para desplegar un servicio de {{site.data.keyword.composeForElasticsearch}} denominado 'myComposeForEnterpriseService' en un clúster de {{site.data.keyword.composeEnterprise}}, donde el `cluster_service_instance_id` sea '12345678-90ab-cdef-1234567890a', debe utilizar el mandato siguiente.

```
bx cf create-service compose-for-elasticsearch Enterprise myComposeForEnterpriseService -c '{"cluster_service_instance_id": "12345678-90ab-cdef-1234567890a"}'
```

---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-24"

keywords: satellite, hybrid, multicloud

subcollection: satellite

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Securing your connection
{: #service-connection}

With {{site.data.keyword.satellitelong_notm}}, you bring {{site.data.keyword.cloud_notm}} to your own infrastructure environment by creating a {{site.data.keyword.satelliteshort}} location. This setup means that you do not need [{{site.data.keyword.cloud_notm}} service endpoints](/docs/account?topic=account-service-endpoints-overview) to access {{site.data.keyword.cloud_notm}}. Instead, {{site.data.keyword.cloud_notm}} needs a {{site.data.keyword.satelliteshort}} Link endpoint to access your infrastructure environment. You can access services in your {{site.data.keyword.satelliteshort}} location by creating {{site.data.keyword.satelliteshort}} Link endpoints, using the cluster URL, or creating a route or similar service for workloads in a cluster.
{: shortdesc}

## User access to resources that run in your {{site.data.keyword.satelliteshort}} location
{: #user-access}

Users can access the resources that run in your {{site.data.keyword.satelliteshort}} location in several ways, depending on what users need to access: service-instance clusters in your {{site.data.keyword.satelliteshort}} location, a resource in your {{site.data.keyword.satelliteshort}} location from the IBM private network, or an application workload in a cluster in your {{site.data.keyword.satelliteshort}} location.
{: shortdesc}

### Service-instance clusters
{: #user-access-service}

A cluster service URL is automatically created for any {{site.data.keyword.satelliteshort}}-enabled services that you run in your location, such as a {{site.data.keyword.openshiftlong_notm}} cluster. These URLs allow you to access your {{site.data.keyword.cloud_notm}} service that runs in your location over the public network or from within your hosts' private network, depending on whether your location hosts have public and private or private only connectivity.
{: shortdesc}

For example, when you create a {{site.data.keyword.satellitelong_notm}} cluster, the cluster is accessible through a URL that consists one of the subdomains for your location and a port, such as `https://pacfd8bdae2d04696301d-6b64a6ccc9c596bf59a86625d8fa2202-ce00.us-east.satellite.appdomain.cloud:32200`. When you access your cluster, such as by using the `ibmcloud oc cluster config --cluster <cluster_name_or_ID> --admin` command or by getting a login token from the {{site.data.keyword.openshiftshort}} web console, this URL is automatically used for your connection to the cluster master. Note that if you use hosts that have private network connectivity only for your location, you must be connected to your hosts' private network, such as through VPN access, to connect to your cluster and access the {{site.data.keyword.openshiftshort}} web console.

For more information about connecting to services that run in your {{site.data.keyword.satelliteshort}} location by using the cluster service URL, see the documentation for that service, such as the [{{site.data.keyword.openshiftlong_notm}} documentation](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat).

### IBM private network access with {{site.data.keyword.satelliteshort}} Link
{: #user-access-loc-ep}

If you have a resource on the IBM private network that requires access to your {{site.data.keyword.satelliteshort}} location, you can [create a `location` endpoint in {{site.data.keyword.satelliteshort}} Link](/docs/satellite?topic=satellite-link-location-cloud#link-location).
{: shortdesc}

### Application workloads that run in clusters
{: #user-access-apps}

To make your apps available, see the options for [Exposing apps in {{site.data.keyword.satelliteshort}} clusters](/docs/openshift?topic=openshift-sat-expose-apps).
{: shortdesc}

## {{site.data.keyword.cloud_notm}} access to your {{site.data.keyword.satelliteshort}} location
{: #ibm-cloud-access}

Default {{site.data.keyword.satelliteshort}} Link endpoints are created for your location's control plane cluster and for any other {{site.data.keyword.satelliteshort}}-enabled services that you run in your location. These default {{site.data.keyword.satelliteshort}} Link endpoints are accessible only from within the {{site.data.keyword.cloud_notm}} private network.
{: shortdesc}

The following table describes the Link endpoints that are automatically created in your {{site.data.keyword.satelliteshort}} location.

| Name | Description | Type | Instances |
| ---- | ----------- | ---- | --------- |
| `satellite-healthcheck-<location_ID>` | Allows the {{site.data.keyword.satelliteshort}} control plane master to check the health of your location's control plane cluster. | `location` | One per location |
| `satellite-cos-<location_ID>` | Allows the control plane data of your {{site.data.keyword.satelliteshort}} location to be backed up to your {{site.data.keyword.cos_full}} instance. | `cloud` | One per location |
| `satellite-iam-<location_ID>` | Allows requests to your {{site.data.keyword.satelliteshort}} location to be authenticated and user actions to be authorized by Identity and Access Management (IAM). | `cloud` | One per location |
| `satellite-logdna-<location_ID>` | Allows logs for your {{site.data.keyword.satelliteshort}} location to be sent to your {{site.data.keyword.la_full}} instance. | `cloud` | One per location |
| `satellite-sysdig-<location_ID>` | Allows metrics for your {{site.data.keyword.satelliteshort}} location to be sent to your {{site.data.keyword.mon_full}} instance. | `cloud` | One per location |
| `satellite-sysdigapi-<location_ID>` | Allows your {{site.data.keyword.satelliteshort}} location to communicate with the {{site.data.keyword.mon_full_notm}} API. | `cloud` | One per location |
| `openshift-api-<cluster_ID>` | Allows the {{site.data.keyword.openshiftlong_notm}} API to communicate with the master for the service cluster. | `location` | One per {{site.data.keyword.satelliteshort}}-enabled service in your location |
{: caption="Default Link endpoints." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the name of the default endpoint. The second column describes what the endpoint is for. The third column describes how many instances of the endpoint are created and for which component the endpoint is created."}

Disabling these automated endpoints prevents your location from being fully managed and updated. Because these endpoints connect your location to {{site.data.keyword.cloud_notm}}, they cannot be removed.
{: important}

For more information about {{site.data.keyword.satelliteshort}} Link endpoints and what kinds of access {{site.data.keyword.cloud_notm}} has to your {{site.data.keyword.satelliteshort}} location, see [Connecting {{site.data.keyword.satelliteshort}} locations with external services using Link endpoints](/docs/satellite?topic=satellite-link-location-cloud).

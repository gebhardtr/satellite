---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-31"

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



# {{site.data.keyword.cloud_notm}} for tests
{: #ibm}

Test out an {{site.data.keyword.satellitelong}} location with virtual instances that you created in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

**Testing only**: Using {{site.data.keyword.cloud_notm}} infrastructure hosts in {{site.data.keyword.satelliteshort}} is supported only for testing, demo, or proof of concept purposes. For production workloads in your {{site.data.keyword.satelliteshort}} location, use on-premises, edge, or other cloud provider hosts. You can also create {{site.data.keyword.openshiftlong_notm}} clusters in the public cloud and add them to a {{site.data.keyword.satelliteshort}} Config cluster group to deploy the same app across your {{site.data.keyword.satelliteshort}} and {{site.data.keyword.cloud_notm}} clusters.
{: important}

## Adding {{site.data.keyword.cloud_notm}} hosts to {{site.data.keyword.satelliteshort}}
{: #ibm-host-attach}

You can create your {{site.data.keyword.satelliteshort}} location by using hosts that you added from {{site.data.keyword.cloud_notm}}. 
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-create). 

1. Follow the steps to create a [classic public virtual server](/docs/virtual-servers?topic=virtual-servers-ordering-vs-public) or a virtual server instance in a [VPC](/docs/vpc?topic=vpc-creating-virtual-servers). Make sure that you select a supported RHEL 7 operating system, configure the machine with at least 4 CPU and 16 RAM, and add a boot disk with a size of at least 100 GB. 
2. Wait for your virtual server instance to be provisioned. 
3. Get the registration script to attach hosts to your {{site.data.keyword.satellitelong_notm}} location.
   ```
   ibmcloud sat host attach --location <location_name_or_ID>
   ```
   {: pre}
4. Retrieve the IP address and ID of your machine.
    * Classic:
       ```
       ibmcloud sl vs list
       ```
       {: pre}
    * VPC:
       ```
       ibmcloud is instances
       ```
       {: pre}

5. Retrieve the credentials to log in to your virtual machine.
    * Classic:
      ```
      ibmcloud sl vs credentials <vm_ID>
      ```
      {: pre}
    * VPC:
       ```
       ibmcloud is instance-initialization-values <instance_ID>
       ```
       {: pre}

6. Copy the script from your local machine to the virtual server instance.
   ```
   scp <path_to_attachHost.sh> root@<ip_address>:/tmp/attach.sh
   ```
   {: pre}
   
7. Log in to your virtual machine. If prompted, enter the password that you retrieved earlier.
   ```
   ssh root@<ip_address>
   ```
   {: pre}
   
8. Refresh the {{site.data.keyword.redhat_notm}} packages on your machine.
   ```
   subscription-manager refresh
   ```
   {: pre}
   ```
   subscription-manager repos --enable=*
   ```
   {: pre}
   
9. Run the registration script on your machine.
   ```
   nohup bash /tmp/attach.sh &
   ```
   {: pre}
   
10. Monitor the progress of the registration script. 
    ```
    journalctl -f -u ibm-host-attach
    ```
    {: pre}
    
11. Exit the SSH session.  	
    ```
    exit
    ```
    {: pre}
    
12. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.
     
13. Assign your hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign).

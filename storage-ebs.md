---

copyright:
  years: 2020, 2021
lastupdated: "2021-03-25"

keywords: satellite storage, satellite config, satellite configurations, aws, ebs, block storage

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


# AWS EBS
{: #config-storage-ebs}

Set up [Amazon Elastic Block Storage (EBS)](https://docs.aws.amazon.com/ebs/?id=docs_gateway){: external} for {{site.data.keyword.satelliteshort}} clusters by creating a storage configuration in your location. When you assign a storage configuration to your clusters, the storage drivers of the selected storage provider are installed in your cluster.
{: shortdesc}

When you create your AWS EBS storage configuration, you provide your AWS credentials which are stored as a Kubernetes secret in the clusters that you assign your configuration to. The secret is mounted inside the CSI controller pod so that when you create a PVC by using one of the IBM-provided storage classes, your AWS credentials are used to dynamically provision an EBS instance.

The {{site.data.keyword.satelliteshort}} storage templates are currently available in beta and should not be used for production workloads.
{: beta}

To use AWS EBS storage for your apps, the {{site.data.keyword.satelliteshort}} hosts that you use for your cluster's worker nodes must reside in AWS.
{: important}

## Prerequisites
{: #aws-ebs-prereq}

To use the AWS EBS storage template, complete the following tasks:

- [Create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations).
- [Create a {{site.data.keyword.satelliteshort}} cluster](/docs/satellite?topic=openshift-satellite-clusters) that runs on compute hosts in Amazon Web Services (AWS). For more information about how to add hosts from AWS to your {{site.data.keyword.satelliteshort}} location so that you can assign them to a cluster, see [Adding AWS hosts to {{site.data.keyword.satelliteshort}}](/docs/satellite?topic=satellite-aws#aws-host-attach).
- [Add your {{site.data.keyword.satelliteshort}} cluster to a cluster group](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-groups).

## Creating an AWS EBS storage configuration
{: #sat-storage-aws-ebs}

You can use the CLI to create an AWS EBS storage configuration in your location and assign the configuration to your clusters to dynamically provision AWS EBS storage for your apps.
{: shortdesc}



Use the command line to create an AWS EBS storage configuration for your location.
{: shortdesc}

Before you begin, review and complete the [prerequisites](#aws-ebs-prereq).

1. [Create an AWS access key ID and secret access key](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html){: external} for your AWS login credentials. These credentials are needed to provision AWS EBS storage in your account. When you assign the storage configuration to your cluster, your AWS access key ID and secret access key are stored in a Kubernetes secret in your cluster.
2. Review the [AWS EBS storage configuration parameters](#sat-storage-aws-ebs-params-cli).
3. Create an AWS EBS storage configuration. Replace the variables with the parameters that you retrieved in the previous step.
   ```sh
   ibmcloud sat storage config create --name <config_name> --template-name aws-ebs-csi-driver --template-version <template_version> --param "aws-access-key=<aws_access_key>" --param "aws-secret-access-key=<aws_secret_access_key>"
   ```
   {: pre}
4. Verify that your storage configuration is created.
   ```sh
   ibmcloud sat storage config get --config <config-name>
   ```
   {: pre}

5. List your {{site.data.keyword.satelliteshort}} cluster groups and note the group that you want to use. The cluster group determines the {{site.data.keyword.satelliteshort}} clusters where you want to install the AWS EBS driver. If you do not have any cluster groups yet, or your cluster is not yet part of a cluster group, follow these [steps](/docs/satellite?topic=satellite-cluster-config#setup-clusters-satconfig-groups) to create a cluster group and add your clusters. Note that all clusters in a cluster group must belong to the same {{site.data.keyword.satelliteshort}} location.
   ```sh
   ibmcloud sat group ls
   ```
   {: pre}

6. Create a storage assignment for your cluster group. After you create the assignment, the AWS EBS driver is installed in all clusters that belong to the cluster group. Replace `<group>` with the name of your cluster group, `<config>` with the name of your storage configuration, and `<name>` with a name for your storage assignment. For more information, see the [`ibmcloud sat storage assignment create`](/docs/satellite?topic=satellite-satellite-cli-reference#cli-storage-assign-create) command.
   ```sh
   ibmcloud sat storage assignment create --group <group_name> --config <config_name> --name <assignment_name>
   ```
   {: pre}

7. Verify that your assignment is created.
   ```sh
   ibmcloud sat storage assignment ls
   ```
   {: pre}

8. Verify that the AWS EBS storage configuration resources are successfully deployed in your cluster.
   1. [Access your cluster](/docs/openshift?topic=openshift-access_cluster).
   2. List the AWS EBS driver pods in the `kube-system` namespace and verify that the status is `Running`.
      ```sh
      oc get pods -n kube-system | grep ebs
      ```
      {: pre}

      Example output:
      ```sh   
      ebs-csi-controller-7bb59dbcdd-8d866     6/6     Running   0          38s
      ebs-csi-controller-7bb59dbcdd-cbgl5     6/6     Running   0          38s
      ebs-csi-node-4hzf9                      3/3     Running   0          38s
      ebs-csi-node-8rgcs                      3/3     Running   0          38s
      ebs-csi-node-j2hc9                      3/3     Running   0          38s
      ```
      {: screen}

   3. List the AWS EBS storage classes.
      ```sh
      oc get sc | grep aws-block
      ```
      {: pre}

      Example output:
      ```sh             
      sat-aws-block-bronze      ebs.csi.aws.com    Delete          WaitForFirstConsumer   true                   2d8h
      sat-aws-block-gold        ebs.csi.aws.com    Delete          WaitForFirstConsumer   true                   2d8h
      sat-aws-block-silver      ebs.csi.aws.com    Delete          WaitForFirstConsumer   true                   2d8h
      ```
      {: screen}

## Deploying an app that uses AWS EBS storage
{: #sat-storage-ebs-deploy}

You can use the `ebs-csi-driver` to dynamically provision AWS EBS storage for the apps in your clusters.
{: shortdesc}

1. List available storage classes and choose the storage class that you want to use.
   ```sh
   oc get sc | grep ebs
   ```
   {: pre}

   To see details for a storage class, use the `oc describe sc <sc-name>` command or review the [storage class reference](#sat-ebs-sc-reference).
   {: tip}

2. Create a PVC that provisions an AWS EBS storage instance with the characteristics that are described in the storage class that you selected. The following example uses the `sat-aws-block-bronze` storage class to create an `st1` HDD AWS EBS storage instance with a size of 125 GB. For more information about this volume type, see [Hard disk drives (HDD)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#hard-disk-drives){: external}.
   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: sat-aws-block-bronze
   spec:
     accessModes:
       - ReadWriteOnce
     storageClassName: sat-aws-block-bronze
     resources:
       requests:
         storage: 125Gi
   ```
   {: codeblock}

3. Create the PVC in your cluster.
   ```sh
   oc apply -f pvc.yaml
   ```
   {: pre}

4. Verify that your PVC is created. Because all IBM-provided storage classes are configured as `WaitForFirstConsumer`, the status of your PVC remains `Pending` until you provision an app that mounts your PVC.
   ```sh
   oc get pvc
   ```
   {: pre}

   Example output:
   ```
   NAME                   STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS           AGE
   sat-aws-block-bronze   Pending                                      sat-aws-block-bronze   17s
   ```
   {: screen}

5. Create a YAML configuration file for your pod that mounts the PVC that you created. When you create this pod, the AWS EBS driver starts to fulfill your storage request by dynamically creating an AWS EBS instance in your AWS account. The following example creates an `nginx` pod that writes the current date and time to a `test.txt` file on your AWS EBS volume mount path.
   ```yaml
   apiVersion: v1
   kind: Pod
   metadata:
     name: app
   spec:
     containers:
     - name: app
       image: nginx
       command: ["/bin/sh"]
       args: ["-c", "while true; do echo $(date -u) >> /test/test.txt; sleep 5; done"]
       volumeMounts:
       - name: persistent-storage
         mountPath: /test
     volumes:
     - name: persistent-storage
       persistentVolumeClaim:
         claimName: sat-aws-block-bronze
   ```
   {: codeblock}

6. Create the pod in your cluster.
   ```
   oc apply -f pod.yaml
   ```
   {: pre}

7. Verify that the pod is deployed. Note that it might take a few minutes for the storage request to be fulfilled and for your app to get into a `Running` state.
   ```sh
   oc get pods
   ```
   {: pre}

   Example output:
   ```
   NAME                                READY   STATUS    RESTARTS   AGE
   app                                 1/1     Running   0          2m58s
   ```
   {: screen}

8. Verify that your PVC status changed to `Bound`.
   ```sh
   oc get pvc
   ```
   {: pre}

   Example output:
   ```
   NAME                 STATUS  VOLUME                                   CAPACITY   ACCESS MODES   STORAGECLASS           AGE
   sat-aws-block-bronze Bound   pvc-86d2f9f4-78d4-4bb2-ab73-39726d144981 125Gi      RWO            sat-aws-block-bronze   33m
   ```
   {: screen}

   If your PVC remains in a `Pending` status, get the details for your PVC by running the `oc describe pvc <pvc_name>` command to see the error that occurred during the provisioning of your AWS EBS storage instance.
   {: tip}

9. Verify that the app can write to your AWS EBS instance.
   1. Log in to your pod.
      ```sh
      oc exec <app_pod_name> -it bash
      ```
      {: pre}

   2. Display the contents of the `test.txt` file to confirm that your app can write data to your persistent storage.
      ```sh
      cat /test/test.txt
      ```
      {: pre}

      Example output:
      ```sh
      Tue Mar 2 20:09:19 UTC 2021
      Tue Mar 2 20:09:25 UTC 2021
      Tue Mar 2 20:09:31 UTC 2021
      Tue Mar 2 20:09:36 UTC 2021
      Tue Mar 2 20:09:42 UTC 2021
      Tue Mar 2 20:09:47 UTC 2021
      ```
      {: screen}

   3. Exit the pod.
      ```sh
      exit
      ```
      {: pre}

10. Verify that your storage instance is created in AWS.
   1. List the PV that was created for your PVC.
      ```sh
      oc get pv
      ```
      {: pre}

   2. Get the details of your PV and note the ID of your AWS EBS instance that was created in the `source.volumeHandle` field.
      ```sg
      oc describe pv <pv_name>
      ```
      {: pre}

   3. From the [AWS EC2 dashboard](https://console.aws.amazon.com/ec2/v2/home){: external}, select **Elastic Block Store** > **Volumes**.
   4. Find your AWS EBS volume by using the ID that you retrieved earlier.


<br />

## Removing AWS EBS storage from your apps
{: #aws-ebs-rm}

If you no longer need your AWS EBS instance, you can remove your PVC, PV, and the AWS EBS instance in your AWS account.
{: shortdesc}

Removing your AWS EBS instance permanently removes all the data that is stored on this instance. This action cannot be undone. Make sure that you back up your data before you delete the AWS EBS instance.
{: important}

1. List your PVCs and note the name of the PVC that you want to remove.
   ```sh
   oc get pvc
   ```
   {: pre}

2. Remove any pods that mount the PVC.
   1. List all the pods that currently mount the PVC that you want to delete. If no pods are returned, you do not have any pods that currently use your PVC.
      ```sh
      oc get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.volumes[*]}{.persistentVolumeClaim.claimName}{" "}{end}{end}' | grep "<pvc_name>"
      ```
      {: pre}

      Example output:
      ```
      app    sat-aws-block-bronze
      ```
      {: screen}

   2. Remove the pod that uses the PVC. If the pod is part of a deployment, remove the deployment.
      ```sh
      oc delete pod <pod_name>
      ```
      {: pre}

      ```sh
      oc delete deployment <deployment_name>
      ```
      {: pre}

   3. Verify that the pod or the deployment is removed.
      ```sh
      oc get pods
      ```
      {: pre}

      ```sh
      oc get deployments
      ```
      {: pre}

3. Delete the PVC. Because all IBM-provided AWS EBS storage classes are specified with a `Delete` reclaim policy, the PV and the AWS EBS instance in your AWS account are automatically deleted when you delete the PVC.
   ```sh
   oc delete pvc <pvc_name>
   ```
   {: pre}

4. Verify that your storage is removed.
   1. Verify that your PV is automatically removed.
      ```sh
      oc get pv
      ```
      {: pre}

   2. From the [AWS EC2 dashboard](https://console.aws.amazon.com/ec2/v2/home){: external}, select **Elastic Block Store** > **Volumes** and verify that your AWS EBS instance is removed.

<br />

## Removing the AWS EBS storage configuration from your cluster
{: #aws-ebs-template-rm}

If you no longer plan on using AWS EBS storage in your cluster, you can unassign your cluster from the storage configuration.
{: shortdesc}

Removing the storage configuration, uninstalls the AWS EBS driver from all assigned clusters. Your PVCs, PVs and data are not removed. However, you might not be able to access your data until you re-install the driver in your cluster again.
{: important}



Use the CLI to remove a storage configuration.
{: shortdesc}

1. List your storage assignments and find the one that you used for your cluster.
   ```sh
   ibmcloud sat storage assignment ls
   ```
   {: pre}

2. Remove the assignment. After the assignment is removed, the AWS EBS driver pods and storage classes are removed from all clusters that were part of the storage assignment.
   ```sh
   ibmcloud sat storage assignment rm --assignment <assignment_name>
   ```
   {: pre}

3. Verify that the AWS EBS driver is removed from your cluster.
   1. List of the storage classes in your cluster and verify that the AWS EBS storage classes are removed.
      ```sh
      oc get sc
      ```
      {: pre}

   2. List the pods in the `kube-system` namespace and verify that the AWS EBS storage driver pods are removed.
      ```sh
      oc get pods -n kube-system | greb ebs
      ```
      {: pre}

   3. List the secrets in the `kube-system` namespace and verify that the AWS secret that stored your AWS credentials is removed.
      ```sh
      oc get secrets -n kube-system | grep aws
      ```
      {: pre}

4. Optional: Remove the storage configuration.
   1. List the storage configurations.
      ```sh
      ibmcloud sat storage config ls
      ```
      {: pre}

   2. Remove the storage configuration.
      ```sh
      ibmcloud sat storage config rm --config <config_name>
      ```
      {: pre}


## AWS EBS storage configuration CLI parameter reference
{: #sat-storage-aws-ebs-params-cli}

Review the AWS EBS storage configuration parameters.
{: shortdesc}

| Parameter | Required? | Description |
| --- | --- | --- |
| `--name` | Required | Enter a name for your storage configuration. |
| `--template-name` | Required | Enter `aws-ebs-csi-driver`. |
| `--template-version` | Required | Enter the version of the `aws-ebs-csi-driver` template that you want to use. To get a list of storage templates and versions, run `ibmcloud sat storage template ls`. |
| `aws-access-key` | Required | Enter your AWS access key. For more information about how retrieve this value, see the [AWS IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html){: external}. |
| `aws-secret-access-key` | Required | Enter your AWS secret access key. For more information about how retrieve this value, see the [AWS IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html){: external}. |
{: caption="Table 1. AWS EBS parameter reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the parameter name. The second column is a brief description of the parameter. The third column is the default value of the parameter."}

## Storage class reference
{: #sat-ebs-sc-reference}

Review the {{site.data.keyword.satelliteshort}} storage classes for AWS EBS. You can describe storage classes in the command line with the `oc describe sc <storage-class-name>` command. You can also view the YAML spec for the EBS storage classes in [GitHub](https://github.com/IBM/ibm-satellite-storage/blob/develop/config-templates/aws/aws-ebs-csi-driver/0.8.2/storage-class.yaml).
{: shortdesc}

| Storage class name | EBS volume type | File system type | Provisioner | Default IOPS per GB | Size range | Hard disk | Encrypted? | Volume binding mode | Reclaim policy | More info | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `sat-aws-block-gold` | io2 | ext4 | `ebs.csi.aws.com` | 10 | 10 GiB - 6.25 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external}
| `sat-aws-block-silver` | gp3 | ext4 | `ebs.csi.aws.com` | N/A | 1 GiB - 16 TiB | SSD | True | WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#solid-state-drives){: external} |
| `sat-aws-block-bronze` | st1 | ext4 | `ebs.csi.aws.com` | N/A | 125 GiB - 16 TiB | HDD | True |  WaitforFirstConsumer | Delete | [Link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volume-types.html#hard-disk-drives){: external} |
{: caption="Table 2. AWS EBS storage class reference." caption-side="top"}
{: summary="The rows are read from left to right. The first column is the storage class name. The second column is the volume type. The third column is the file system type. The fourth column is the provisioner. The fifth column is the default IOPs per GB. The size column is the supported size range. The seventh column is disk type. The eigth column is encryption support. The ninth column is the volume binding mode. The tenth column is the reclaim policy."}

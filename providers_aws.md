---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-15"

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



# Amazon Web Services (AWS)
{: #aws}

Review the following host requirements that are specific to hosts that are in the Amazon Web Services (AWS) cloud. For required access in AWS cloud, see [AWS permissions](/docs/satellite?topic=satellite-iam#permissions-aws).
{: shortdesc}

Want to try an automated setup? See [Automating your location setup with a Schematics template](/docs/satellite?topic=satellite-locations#satloc-template).
{: tip}

## Adding AWS hosts to {{site.data.keyword.satelliteshort}}
{: #aws-host-attach}

You can create your {{site.data.keyword.satellitelong_notm}} location by using hosts that you added from Amazon Web Services (AWS) cloud.
{: shortdesc}

All hosts that you want to add must meet the general host requirements, such as the RHEL 7 packages and networking setup. For more information, see [Host requirements](/docs/satellite?topic=satellite-host-reqs).
{: note}

Before you begin, [create a {{site.data.keyword.satelliteshort}} location](/docs/satellite?topic=satellite-locations#location-create).

1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, select the location where you want to add AWS hosts.
2. Retrieve the host registration script that you must run on your hosts to make them visible to your {{site.data.keyword.satellitelong_notm}} location.
   1. From the **Hosts** tab, click **Attach host**.
   2. Optional: Enter any host labels that are used later to [automatically assign hosts to {{site.data.keyword.satelliteshort}}-enabled services](/docs/satellite?topic=satellite-hosts#host-autoassign-ov) in the location. Labels must be provided as key-value pairs, and must match the request from the service. For example, you might have host labels such as `env=prod` or `service=database`. By default, your hosts get a `cpu` label, but you might want to add more to control the autoassignment, such as `env=prod` or `service=database`.
   3. Enter a file name for your script or use the name that is generated for you.
   4. Click **Download script** to generate the host script and download the script to your local machine.
3. Open the registration script. After the `API_URL` line, add a section to pull the required RHEL packages with the subscription manager.
   ```
   # Enable AWS RHEL package updates
   yum update -y
   yum-config-manager --enable '*'
   yum repolist all
   yum install container-selinux -y
   echo "repos enabled"
   ```
   {: codeblock}
4. From the [AWS EC2 dashboard](https://console.aws.amazon.com/ec2/v2/home){: external}, go to **Instances** > **Launch Templates**.
5. Click **Create Launch template** and enter the template details as follows.

   For an overview of available options that you can specify in your launch template, see the [AWS documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-templates.html#create-launch-template-define-parameters){: external}
   {: tip}

   1. Enter a name for your launch template.
   2. In the **Amazon machine image (AMI)** section, make sure to select a supported Red Hat Enterprise Linux 7 operating system, such as RHEL 7.7 that you can find by entering the AMI ID `ami-0170fc126935d44c3`.
   3. From the **Instance type** section, select one of the [supported AWS instance types](#aws-instance-types).
   4. From the **Key pair (login)** section, select the pem key that you want to use to log in to your machines later. If you do not have a pem key, create one.
   5. In the **Network settings**, select **Virtual Private Cloud (VPC)** and an existing subnet and a security group that allows network traffic as defined in [Security group settings](#aws-reqs-secgroup). If you do not have a subnet or security group that you want to use, create one.
   6. In the **Storage (volumes)** section, expand the default root volume and update the size of the boot volume to a minimum of 100 GB.
   7. Expand the **Advanced details** and go to the **User Data** field.
   8. Enter the host registration script that you modified earlier.
   9. Click **Create launch template**.
6. From the **Launch Templates** dashboard, find the template that you created.
7. From the **Actions** menu, select **Launch instance from template**.
8. Enter the number of instances that you want to create and click **Launch instance from template**.
9. Wait for the instance to launch. During the launch of your instance, the registration script runs automatically. This process takes a few minutes to complete.
10. Monitor the progress of the registration script.
    1. From the EC2 **Instances** dashboard, retrieve the public IP address of your instance.
    2. Log in to your instance.
       ```
       ssh -i <key>.pem ec2-user@<public_IP_address>
       ```
       {: pre}

    3. Review the status of the registration script.
       ```
       journalctl -f -u ibm-host-attach
       ```
       {: pre}  
11. Check that your hosts are shown in the **Hosts** tab of your [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}. All hosts show a **Health** status of `Ready` when a connection to the machine can be established, and a **Status** of `Unassigned` as the hosts are not yet assigned to your {{site.data.keyword.satelliteshort}} location control plane or a {{site.data.keyword.openshiftlong_notm}} cluster.   
12. Assign your AWS hosts to the [{{site.data.keyword.satelliteshort}} control plane](/docs/satellite?topic=satellite-locations#setup-control-plane) or a [{{site.data.keyword.openshiftlong_notm}} cluster](/docs/satellite?topic=satellite-hosts#host-assign).


## Supported AWS instance types
{: #aws-instance-types}

Review the following [AWS EC2 instance types](https://aws.amazon.com/ec2/instance-types/){: external} that you can use as hosts in {{site.data.keyword.satellitelong_notm}}.
{: shortdesc}

| Instance | vCPU | Memory (GiB) | Storage disk (GiB) | Network bandwidth (Gbps) |
| -------- | ---- | ------------ | ------------------ | ------------------------ |
| m5d.xlarge | 4 | 16 | At least 100 GB SSD attached | Up to 10 |
| m5d.2xlarge | 8 | 32 | At least 100 GB SSD attached | Up to 10 |
| m5d.4xlarge | 16 | 64 | At least 100 GB SSD attached | Up to 10 |
{: caption="Supported AWS instance types" caption-side="top"}
{: summary="The rows are read from left to right. The first column is the name of the supported instance. The second column is the number of vCPUs. The third column is the memory in gibibytes (GiB). THe fourth column is the number of storage disks and their size in gibibytes (GiB). The fifth column is the network bandwidth in gigabits per second (Gbps)."}

## Security group settings
{: #aws-reqs-secgroup}

As described in the [host networking requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network), your AWS hosts must have access to connect to {{site.data.keyword.satellitelong_notm}}. If you use hosts in a virtual private cloud (VPC), you can create a security group similar to the following example. You can get the owner, group, user, and VPC IDs from your AWS provider resources.
{: shortdesc}

**Example security group for AWS**
```
{
	"Description": "Security group for IBM Cloud Satellite hosts",
	"GroupName": "Satellite",
	"IpPermissions": [{
			"FromPort": 80,
			"IpProtocol": "tcp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [],
			"PrefixListIds": [],
			"ToPort": 80,
			"UserIdGroupPairs": []
		},
		{
			"FromPort": 30000,
			"IpProtocol": "tcp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [{
				"CidrIpv6": "::/0"
			}],
			"PrefixListIds": [],
			"ToPort": 32767,
			"UserIdGroupPairs": []
		},
		{
			"IpProtocol": "-1",
			"IpRanges": [],
			"Ipv6Ranges": [],
			"PrefixListIds": [],
			"UserIdGroupPairs": [{
				"GroupId": "<group_ID>",
				"UserId": "<user_ID>"
			}]
		},
		{
			"FromPort": 22,
			"IpProtocol": "tcp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [],
			"PrefixListIds": [],
			"ToPort": 22,
			"UserIdGroupPairs": []
		},
		{
			"FromPort": 30000,
			"IpProtocol": "udp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [{
				"CidrIpv6": "::/0"
			}],
			"PrefixListIds": [],
			"ToPort": 32767,
			"UserIdGroupPairs": []
		},
		{
			"FromPort": 443,
			"IpProtocol": "tcp",
			"IpRanges": [{
				"CidrIp": "0.0.0.0/0"
			}],
			"Ipv6Ranges": [{
				"CidrIpv6": "::/0"
			}],
			"PrefixListIds": [],
			"ToPort": 443,
			"UserIdGroupPairs": []
		}
	],
	"OwnerId": "<owner_ID>",
	"GroupId": "<group_ID>",
	"IpPermissionsEgress": [{
		"IpProtocol": "-1",
		"IpRanges": [{
			"CidrIp": "0.0.0.0/0"
		}],
		"Ipv6Ranges": [],
		"PrefixListIds": [],
		"UserIdGroupPairs": []
	}],
	"VpcId": "<vpc_ID>"
}
```
{: screen}

## Access to {{site.data.keyword.satelliteshort}} clusters and the {{site.data.keyword.openshiftshort}} web console
{: #aws-reqs-console-access}

The private IP addresses of your instances are automatically registered and added to your location's DNS record and the cluster's subdomain. This setup prevents users that are not connected to your hosts' private network from accessing the cluster from a local machine or opening the {{site.data.keyword.openshiftshort}} web console. You must be connected to your hosts' private network, such as through VPN access, to [connect to your cluster and access the {{site.data.keyword.openshiftshort}} web console](/docs/openshift?topic=openshift-access_cluster#access_cluster_sat_se). Alternatively, if your hosts have public network connectivity, you can test access to your cluster by changing your cluster's and location's DNS records to [use your hosts' public IP addresses](/docs/openshift?topic=openshift-access_cluster#sat_public_access).

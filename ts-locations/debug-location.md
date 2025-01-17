---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-15"

keywords: satellite, hybrid, multicloud

subcollection: satellite
content-type: troubleshoot

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


# Location error messages
{: #ts-locations-debug}

By default, {{site.data.keyword.satellitelong_notm}} monitors the health of your locations and tries to resolve issues automatically for you. For issues that cannot be resolved automatically, you can debug the location by reviewing the provided health information.
{: shortdesc}

## Reviewing error messages and logs
{: #review-messages-logs}

1.  View your locations in the console or list your locations in the CLI, and review the **Status**. If the status is not healthy, continue to the next step. For more information, see [Viewing location health](/docs/satellite?topic=satellite-monitor#location-health).
    ```
    ibmcloud sat location ls
    ```
    {: pre}

    Example output:
    ```
    Name         ID                     Status            Ready   Created      Hosts (used/total)   Managed From   
    Port-North   aaaaa1a11aaaaaa111aa   action required   no      6 days ago   3 / 5                Washington DC  
    ```
    {: screen}

2.  Get the details for your location, and review the **Message**. From the console, you can click the location and hover over the tooltip in the title with the location name and health.
    ```
    ibmcloud sat location get --location <location_name_or_ID>
    ```
    {: pre}

    Example output:
    ```
    Name:                           Port-NewYork   
    ID:                             aaaaa1a11aaaaaa111aa   
    Created:                        2020-06-05 13:50:58 -0400 (6 days ago)   
    Creator:                        name@email.com   
    Managed From:                   Washington DC   
    State:                          action required   
    Ready for deployments:          no   
    Message:                        R0015: Could not assign hosts because no hosts are available. Attach more hosts to the location and try again. For more information, see the docs: 'http://ibm.biz/sat-loc'
    ```
    {: screen}

3.  Get more details about the error message and affected components by [setting up {{site.data.keyword.la_short}} to review {{site.data.keyword.satelliteshort}} location logs](/docs/satellite?topic=satellite-get-help#review-logs).

4.  Review the following common location messages for steps to resolve the issue.

## R0001: Ready location
{: #R0001}

**Location message**

The {{site.data.keyword.satelliteshort}} location is ready for operations.

**Steps to resolve**

Your {{site.data.keyword.satelliteshort}} location has no critical alerts, and the IBM monitoring component in the location control plane is monitoring the health of your location. You might still see some warning messages for actions that you can take to improve the state of resources in your location, such as hosts.

## R0002, R0018, R0020, R0023, R0029, R0037, R0039, R0042: Wait for location to be ready
{: #R0002}

**Location message**

R0002 The {{site.data.keyword.satelliteshort}} location has issues that {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later.

R0018 {{site.data.keyword.satelliteshort}} is attempting to recover.
{: #R0018}

R0020 Wait while {{site.data.keyword.satelliteshort}} completes a recovery action.
{: #R0020}

R0023 Wait while {{site.data.keyword.satelliteshort}} sets up the location control plane.
{: #R0023}

R0029 Successfully initiated recovery action.
{: #R0029}

R0037 The {{site.data.keyword.satelliteshort}} location has clusters that are in a failed state. {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later.
{: #R0037}

R0039 The {{site.data.keyword.satelliteshort}} location control plane is currently unhealthy. {{site.data.keyword.cloud_notm}} Support is working to resolve. Check back later.
{: #R0039}

R0042 {{site.data.keyword.cloud_notm}} Support is resolving link api failures. Check back later. If this issue persists, raise a support case.
{: #R0042}

**Steps to resolve**

Check back later to see if the issue is resolved. If the issue persists for a while, you can [open a support case](/docs/satellite?topic=satellite-get-help).

For more details on the issue:
1.  [Set up {{site.data.keyword.la_short}} for {{site.data.keyword.satelliteshort}} location platform logs](/docs/satellite?topic=satellite-health#setup-la).
2.  Search the platform logs for the error code for more details, such as failed API method due to a permissions error.
3.  If the details indicate a permission error:
    1.  As the account administrator, log in to the {{site.data.keyword.cloud_notm}} CLI and target the resource group and region that the location is in.
        ```
        ibmcloud login -g <resource_group> -r <region>
        ```
        {: pre}
    2.  Reset the API key that is used for permissions.
        ```
        ibmcloud ks api-key reset
        ```
        {: pre}

## R0009: Unable to recover
{: #R0009}

**Location message**

R0009 {{site.data.keyword.satelliteshort}} is unable to recover from issues.


**Steps to resolve**

{{site.data.keyword.satelliteshort}} tried unsuccessfully to automatically resolve the issue. Review any further messages to troubleshoot further, such as adding more hosts to the location. If the issue persists for a while, you can [open a support case](/docs/satellite?topic=satellite-get-help).

## R0010, R0030, R0031, R0032: Control plane needs hosts
{: #R0010}

**Location message**

R0010 Assign more hosts to the location control plane, or replace unhealthy hosts.

R0030 A zone for the {{site.data.keyword.satelliteshort}} location control plane is reaching a critical capacity. If critical capacity is reached, you cannot add more clusters to location. Add more hosts to the control plane zone, or replace unhealthy hosts.
{: #R0030}

R0031 A zone for the {{site.data.keyword.satelliteshort}} location control plane is reaching a warning capacity. Add more hosts to the control plane zone, or replace unhealthy hosts.
{: #R0031}

R0032 Manually assign hosts to the control plane across all 3 zones.
{: #R0032}

**Steps to resolve**

Your location has no available hosts for {{site.data.keyword.satelliteshort}} to automatically assign to the location control plane, and might be reaching capacity limits. You can choose among the following options:
*   [Attach](/docs/satellite?topic=satellite-hosts#attach-hosts) and [assign](/docs/satellite?topic=satellite-locations#setup-control-plane) more hosts to the location control plane. Keep in mind that when you scale up the location control plane, scale evenly in multiples of 3, and assign the hosts evenly across zones.
*   [Remove](/docs/satellite?topic=satellite-hosts#host-remove) and [reattach the host](/docs/satellite?topic=satellite-hosts#attach-hosts).

## R0011, R0040, R0041: Issues with the control plane hosts
{: #R0011}

**Location message**

R0011 Make sure that all hosts for your {{site.data.keyword.satelliteshort}} location are in a normal state. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.

R0040 The {{site.data.keyword.satelliteshort}} location data plane is currently unhealthy. To debug the host, see 'http://ibm.biz/sat-host-debug'. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.
{: #R0040}

R0041 Unknown issues are detected with the {{site.data.keyword.satelliteshort}} location control plane hosts. Ensure that hosts meet the minimum requirements, http://ibm.biz/sat-host-reqs. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.
{: #R0041}

**Steps to resolve**

1.  Check the **Status** of your hosts.
    ```
    ibmcloud sat host ls --location <location_name_or_ID>
    ```
    {: pre}
2.  If you have no hosts, [attach](/docs/satellite?topic=satellite-hosts#attach-hosts) hosts to your location.
3.  Make sure that you have at least 6 hosts (2 hosts per zone across 3 zones) that are assigned to the **infrastructure** cluster for the location, to run location control plane operations.
4.  If your hosts have no status and are unassigned, [log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).
5.  Review the host status to [resolve the host issue](/docs/satellite?topic=satellite-ts-hosts-debug).

## R0012: Hosts are needed in all 3 zones
{: #R0012}

**Location message**

R0012 The location control plane does not have hosts in all 3 zones. Add available hosts to your location for the control plane.

**Steps to resolve**

If you just assigned hosts to the control plane, wait a while for the bootstrapping process to complete. Otherwise, [assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones for the location itself, to run control plane operations.
*   If you did assign at least 2 hosts in each of the 3 zones, check the CPU and memory size of the hosts. The hosts must have at least 4 vCPU and 16 memory.
*   If you did assign at least 2 hosts per zone, make sure that the [hosts meet the minimum requirements](/docs/satellite?topic=satellite-host-reqs) to use in {{site.data.keyword.satelliteshort}}, such as operating system and networking configuration.
*   If you did assign at least 2 hosts in each of the 3 zones but the bootstrapping process failed, [log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).


## R0013: Unavailable zone
{: #R0013}

**Location message**

R0013 A zone in the location control plane is unavailable. Attach more hosts to the location and assign the hosts to the zone, or replace unhealthy hosts.

**Steps to resolve**

[Assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least 2 hosts to each of the 3 zones for the location itself, to run control plane operations. If you did assign at least 2 hosts in each of the 3 zones:
1.  Check the CPU and memory size of the hosts. The hosts must have at least 4 vCPU and 16 memory.
2.  Make sure that the [hosts meet the minimum requirements](/docs/satellite?topic=satellite-host-reqs) to use in {{site.data.keyword.satelliteshort}}, such as operating system and networking configuration.
3.  [Log in to debug the host machines](/docs/satellite?topic=satellite-ts-hosts-login).
4.  [Remove](/docs/satellite?topic=satellite-hosts#host-remove) and [reattach the host](/docs/satellite?topic=satellite-hosts#attach-hosts). When you remove a host, the host is unassigned from the location control plane, and you must assign another host to the zone.

## R0014: DNS record for control plane
{: #R0014}

**Location message**

R0014 Verify that the {{site.data.keyword.satelliteshort}} location has a DNS record for load balancing requests to the location control plane.

**Steps to resolve**

1.  Verify that all hosts in your {{site.data.keyword.satelliteshort}} control plane show a **State** of `assigned` and a **Status** of `Ready`.
    ```
    ibmcloud sat host ls --location <location_ID_or_name>
    ```
    {: pre}
2.  If all hosts show the correct state and status, the DNS record for your location is not yet created. This process can take up to 30 minutes to complete after all hosts are successfully assigned to your location.
3.  If one or more hosts do not show the correct state or status, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).

## R0015, R0016: Host issues
{: #R0015}

**Location message**

R0015 Could not assign hosts because no hosts are available. Attach more hosts to the location and try again. For more information, see the docs: 'http://ibm.biz/sat-loc'

R0016 Unexpected error occurred after assigning host. To debug the host, see 'http://ibm.biz/sat-host-debug'. If you still have issues, contact {{site.data.keyword.cloud_notm}} Support and include your {{site.data.keyword.satelliteshort}} location ID.
{: #R0016}

**Steps to resolve**

[Attach more hosts](/docs/satellite?topic=satellite-hosts#attach-hosts) to the location. If you attached hosts that are not showing up as available, see [Debugging host health](/docs/satellite?topic=satellite-ts-hosts-debug).

## R0024, R0025, R0038: Cluster issues
{: #R0024}

**Location message**

R0024 The {{site.data.keyword.satelliteshort}} location has {{site.data.keyword.openshiftshort}} clusters in warning health.

R0025 The {{site.data.keyword.satelliteshort}} location has {{site.data.keyword.openshiftshort}} clusters in critical health.
{: #R0025}

R0038 The {{site.data.keyword.satelliteshort}} location has clusters in the middle of an operation. Wait for them to finish and check back later.
{: #R0038}

**Steps to resolve**

1.  Wait to see if another message is returned, such as a message about host capacity.
2.  If a host message is returned, try [Debugging hosts](/docs/satellite?topic=satellite-ts-hosts-debug).
3.  If no further message is returned, try [Debugging your {{site.data.keyword.openshiftlong_notm}} clusters](/docs/openshift?topic=openshift-cs_troubleshoot).

## R0026: Host disk space
{: #R0026}

**Location message**

R0026 Hosts in the location control plane are running out of disk space. Assign more hosts to the location control plane, or reload the hosts with disk space issues.

**Steps to resolve**

1.  List the hosts that are assigned to the control plane.
    ```
    ibmcloud sat host ls --location <location_name_or_ID> | grep infrastructure
    ```
    {: pre}
2.  Check the details of the hosts.
    ```
    ibmcloud sat host get --host <host_ID> --location <location_name_or_ID>
    ```
    {: pre}
3.  In the infrastructure provider for the host, check the disk space of your host machine. Each host must have at least 20 GB disk available. [Remove](/docs/satellite?topic=satellite-hosts#host-remove) and [reattach the host](/docs/satellite?topic=satellite-hosts#attach-hosts).
4.  If debugging and reattaching the host do not resolve the issue, the location control plane needs more compute resources to continue running. [Assign more hosts to the location control plane](/docs/satellite?topic=satellite-locations#setup-control-plane).

## R0033, R0034, R0035: Control plane capacity issues
{: #R0033}

**Location message**

R0033 Hosts in the location control plane have critical memory usage issues. Add more hosts to the location control plane and wait for the location to return to normal.

R0034 Hosts in the location control plane have critical CPU usage issues. Add more hosts to the location control plane and wait for the location to return to normal.
{: #R0034}

R0035 The location control plane is running at max capacity and cannot support any more workloads. Add hosts to each zone and wait for the location to return to normal.
{: #R0035}

**Steps to resolve**

1.  In each zone, check the CPU and memory size of the hosts.
  * Across all hosts in a zone, at least 3 CPU total must be available.
  * Across all hosts in a zone, at least 4 GB memory total must be available.
2.  [Attach 3 more hosts to the location](/docs/satellite?topic=satellite-hosts#attach-hosts).
3.  [Assign](/docs/satellite?topic=satellite-locations#setup-control-plane) at least one host to each of the three zones to add capacity for control plane operations. Keep in mind that when you scale up the location control plane, scale evenly in multiples of 3, and assign the hosts evenly across zones.

## R0036: Location subdomain traffic routing
{: #R0036}

**Location message**

R0036 The location subdomains are not correctly routing traffic to your control plane hosts. Verify that the location subdomains are registered with the correct IP addresses for your control plane hosts with the 'ibmcloud sat location dns' commands.

**Steps to resolve**

See [Why does the location subdomain not route traffic to control plane hosts?](/docs/satellite?topic=satellite-ts-location-subdomain).

## R0043: Layer 3 connectivity
{: #R0043}

**Location message**

R0043 The location does not meet the following requirement: Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. If you still have issues, contact IBM Cloud Support and include your Satellite location ID.

**Steps to resolve**

Hosts must have TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts. You cannot block access to certain ports that might block communication across hosts. Review [Host network requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network) and unblock the ports on the host in your infrastructure provider.

To test TCP/UDP/ICMP Layer 3 connectivity for all ports across hosts:
1.  SSH into a host that is attached to your location but that is not assigned to any resources.

    You can only SSH into the machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host via SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-hosts#host-remove) and reload the operating system to restore the ability to SSH into the machine.
    {: note}

2.  To check TCP connectivity, verify that `netcat` receives a response from all other hosts on port `10250`. If the operation times out, review [Host network requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network) to unblock the ports on the host in your infrastructure provider.
  ```
  nc -zv <host_IP> 10250
  ```
  {: pre}

3. To check ICMP connectivity, verify that a ping is successful to all other hosts. Repeat this step for each IP address of the hosts that are attached to your location. If the ping times out, review [Host network requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network) to unblock the ports on the host in your infrastructure provider.
  ```
  ping <host_IP>
  ```
  {: pre}

## R0044: DNS issues
{: #R0044}

**Location message**

R0044 DNS issues have been detected on one or more hosts. Verify that your DNS solution is working as expected. If you still have issues, contact IBM Cloud Support and include your Satellite location ID.

**Steps to resolve**

One or more hosts in your locations is unable to resolve DNS queries or a search domain is causing unexpected issues. Verify that your DNS solution is working as expected and that all hosts meet the [network host requirements](/docs/satellite?topic=satellite-host-reqs#reqs-host-network).

To test DNS resolution:
1.  SSH into a host that is attached to your location but that is not assigned to any resources.

    You can only SSH into the machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host via SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-hosts#host-remove) and reload the operating system to restore the ability to SSH into the machine.
    {: note}

2.  Ensure that DNS resolution is working properly.
    ```
    dig +short +timeout=5 +nocookie cloud.ibm.com
    ```
    {: pre}

3.  Ensure that `localhost` with any appended search domains in your DNS configuration either do not resolve to anything, or resolve only to `127.0.0.1`. In the `/etc/resolv.conf` file that manages DNS resolution for each host, multiple search domains, such as `search ibm.com`, might be listed. Calico Typha pods on each host run a health check that uses `localhost` resolution. However, some search domains might be appended when the health check attempts to resolve `localhost`, which causes the health check to fail. To ensure that the health check can run properly, make sure that none of the listed search domains resolve to anything other than the `127.0.0.1` IP address when appended to `localhost`.

## R0045: Host read-only file system issues
{: #R0045}

**Location message**

R0045 A read-only file system has been detected on one or more hosts. Replace the affected host(s).

**Steps to resolve**

1.  [Set up {{site.data.keyword.la_short}} for {{site.data.keyword.satelliteshort}} location platform logs](/docs/satellite?topic=satellite-health#setup-la) for more information about which hosts are affected.
2.  [Remove](/docs/satellite?topic=satellite-hosts#host-remove) the affected hosts and [reattach new hosts](/docs/satellite?topic=satellite-hosts#attach-hosts).
3.  If you still have issues, [open a support case](/docs/satellite?topic=satellite-get-help) and include your {{site.data.keyword.satelliteshort}} location ID.

## R0046: NTP issues
{: #R0046}

**Location message**

R0046 An NTP issue has been detected on one or more hosts. Verify that your NTP solution is working as expected.

**Steps to resolve**

One or more hosts in your locations has Network Time Protocol (NTP) issues that must be resolved.

To test NTP on your hosts:
1.  SSH into a host that is attached to your location but that is not assigned to any resources.

    You can only SSH into the machine if you did not assign the host to a cluster, or if the assignment failed. Otherwise, {{site.data.keyword.satelliteshort}} disables the ability to log in to the host via SSH for security purposes. You can [remove the host](/docs/satellite?topic=satellite-hosts#host-remove) and reload the operating system to restore the ability to SSH into the machine.
    {: note}

2.  Ensure that the time that is reported by the host does not differ from the actual time by more than 3 minutes. If the time differs by more than 3 minutes, verify your NTP solution with your infrastructure provider.
    ```
    date +%s
    ```
    {: pre}

3. Repeat these steps to identify any hosts that have NTP issues.

## R0047: Location health checking
{: #R0047}

**Location message**

R0047 IBM Cloud is unable to use the health check endpoint to check the location's health.

**Steps to resolve**

See [Why is {{site.data.keyword.cloud_notm}} unable to check my location's health?](/docs/satellite?topic=satellite-ts-location-healthcheck).

## R0048: Etcd backup failure
{: #R0048}

**Location message**

```
R0048 The etcd backup for a cluster in your location failed to complete within the past day.
```
{: screen}

**Steps to resolve**

Etcd data is backed up every 8 hours from your {{site.data.keyword.satelliteshort}} location control plane to a bucket in your {{site.data.keyword.cos_full_notm}} instance. If this backup consecutively fails 3 times over 24 hours, problems might exist with the {{site.data.keyword.cos_short}} bucket or service instance, or with your {{site.data.keyword.satelliteshort}} location's connection to the {{site.data.keyword.cos_short}} instance.

1. Ensure that the hosts that you assigned to the location control plane are able to access the {{site.data.keyword.cos_full_notm}} endpoint for the {{site.data.keyword.cloud_notm}} region that your location is managed from. For example, in your host firewall, you must allow outbound connectivity from your control plane hosts to the following endpoints:

    <table summary="The table shows the required outbound connectivity for hosts to the {{site.data.keyword.cos_short}} endpoint for the region. Rows are to be read from the left to right. The region that your {{site.data.keyword.satelliteshort}} location is managed from is in the first column. The {{site.data.keyword.cos_short}} endpoint for that region is in the second column.">
    <caption>Required outbound connectivity for hosts to {{site.data.keyword.cos_short}} endpoints</caption>
    <table>
    <thead>
    <tr>
    <th>Region</th>
    <th>{{site.data.keyword.cos_short}} endpoint</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td><code>wdc</code></td>
    <td><code>s3.us.cloud-object-storage.appdomain.cloud</code></td>
    </tr>
    <tr>
    <td><code>lon</code></td>
    <td><code>s3.eu.cloud-object-storage.appdomain.cloud</code></td>
    </tr>
    </tbody>
    </table>

2.  Verify that the {{site.data.keyword.cos_short}} service instance and bucket that back up your etcd data are available and were not deleted.
    1. From the [{{site.data.keyword.satelliteshort}} console](https://cloud.ibm.com/satellite/locations){: external}, click the name of your location.
    2. In the details section of your location overview, copy the name of the **{{site.data.keyword.cos_short}} bucket**.
    3. In the {{site.data.keyword.cloud_notm}} console, navigate to your [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources){: external}.
    4. Expand the **Storage** row.
    5. Look for the {{site.data.keyword.cos_short}} instance where you created the bucket. If you did not specify a bucket name during location creation, check each {{site.data.keyword.cos_short}} instance until you find the auto-generated bucket for your location.
    6. Click the instance's name. The **Buckets** list page opens.
    7. Verify that the bucket for your control plane etcd backup exists.
    8. If either the service instance or bucket were deleted, [open a support case](/docs/satellite?topic=satellite-get-help) and include your {{site.data.keyword.satelliteshort}} location ID.

3. If the control plane hosts can reach the {{site.data.keyword.cos_short}} endpoint, and the {{site.data.keyword.cos_short}} service instance and bucket exist, [open a support case](/docs/satellite?topic=satellite-get-help) to investigate backup failures and include your {{site.data.keyword.satelliteshort}} location ID.

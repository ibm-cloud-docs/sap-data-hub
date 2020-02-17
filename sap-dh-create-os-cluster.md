---

copyright:
  years: 2020
lastupdated: "2020-02-17"

keywords: SAP Data Hub, {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.cos_short}}, {{site.data.keyword.openshiftlong_notm}}, {{site.data.keyword.openshiftshort}}, Red Hat Enterprise Linux, SAP Data Hub on {{site.data.keyword.cloud_notm}}, data orchestration, data governance, data integration

subcollection: sap-data-hub

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:note: .note}
{:tip: .tip}

# Creating the OpenShift cluster and the jump host
{: #create-os-cluster}

This topic takes you through the steps of creating the {{site.data.keyword.openshiftlong}} cluster and your jump host.
{: shortdesc}

## Before you begin
{: #before-you-begin-create-os-cluster}

You need to determine the flavor of your worker nodes within your {{site.data.keyword.openshiftshort}} cluster. Flavor, on the portal order form, describes the compute resources, such as CPU or vCPU, memory, and disk capacity that you order when provisioning your worker node. Worker nodes of the same flavor are grouped in worker node pools. The total number of worker nodes in a cluster determine the compute capacity that is available to your apps in the cluster. For more information, see [Planning your worker node setup](/docs/openshift?topic=openshift-planning_worker_nodes).

Make sure that the flavor meets SAP's sizing recommendations and your expected workload characeterisitcs, as well as the expected data volumes. The flavor used in the following scenario is appropriate for a test environment. Use it for your own tests or adapt it for your needs. Refer to the most recent SAP documentation when creating your cluster. The following implementation scenario is based on [SAP Data Hub 2.7.3](https://help.sap.com/viewer/1f833eab23244ef2ad66fe982dd14873/2.7.latest/en-US/d771891d749d425ba92603ec9b0084a8.html){: external}.

Make sure that your {{site.data.keyword.cloud_notm}} account is set up to create clusters. For more information and the steps to ready your account, see [Prepare to create clusters at the account level](/docs/openshift?topic=openshift-clusters#cluster_prepare).

## Creating the OpenShift cluster
{: #create-clusters}

Use the following steps to create your {{site.data.keyword.openshiftshort}} cluster. The values used in the scenario below are suggestions for a test environment. Select the optional values that will fit your needs.

The Information button provides field-specific information on how to use a field.
{: tip}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/kubernetes/clusters?platformType=openshift){: external} and click *Create clusters*.
1. Select *Single zone* or *Multizone* for *Availability*.
1. Select the *Worker zones*, or data centers, based on your *Geography*. Private VLAN and Public VLAN default based on your selections. Make your selections.

  Your clusters, VLAN, and jump host must be in the same worker zone, or data center.
  {: note}

1. Choose *Private endpoint only* for *Master service endpoint*.
1. Enter a *Cluster name*, *Resource group*, and *Tags*.
1. Select *3.11* for *{{site.data.keyword.openshiftshort}} version*.
1. Select *8 vCPUs, 32 GB RAM* for *Flavor* for the test environment.
1. Enter at least *3* for the number of *Worker nodes*.
1. Click *Create cluster*.

For additional information on the fields, see [Creating a classic standard cluster in the console](/docs/openshift?topic=openshift-clusters#clusters_ui).

## Setting up your jump host
{: #set-up-jump-host}

The instructions for implementing SAP Data Hub on the {{site.data.keyword.openshiftshort}} cluster follow [SAP Data Hub 2 on OpenShift Container Platform 3](https://access.redhat.com/articles/3630111){: external}, which lays out the [jump host's requirements](https://access.redhat.com/articles/3630111#jump-server-requirements){: external}. Either you have a suitable jump server available that is connected to the {{site.data.keyword.openshiftshort}} cluster in {{site.data.keyword.cloud_notm}}, or use the following steps to create an {{site.data.keyword.virtualmachineslong}} instance to serve as your jump host.

1. Once you've confirmed that the {{site.data.keyword.openshiftshort}} cluster is building, select the **Menu icon ![Menu icon](../../icons/icon_hamburger.svg)** > **Classic Infrastructure** > **Devices** > **Order Devices** > **Compute** > **Virtual Server**.
1. Leave the default values for * **Type of virtual server**, **Quantity**, and **Billing**.
1. Enter, for example, **jump4sdh** for **Hostname**. Click **Information** for formatting specifics.
1. Enter, for example, **sdh.com** for **Domain**. Domain is the identification string that defines administrative control within the internet. Click **Information** for formatting specifics.
1. Select your **Placement group**, default is **None**.
1. **Location** needs to be the same data center as where you set up your {{site.data.keyword.openshiftshort}} cluster.
1. Click **All profiles** and select **B1.4x16**, which is the minimum requirement.
1. Provide your SSH key or leave **None** for **SSH keys**. For more information on SSH keys, see [About SSH keys](/docs/ssh-keys?topic=ssh-keys-about-ssh-keys). 
1. Select **Red Hat 7xMinimal (64 bit) - HVM for **Image**.
1. Leave the default values for **Attached storage disks** and **Network interface**.
1. Click **Third-Party Service Agreements** and click **Create**.

Your {{site.data.keyword.openshiftshort}} cluster and your virtual server should be available in about 15 minutes.

## Next Steps
{: #next-steps-create-os-cluster}

[Prepare jump host](/docs/sap-data-hub?topic=sap-data-hub-prepare-jump-host).

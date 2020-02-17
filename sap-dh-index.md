---

copyright:
  years: 2020
lastupdated: "2020-02-12"

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

# Getting started with SAP Data Hub on IBM Cloud
{: #getting-started}

{{site.data.keyword.IBM}} and SAP continue an over 40-year collaboration in multiple areas, including hardware, software, cloud, services, and finance. They are now collaborating to run SAP Data Hub on {{site.data.keyword.cloud}}.
{: shortdesc}

This content provides you with recommendations for the provisioning and installation of the infrastructure to support SAP Data Hub on {{site.data.keyword.cloud_notm}}. It does not replace to any SAP Data Hub implementation-related documentation. Its purpose is to help you with infrastructure planning and provisioning so you can run SAP Data Hub on the {{site.data.keyword.cloud_notm}}. Recommendations and guidelines are provided to help you operate your SAP system in the {{site.data.keyword.cloud_notm}} environment.

## Before you begin
{: #before-you-begin-getting-started}

Table 1 contains useful information you should review before you begin your actual implementation steps.

| Task | Details |
| ----- | ----- |
| Read IBM Cloud, SAP, and other related content that will help you with your implementation | * [What is IBM Cloud](https://www.ibm.com/cloud) |
| | * [What is the IBM Cloud platform?](https://cloud.ibm.com/docs/overview?topic=overview-whatis-platform)
| | * [Get started with IBM Cloud](https://www.ibm.com/cloud/get-started) |
| | * [How to create an SAP S-user ID](https://www.youtube.com/watch?v=4wICiRTP8u0/). Note that only super administrators or S-users with the required authorization are allowed to create S-user IDs. |
| | * [SAP Notes through SAP Support](https://support.sap.com/en/index.html) |
| | * [What are Containers?](https://www.ibm.com/cloud/learn/containers)
| | * [Getting started with Red Hat OpenShift on IBM Cloud?](/docs/openshift?topic=openshift-getting-started), including the tutorial
| | * [Tutorial: Creating a Red Hat OpenShift on IBM Cloud cluster](/docs/openshift?topic=openshift-openshift_tutorial)
| | * [What is SAP Data Hub?](https://www.sap.com/products/data-hub.html)
| Sign up for IBM Cloud | [Signing up for IBM Cloud](https://cloud.ibm.com/docs/account/adminpublic.html#signing-up-for-ibm-cloud) |
| Access the IBM Cloud console | [IBM Cloud console](https://cloud.ibm.com) is your graphical gateway to all your infrastructure components-compute, networking, and storage. You need an [IBMid and password](https://cloud.ibm.com/docs/account?topic=account-signup#signing-up-for-ibm-cloud) to access the console.
{: caption="Table 1. Information to review prior to beginning your implementation" caption-side="top"}

## Implementing SAP Data Hub
{: #implement-sap-data-hub}

Table 2 contains the steps to build your SAP Data Hub infrastructure.

| Task | Details |
| ----- | ----- |
| 1 | [Create the OpenShift cluster and the jump host](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-create-os-cluster)
| 2 | [Prepare the jump host](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-prepare-jump-host) |
| 3 | [Choose the Dynamic Storage Provisioner](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-configure-dynamic-storage-provider) |
| 4 | [Set up the IBM Cloud Container Registry](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-set-up-external-image-registry) |
| 5 | [Deploy Red Hat's SDH Observer](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-deploy-sdh-observer) |
| 6 | [Run the installation shell script](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-run-install-sh) |
| 7 | [Provide the Modeler's Access Credentials for the IBM Cloud Container Registry](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-modeler-provide-credentials-for-registry) |
{: caption="Table 2. Steps to build your SAP Data Hub infrastructure" caption-side="top"}

Many of the implementation steps are in the Red Hat article [SAP Data Hub 2 on OpenShift Container Platform 3](https://access.redhat.com/articles/3630111#jump-host-preparation){: external}. {{site.data.keyword.cloud_notm}}-specific steps are pointed out within each section of Table 2.
{: note}

## Next Steps
{: #next-steps-getting-started}

Select [Create the {{site.data.keyword.openshiftshort}} cluster](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-create-os-cluster).

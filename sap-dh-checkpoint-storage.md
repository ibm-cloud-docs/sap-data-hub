---

copyright:
  years: 2020
lastupdated: "2020-03-24"

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
{:pre. pre}

# Preparing IBM Cloud Object Storage for SAP Vora Checkpoint
{: #set-up-checkpoint-storage}

This topic points you to the steps of provisioning the {{site.data.keyword.cos_full}} and creating the bucket and directory that are required for SAP Vora Checkpoint storage. In addition, you learn how to find the parameters for the SAP Data Hub installation dialog.
{: shortdesc}

## Before you begin
{: #before-you-begin-set-up-checkpoint-storage}

Review [Getting started with {{site.data.keyword.cos_full_notm](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-getting-started) for general information on {{site.data.keyword.cos_full_notm}}.

## Provisioning {{site.data.keyword.cos_short}}
{: #provision-storage}

Use the steps in [Provision storage](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-provision) to provision your {{site.data.keyword.cos_short}}.

  The service instance name for the following example is `sdh_cos_k8`. Choose a name that fits your needs when creating your service instance.
  {: note}

## Creating the bucket and the directory
{: #create-bucket}

1. Use the steps under [Creating some buckets to store you data](/docs/services/cloud-object-storage/basics?topic=cloud-object-storage-getting-started#gs-create-buckets).
1. Choose `Regional` as your **level of resiliency** and the same **Location** where your {{site.data.keyword.openshiftshort}} cluster is deployed. For this example, the location is `eu-de`.
1. A **Storage Class** of **`Standard`** is required to meet performance needs.

  The bucket name in this example is `sdh-cos-vora-bucket`. You must not use `.` in the bucket name, otherwise SAP Data Hub cannot access it.
  {: note}

1. Create the directory by uploading an empty folder from your desktop to the bucket. In the console, navigate to your bucket. Select **Resource List** > **Storage** > **`sdh_cos_k8`** > **`sdh-cos-vora-bucket`** and click **Upload**.
1. Select the empty folder and name it `checkpoints`.

  The first time you upload, you may be required to install the free tool, Aspera Connect. Another option is to create the directory by using `aws` in the command line tool. This information is not discussed in this content.
  {: note}

### Creating the service instance and credentials for accessing the bucket
{: #create-serv-inst-cred}

1. Follow the steps under [Service credentials](/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials) to create the service credentials for accessing the bucket that is handed over to the SAP Data Hub installation script. Make sure that you select the **Writer** role and click **Include HMAC Credential**.

  The credential name in this example is `sdhOScred`. Choose a name that fits your needs when creating your service credentials.
  {: note}

  After the service credentials have been created, click **View credentials** and note the values of `access_key_id` and `secret_access_key`. See below for an example.

  ```
  ...
    "cos_hmac_keys": {
      "access_key_id": "383**************************cf3",
      "secret_access_key": "a24******************************************0f9"
    },
  ```

## Locating the Installation Dialog parameters
{: #find-parameters}

During installation of SAP Data Hub, you're prompted to:

`Enable Vora checkpoint store? (yes/no)`.

If you're setting up a test environment, select **no**. However, for a production environment, select **yes**. You're next asked for the following parameters:

  ```
  Please provide the following parameters for Vora's checkpoint store
  Please enter type of shared storage (oss/s3/wasb/gcs/webhdfs):
  Please enter S3 access key:
  Please enter S3 secret access key:
  Please enter S3 host (empty for default 'https://s3.amazonaws.com'):
  Please enter connection timeout in seconds (empty for default 180):
  Please enter S3 bucket and directory (in the form my-bucket/directory):
  Do you want to validate the checkpoint store? (yes/no)
  ```
  {: screen}

1. Select **`s3`** for **`type of shared storage1**. You can take advantage of {{site.data.keyword.openshiftshort}}'s compatibility with the S3 API.
1. Enter the **`access_key_id'** from the generation of the service credentials for the **S3 access key**.
1. Enter the **`secret_access_key`** from the generation of the service credentials for the *S3 secret access key**.
1. Use [Endpoints and storage locations](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints) to find the **S3 Host** that matches the location where your bucket's created. In the example, it's `s3.eu-de.cloud-object-storage.appdomain.cloud`. Leave **region:** blank. It's taken from the endpoint's URL, for example, `eu-de`.

  In a production environment, you should use the private endpoint.
  {: note}

1. Leave **connection timeout in second** blank. It will default to 180 seconds.
1. Enter the bucket and directory names you entered in [Creating the bucket and the directory](#create-bucket) for **S3 bucket and directory**.
1. Enter **yes** to **validate the checkpoint store**.

## Next Steps
{: #next-steps-deploying}

Manual installation is recommended. Follow the steps under [Manual Installation using an installation script (manual)](https://access.redhat.com/articles/3630111#manual){: external} instructions, and the instructions for [running the installation script](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-run-install-sh) for the {{site.data.keyword.cloud_notm}} specific parameters.

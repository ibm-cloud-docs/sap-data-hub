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

# Implementing SAP Data Hub on IBM Cloud
{: #implementing}

There are four steps to implementing SAP Data Hub on {{site.data.keyword.cloud}}. You must
 * [Prepare the Jump Host](#jump-host)
 * [Configure the dynamic storage provider](#dynamic-storage-provider)
 * [Set up an external image registry](#external-image-registry)
 * [Deploy Red Hat's SAP Data Hub (SDH) observer in the `sdh` namespace](#deploy-in-sdh-namespace)
{: shortdesc}

## Preparing the Jump Host
{: #jump-host}

The first step is to prepare your jump host to connect to the OpenShift cluster. Use the following steps to prepare for this connection.

1. Install the [{{site.data.keyword.cloud}} CLI](/docs/cli?topic=cloud-cli-install-ibmcloud-cli).
   `curl -sL https://ibm.biz/idt-installer | bash`
1. Log in to {{site.data.keyword.cloud_notm}}.
1. Create an [{{site.data.keyword.cloud_notm}} API key file](/docs/iam?topic=iam-userapikey#create_user_key).

### Log in to {{site.data.keyword.cloud_notm}} {{site.data.keyword.openshiftshort}}
{: #log-in-to-openshift}

Is this the first time you've logged in to {{site.data.keyword.openshiftshort}}?
 * Yes. Use the following command:
 ```

  $ ibmcloud login --sso
```  
    * Use a temporary key.
    * Use the following command to generate permanent API key.
```
      $ ibmcloud iam api-key-create <APIKeyName> -d "This is your APIKey" --file <APIKeyFilename>
```
      The API key file <APIKeyFilename> is stored in your current directory.

 * No. Use the following command to log in with an API key file.
 ```
  $ ibmcloud login --apikey @<APIKeyFilename>
  ```

`userID` must be `cluster-admin`.
{: note}

### Preparing the Jump Host
{: #jump-host-2}

1. Prepare the jump host on the [Red Hat OpenShift Container Platform (OCP)](https://access.redhat.com/articles/3630111#jump-host-preparation){: external}. Begin with step 2.
1. Skip the steps in Section 3.2 because the OCP is automatically installed for you.
1. Skip the steps in Section 3.3. Environment health checks are automatically performed.

## Configuring the dynamic storage provider
{: #dynamic-storage-provider}

Dynamic volume provisioning allows storage volumes to be created on-demand. Before dynamic storage provisioning, you had to make a manually call to your cloud or your storage provider to provision new storage volumes and create storage classes. These two steps are now automated. Use the steps below configure Red Hat OpenShift with the {{site.data.keyword.cloud_notm}} storage class.

1. Use the information found in [3.4.1 Configure Dynamic Storage Provider](https://access.redhat.com/articles/3630111#configure-pv-provider){: external}.
1. Use the {{site.data.keyword.cloud_notm}} storage class, `ibmc-block-*`.

## Setting up an external image registry
{: #external-image-registry}

1. Set up your [external image registry](https://access.redhat.com/articles/3630111#external-image-registry){: external}.

## Deploying Red Hat's SDH observer in the `sdh` namespace
{: #deploy-in-sdh-namespace}

To be used with OCP 3.10 or higher.
{: note}

Deploy `sdh-observer` in the `sdh` namespace when at least one of the following holds true:

 * [Kaniko Image Builder](https://access.redhat.com/articles/3630111#kaniko){: external} shall be enabled and the external image registry is not secured with a certificate signed by a trusted certificate authority.
 * [Kaniko Image Builder](https://access.redhat.com/articles/3630111#kaniko){: external} shall be disabled and the OCP cluster version is 3.10 or higher.

OCP process:
```
-f https://rawgithubusercontent.com/redhat-sap/sap-datahub/master/sdh-observer.yaml BASE_IMAGE_TAG=v3.11 SDH_NAMESPACE=sdh
MAKE_VSYSTEM_IPTABLES_PODS_PRIVILEGED=true NAMESPACE=sdh
REGISTRY=de.icr.io/sdh_cr_os | oc replace -f-
```

---

copyright:
  years: 2020
lastupdated: "2020-02-14"

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
{:pre. .pre}

# Running the installation shell script
{: #run-install-sh}

The command line parameters provide {{site.data.keyword.cloud}} specific information for the SAP installation script. The previous steps showed you how to define the default storage provider and how to set up the image registry, including its namespace and secret.

In addition, the SAP Data Hub installer needs information about the domain to be used for the auto-generated certificates and the enablement of the [Kaniko image builder](https://access.redhat.com/articles/3630111#kaniko){: external}.

The default domain for {{site.data.keyword.openshiftlong}} clusters on {{site.data.keyword.cloud_notm}} is `<region>.containers.cloud.ibm.com`. You can see your region with the command `oc project`.
{: note}

1. Before you can unzip the SAP Data Hub Foundation image you must install `unzip`. See [4.5.1. Download and unpack the SDH binaries](https://access.redhat.com/articles/3630111#download-and-unpack-sdh-binaries){: external} for how to obtain the SAP image.

    ```
    yum install unzip
    ```
    {: pre}
    
1. Log in to the IBM Cloud Container Registry.

    ```
    ibmcloud cr login 
    ```
    {: pre}

1. Use the following command using parameters from previous steps. You will be entering your S-UserID and password.

    ```
    ./install.sh --namespace sdh --registry de.icr.io/sdh_cr_os --pv-storage-class ibmc-block-bronze \
    --cert-domain eu-de.containers.cloud.ibm.com --image-pull-secret sdh-de-icr-io --enable-kaniko yes
    ```
    {: pre}
    
The installation and the initializations of all pods is a lengthy process.
{: note}

## Next Steps
{: #next-steps-running}

After installation has finished and the SAP Data Hub Launchpad is available, log in to SAP Data Hub, and use [Red Hat's instructions](https://access.redhat.com/articles/3630111#aws-ecr-secret-for-modeler){: external} to [provide the proper credentials for the Modeler in System Management](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-modeler-provide-credentials-for-registry).

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

# Preparing the jump host
{: #prepare-jump-host}

Prepare your jump host to connect to the {{site.data.keyword.openshiftlong}} cluster that will host SAP Data Hub.
{: shortdesc}

## Before you begin
{: #prepare-jump-host-before}

It's recommended you become familiar with the [{{site.data.keyword.cloud}} user management](/docs/iam?topic=iam-getstarted). Your {{site.data.keyword.openshiftshort}} user ID must have the cluster Administrator role.

## Install the {{site.data.keyword.cloud_notm}} and {{site.data.keyword.openshiftshort}} client Command Line Interfaces (CLI)
{: #jump-host}

Use the following commands to prepare for the connection between your {{site.data.keyword.openshiftshort}} cluster and your jump host.

1. Login to the jump host as `root` and install the {{site.data.keyword.cloud}} CLI `ibmcloud`.

    ```
    curl -sL https://ibm.biz/idt-installer | bash
    ```
    {: pre}
    For more information on {{site.data.keyword.cloud_notm}} CLI, see [Getting started with the IBM Cloud CLI and Developer Tools](/docs/cli?topic=cloud-cli-getting-started).
    
1. Download and install the {{site.data.keyword.openshiftshort}} 3.11 CLI `oc`.

    Click [here](http://mirror.openshift.com/pub/openshift-v3/clients){: external} for the repository of all `oc` versions. To find the correct version, navigate to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/kubernetes/clusters?platformType=openshift){: external} and look at the details of the {{site.data.keyword.openshiftshort}} cluster that you created before under `Version`. For example, 3.11.161_1539, which matches version 3.11.161.

    Download the appropriate version.
    
    ```
    wget http://mirror.openshift.com/pub/openshift-v3/clients/3.11.161/linux/oc.tar.gz
    ```
    {: pre}
    
    Unpack the {{site.data.keyword.openshiftshort}} client.
    
    ```
    tar -xf oc.tar.gz
    ```
    {: pre}
    
    Move it to the local executables directory.

    ```
    mv oc /usr/local/bin
    ```
    {: pre}

1. Download and install `kubectl`. For {{site.data.keyword.openshiftshort}} 3.11, release v1.11 of `kubectl` is required.
    
    Download the appropriate version.
    
    ```
    curl -LO https://dl.k8s.io/release/v1.11.10/bin/linux/amd64/kubectl
    ```
    {: pre}
    
    Set the executable attribute.
    
    ```
    chmod +x ./kubectl
    ```
    {: pre}
    
    Move it to the local executables directory.
    
    ```
    mv kubectl /usr/local/bin
    ```
    {: pre}

1. Download a script from [The Kubernetes Package Manager](https://github.com/kubernetes/helm){: external} and execute it with the the latest release of `helm` for your SAP Data Hub (SDH) release. The 'helm' version is `v2.11.0` for SAP Data Hub 2.4, 2.5, 2.6 and 2.7.

    ```
    curl --silent https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | \
    DESIRED_VERSION=v2.11.0 bash
    ```
    {: pre}

1. Download and install `docker` through the standard Red Hat repositories.

    Enable the appropriate Red Hat repositories.
   
    ```
    subscription-manager repos --enable=rhel-7-server-rpms \
     --enable=rhel-7-server-extras-rpms \
     --enable=rhel-7-server-optional-rpms
    ```
    {: pre}
    
    Install `docker` and some dependent packages.
    
    ```
    yum install -y docker device-mapper-libs device-mapper-event-libs
    ```
    {: pre}
    Start the docker process.
    ```
    systemctl enable --now docker.service
    ```
    {: pre}

1. Verify the installation of the {{site.data.keyword.cloud_notm}} CLI.
    
    ```
    ibmcloud dev -v
    ```
    {: pre}

    Example output:
    
    ```
    ibmcloud dev version 2.4.0
    ```
    {: screen}
    Verify `kubectl`.
    ```
    kubectl version
    ```
    {: pre}

    Example output:
    
    ```
    Client Version: version.Info{Major:"1", Minor:"11", GitVersion:"v1.11.10", GitCommit: ...
    ```
    {: screen}
    Verify `docker`.
    ```
    systemctl status docker
    ```
    {: pre}

    Example output:
    
    ```
    ● docker.service - Docker Application Container Engine
    Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
    Active: active (running) since ...
    ```
    {: screen}

1. Log in to {{site.data.keyword.cloud_notm}}.

    ```
    ibmcloud login [--sso]
    ```
    {: pre}
    
    a. If you're logging in to {{site.data.keyword.cloud_notm}} for the first time, create an [{{site.data.keyword.cloud_notm}} API key file](/docs/iam?topic=iam-userapikey#create_user_key).

    The command is `ibmcloud iam api-key-create <APIKeyName> -d <description> --file <APIKeyFilename>`.
    The API key file <APIKeyFilename> will be stored in your current directory.
    
    ```
    ibmcloud iam api-key-create myAK4sdh -d "This is your API Key for SAP Data Hub" \
    --file myAK4sdh_file
    ```
    {: pre}
  
    b. For all subsequent logins use the API Key file that you created earlier during step a.
    The command is `ibmcloud login --apikey @<APIKeyFilename>`.
 
    ```
    ibmcloud login --apikey @myAK4sdh_file
    ```
    {: pre}

## Access the {{site.data.keyword.openshiftshort}} cluster

After logging in to the jump host you **must complete the following steps** before you can work with your cluster.

1. Determine the cluster ID on which you want to install SAP Data Hub.

    ```
    ibmcloud oc cluster ls
    ```
    {: pre}

    ```
    Name                     ID                     State    Created        Workers   ...
    RH_OS_DataHub_SAP_Test   bm****************kg   normal   3 months ago   3         ...
    ```
    {: screen}

1. Download the Kubernetes configuration files and certificates to connect to your cluster.

    The command is `ibmcloud oc cluster config --cluster <clusterID> --admin`. Copy the cluster ID frome the previous step.
    
    You can copy the export command from the response of the `ibmcloud oc` cluster config command.
    {: note}
    
    ```
    ibmcloud oc cluster config --cluster bm****************kg --admin
    ```
    {: pre}
    
    ```
    export KUBECONFIG=/root/.bluemix/plugins/container-service/clusters/......-RH_OS_DataHub_SAP_Test.yml
    ```
    {: pre}

You have created an {{site.data.keyword.openshiftshort}} cluster and prepared the jump host.

## Next Steps
{: #next-steps-prepare-jump-host}

* As the OCP has been automatically installed and verified for you, **skip the following sections**:
  * [3.2. Install {{site.data.keyword.openshiftshort}} Container Platform](https://access.redhat.com/articles/3630111#install-openshift){: external}.
  * [3.3. (Optional) Validate the {{site.data.keyword.openshiftshort}} cluster](https://access.redhat.com/articles/3630111#ocp-validate){: external}.
  * Continue with the [3.4. OCP Post Installation Steps](https://access.redhat.com/articles/3630111#ocp-post-installation){: external}.

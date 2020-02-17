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
{:pre. pre}

# Setting up the IBM Cloud Container Registry
{: #set-up-external-image-registry}

In [Preparing the Jump Host](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-prepare-jump-host), you installed the {{site.data.keyword.cloud}} CLI, which installed the CLI plug-ins for {{site.data.keyword.containerlong}} and {{site.data.keyword.registryfull}}. Next, you'll set up the external image registry.
{: shortdesc}

Use the following commands to set up the external image registry.

1. Log in to {{site.data.keyword.cloud}} with your API key file that you have generated when you have [prepared your jump host](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-prepare-jump-host).

    ```
    ibmcloud login --apikey @myAK4sdh_file
    ```
    {: pre}
    
1. Create a new {{site.data.keyword.registryshort_notm}} namespace, `ibmcloud cr namespace-add <my_namespace>`. For this example, the namespace name is `sdh_cr_os`, which is an acronyms for SAP Data Hub - Container Registry - OpenShift.

    ```
    ibmcloud cr namespace-add sdh_cr_os
    ```
    {: pre}
    
1. Verify that the namespace is created.
   
    ```
    ibmcloud cr namespace-list
    ```
    {: pre}

## Next Steps
{: #next-steps-external-image-registry}

Continue with [Project setup](https://access.redhat.com/articles/3630111#project-setup){: external}, **skipping the following steps:**
 * [3.4.3.2.1. Enable NFS in containers](https://access.redhat.com/articles/3630111#virt-use-nfs){: external}
 * [3.4.3.2.2. Pre-load kernel modules](https://access.redhat.com/articles/3630111#pre-load-kernel-modules){: external}
 * [3.4.3.2.3. Permit access to docker socket](https://access.redhat.com/articles/3630111#permit-access-to-docker-socket){: external}
 
Begin with the following steps:
 * [3.4.3.2.4. Allow administrator to manage SDH resources](https://access.redhat.com/articles/3630111#admin-can-manage-sdh){: external}
 * [3.4.3.2.5. Create privileged tiller service account](https://access.redhat.com/articles/3630111#create-privileged-tiller){: external}
 * [3.4.3.2.6. Initialize helm](https://access.redhat.com/articles/3630111#helm-init){: external}
 * [3.4.3.2.7. Create sdh project](https://access.redhat.com/articles/3630111#create-sdh-project){: external}
 
 * Skip step [3.4.3.2.8. Granting privileges to sdh admin](https://access.redhat.com/articles/3630111#sdhadmin-privileged){: external}.

 * Continue with [Deploying the SDH Observer](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-deploy-sdh-observer).

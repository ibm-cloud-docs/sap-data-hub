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

# Choosing the Dynamic Storage Provisioner
{: #configure-dynamic-storage-provider}

Dynamic volume provisioning allows storage volumes to be created on-demand. {{site.data.keyword.cloud}} provides several [storage classes](/docs/openshift?topic=openshift-storage_planning) that can be used as storage providers.
{: shortdesc}

Use the steps below to define the default {{site.data.keyword.cloud_notm}} storage class.

1. Select the appropriate storage class that will fit your needs.

  For SAP Data Hub `ibmc-block-bronze` is recommended.
  {: note}
  
1. Remove the current default storage class:

    ```
    oc annotate sc --all storageclass.beta.kubernetes.io/is-default-class- \
    storageclass.kubernetes.io/is-default-class-
    ```
    {: pre}
    
1. Run the following command to define `ibmc-block-bronze` as the default storage class.

    ```
    oc annotate storageclass ibmc-block-bronze storageclass.kubernetes.io/is-default-class=true
    ```
    {: pre}
    
The SAP Data Hub installation command lets you provide different storage classes for the different SAP Data Hub storage types.
{: note}

## Next Steps
{: #next-steps-configure}

Use [Red Hat's instructions](https://access.redhat.com/articles/3630111#setup-image-registry){: external} to [set up an external image registry](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-set-up-external-image-registry).

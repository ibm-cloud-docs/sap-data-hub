---

copyright:
  years: 2020
lastupdated: "2020-03-24"

keywords: SAP Data Hub, {{site.data.keyword.cloud_notm}}, {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.cos_short}}, {{site.data.keyword.openshiftlong_notm}}, {{site.data.keyword.openshiftshort}}, Red Hat Enterprise Linux, SAP Data Hub on {{site.data.keyword.cloud_notm}}, data orchestration, data governance, data integration

subcollection: sap-data-hub

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:note: .note}
{:pre: .pre}
{:screen: .screen}
{:table: .aria-labeledby="caption"}

# Deploying Red Hat's SDH Observer
{: #deploy-sdh-observer}

Deploying the SAP Data Hub (SDH) Observer lets the SAP Data Hub installer and its runtime access images in the {{site.data.keyword.registryfull}} to provide the image pull secret for the project. In the Red Hat instructions, the project name is `sdh`.
{: shortdesc}

The project's namespace and {{site.data.keyword.registryshort_notm}} names used in the following steps are the same as those used in previous steps.
{: note}

1. Copy the local pull image secret from the `default` to the `sdh` project. For more information, see [Copying an existing image pull secret](/docs/openshift?topic=openshift-images#copy_imagePullSecret).

    The source secret should match your region, for example, `de` for Central Europe. In this example, the source secret `default-de-icr-io` is copied to `sdh-de-icr-io`.
    {: note}

    Find your region.

    ```
    ibmcloud cr api
    ```
    {: pre}
    The region code precedes the `icr.io` domain - here we get `de`.    
    ```
    Registry API endpoint   https://de.icr.io/api
    ```
    {: screen}

    The command line is: `oc patch secret --dry-run -o yaml -n default <local_secret>  -p '{"metadata":{"namespace": "<project_name>"}}' | oc -n sdh create -f -`
    ```
    oc patch secret --dry-run -o yaml -n default default-de-icr-io -p '{"metadata": {"namespace": "sdh", "name": "sdh-de-icr-io"}}' | oc -n sdh create -f -
    ```
    {: pre}

1. Use the secret `sdh-de-icr-io` as an image pull secret by the default service account’s pods.

    ```
    oc secrets link -n sdh --for=pull default sdh-de-icr-io
    ```
    {: pre}

1. Use the following command to deploy the SAP Data Hub Observer in the `sdh` namespace.

    Review the [Required Input Parameters](https://access.redhat.com/articles/3630111#deploy-sdh-observer){: external} to confirm the deployment instructions and the source URL are valid.
    {: note}

    ```
    oc process -f https://raw.githubusercontent.com/redhat-sap/sap-datahub/master/sdh-observer.yaml \
    BASE_IMAGE_TAG=v3.11 SDH_NAMESPACE=sdh MAKE_VSYSTEM_IPTABLES_PODS_PRIVILEGED=true NAMESPACE=sdh \
    REGISTRY=de.icr.io/sdh_cr_os | oc create -f -
    ```
    {: pre}

## Next Steps
{: #next-steps-deploying}

For production environments, storage for SAP Vora checkpoint needs to be set up. Follow [these instructions](/docs/sap-data-hub?topic=sap-data-hub-set-up-checkpoint-storage) to set up {{site.data.keyword.cos_full_notm}} and to define the parameters that will be handed over to the installation dialog.

If installing SAP Data Hub for test and evaluation, skip this step and go directly to the [Manual Installation using an installation script (manual)](https://access.redhat.com/articles/3630111#manual){: external} instructions, and the instructions for [running the installation script](docs/infrastructure/sap-data-hub?topic=sap-data-hub-run-install-sh) for the {{site.data.keyword.cloud_notm}} specific parameters.

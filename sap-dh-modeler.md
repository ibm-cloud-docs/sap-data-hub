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
{:codeblock: .codeblock}
{:pre. .pre}

# Provide the Modeler's Access Credentials for the IBM Cloud Container Registry
{: #modeler-provide-credentials-for-registry}

Use the following commands to provide the modeler's access credentials to the {{site.data.keyword.registryfull}}.
{: shortdesc}

1. Retrieve your user's API key that you have created during [Preparing the jump host](/docs/infrastructure/sap-data-hub?topic=sap-data-hub-prepare-jump-host).

    ```
    cat myAK4sdh_file
    {
	    "name": "myAK4sdh",
	    "description": "This is your API Key for SAP Data Hub",
	    "apikey": "********************************************",
	    "createdAt": "2019-12-18T10:40+0000",
	    "locked": false,
	    "uuid": "ApiKey-********-****-****-****-************"
    }
    # copy the API key shown after "apikey".
    ```
    {: pre}

2. Follow the instructions in [Provide Access Credentials for a Password Protected Container Registry](https://help.sap.com/viewer/e66c399612e84a83a8abe97c0eeb443a/2.7.latest/en-US/a1cbbc0acc834c0cbbe443f2e0d63ab9.html){: external}. Be sure to use the following parameters, including pasting the copied API key into the **password** field.

    ```
    cat >/tmp/vsystem-registry-secret.txt <<EOF
    username: "iamapikey"
    password: "********************************************"
    EOF
    ```
    {: pre}

## Next Steps
{: #next-steps-provide-credentials}

Continue with the [validation of the installation](https://access.redhat.com/articles/3630111#sdh-validation){: external}.

---
description: Step-by-step guide to ingest your data from Google BigQuery into RudderStack.
---

# Google BigQuery

[Google BigQuery](https://cloud.google.com/bigquery) ****is an industry-leading, fully-managed cloud data warehouse that allows you to store and analyze petabytes of data in no time.

This guide will help you configure BigQuery as a source from which you can route event data to your desired destinations through RudderStack.

## Granting Permissions

Follow these steps below to grant the necessary permissions for warehouse actions. For BigQuery, use the BigQuery Console.

* Go to [https://console.cloud.google.com/iam-admin/roles](https://console.cloud.google.com/iam-admin/roles) and click on “CREATE ROLE”.
* Fill it in as shown below. 

![](../.gitbook/assets/image1.png)

* Click on “ADD PERMISSIONS” and add the permissions mentioned below.

![](../.gitbook/assets/image3.png)

* Click on “CREATE”.
* Now, move to [https://console.cloud.google.com/iam-admin/serviceaccounts](https://console.cloud.google.com/iam-admin/serviceaccounts) and select the project which has the dataset or table you want to share.
* Click on “CREATE SERVICE ACCOUNT”.
* Fill in the details in Step 1 and click CREATE:

![](../.gitbook/assets/image2.png)

* Fill in the details in Step 2 and click CONTINUE:

![](../.gitbook/assets/image4.png)

* After filling in steps 1 and 2 click DONE.
* This will move you to the list of service accounts. Now, click on the “3 dots” for the service account that just created, and select “Manage keys”.
* Click on ADD KEY and select "Create new key". In the pop up select JSON and click CREATE. 
* Download this json file and use its data while creating a BQ source on RudderStack.

## Set Up as Source

To set up Google BigQuery as a source in RudderStack, follow these steps:

* Log into your [RudderStack dashboard](https://app.rudderlabs.com/signup?type=freetrial).
* From the left panel, select **Sources**. Then, click on **Add Source**, as shown:

![](../.gitbook/assets/image%20%2897%29%20%281%29%20%281%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%282%29%20%283%29%20%281%29%20%282%29.png)

* Scroll down to the **Warehouse Sources** and select **BigQuery**. Then, click on **Next**.

![](../.gitbook/assets/screen-shot-2021-01-05-at-2.03.31-pm.png)

### Setting Up the Connection

* Assign a name to your source, and click on **Create Credentials from Scratch**. Then, click on **Next**.

![](../.gitbook/assets/screen-shot-2021-01-05-at-2.05.48-pm.png)

{% hint style="success" %}
If you've already configured BigQuery as a source before, the existing credentials will automatically appear in the above window.
{% endhint %}

* Next, enter the **GCP project ID** and the **Credentials** JSON which RudderStack will use to import the data from your BigQuery instance.

![](../.gitbook/assets/screen-shot-2021-01-05-at-2.07.29-pm.png)

### Specifying the Data to Import

* Next, select the **Schema** and the **Table** from which you want RudderStack to import the data.

![](../.gitbook/assets/screen-shot-2021-01-05-at-5.18.59-pm.png)

{% hint style="warning" %}
Your table must include one of the following columns - `email`, `user_id`, or `anonymous_id`.
{% endhint %}

* Once you specify the table containing the required columns, you will be able to preview a snippet of your data, as shown below:

![](../.gitbook/assets/screen-shot-2021-01-05-at-3.21.38-pm.png)

* Here, you can select all or only a few specific columns of your choice, search the columns by a keyword, and also edit the **JSON Trait Key**, as shown below. You can also preview the resultant JSON on the right. Once you've select the required table columns to import the data from, click on **Next**.

![](../.gitbook/assets/screen-shot-2021-01-05-at-3.22.09-pm.png)

### Setting the Data Update Schedule

* Next, you will be required to set the **Run Frequency** to schedule the data import from your PostgreSQL database to RudderStack. You can also specify the time when you want this synchronization to start, by choosing the time under the **Sync Starting At** option. Then, click on **Next**.

![](../.gitbook/assets/screen-shot-2021-01-05-at-5.19.23-pm.png)

That's it! BigQuery is now successfully configured as a source on your RudderStack dashboard. 

RudderStack will start importing data from your BigQuery instance as per the specified frequency. You can further connect this source to your preferred destinations by clicking on **Connect Destinations** or **Add Destinations**, as shown:

![](../.gitbook/assets/screen-shot-2021-01-06-at-2.55.24-pm%20%281%29.png)

{% hint style="info" %}
If you have already configured a destination on the RudderStack platform, choose the **Connect Destinations** option. To add a new destination from scratch, you can select the **Add Destination** option.
{% endhint %}

## Contact Us

If you come across any issues while configuring Google BigQuery as a source on the RudderStack dashboard, please feel free to [contact us](mailto:%20docs@rudderstack.com). You can also start a conversation on our [Slack](https://resources.rudderstack.com/join-rudderstack-slack) channel; we will be happy to talk to you!


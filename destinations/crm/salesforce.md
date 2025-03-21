---
description: Step-by-step guide to set up Salesforce as a destination in RudderStack
---

# Salesforce

[Salesforce](https://www.salesforce.com/in/?ir=1) is an industry leader in enterprise CRM. It offers a suite of enterprise applications revolving around marketing automation, customer engagement and support, application development as well as analytics.

RudderStack allows you to integrate your source to Salesforce in order to identify your leads without having to use the REST APIs.

{% hint style="success" %}
**Find the open-source transformer code for this destination in our** [**GitHub repo**](https://github.com/rudderlabs/rudder-transformer/tree/master/v0/destinations/salesforce)**.**
{% endhint %}

## Getting Started

Before configuring your source and destination on the RudderStack, please verify if the source platform is supported by Salesforce by referring to the table below:

| **Connection Mode** | **Web** | **Mobile** | **Server** |
| :--- | :--- | :--- | :--- |
| **Device mode** | - | - | - |
| **Cloud** **mode** | **Supported** | **Supported** | **Supported** |

{% hint style="info" %}
To know more about the difference between Cloud mode and Device mode in RudderStack, read the [RudderStack connection modes](https://docs.rudderstack.com/get-started/rudderstack-connection-modes) guide.
{% endhint %}

Once you have confirmed that the platform supports sending events to Salesforce, perform the steps below:

* From your [RudderStack dashboard](https://app.rudderlabs.com/), add the source. From the list of destinations, select Salesforce.

{% hint style="info" %}
Please follow our guide on [How to Add a Source and Destination in RudderStack](https://docs.rudderstack.com/how-to-guides/adding-source-and-destination-rudderstack) to add a source and destination in RudderStack.
{% endhint %}

* Give a name to the destination and click on **Next**. You should then see the following screen:

![Salesforce Connection Settings](../../.gitbook/assets/image%20%2841%29.png)

* Please provide your Salesforce username and password here, along with the access token. Then, click on **Next**. Salesforce will be enabled as a destination in RudderStack.

{% hint style="info" %}
We **recommend** that you create a new Salesforce account to use it with RudderStack. This will protect any confidential information present in your existing Salesforce account. This is **entirely optional**, however, and you can also use your existing Salesforce account.

To create a new account, please go to **Setup** - **Administration Setup** - **Users** - **New User** and create a System Administrator profile. This should give RudderStack enough permissions to access the API.
{% endhint %}

## Identify

RudderStack makes it very easy for you to get your leads from your website or mobile app into Salesforce through our `identify` call.

### Identifying a potential lead

The following code snippet demonstrates a sample `identify` call in RudderStack:

```javascript
rudderanalytics.identify('userid', {
  name: 'John Doe',
  title: 'CEO',
  email: 'name.surname@domain.com',
  company: 'Company123',
  phone: '123-456-7890',
  state: 'Texas',
  rating: 'Hot',
  city: 'Austin',
  postalCode: '12345',
  country: 'US',
  street: 'Sample Address',
  state: 'TX'
}, {
  'integrations': {
    'Salesforce': true
  }
});
```

The above code snippet identifies a unique user based on the `userid` and the associated traits passed in the `identify` call.

{% hint style="warning" %}
It is mandatory to include `'Salesforce':true` in every Salesforce integration object. As Salesforce has strict API limits, this is required in order to prevent the users from hitting their limits. By default, RudderStack does not send `identify` calls to Salesforce. Hence, any `identify` call that does not include `'Salesforce':true` in its payload will be ignored.
{% endhint %}

When the `identify` method is called, RudderStack checks if the lead already exists using the `email` property. If it exists, the lead is updated with the traits passed in the `identify` call. If no lead exists, a new lead in Salesforce is created.

### Updating custom fields in Salesforce

If you wish to update custom fields in Salesforce using RudderStack, please ensure you create those lead fields in Salesforce before you send the data through RudderStack. As `lastName` and `company` are needed by the [Salesforce Leads API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_lead.htm), absence of either of these fields will result in RudderStack automatically appending the `'n/a'`string to both the fields - even if they have been specified in some previous request.

As an example, if you wish to collect a custom trait in RudderStack named `newProp`, create a field label named `newProp`. This will generate an API name as `newProp__c`. RudderStack automatically appends the `__c` to any custom trait.

{% hint style="info" %}
Make sure you are consistent with your casing. If the custom fields are created in camelCase, please make sure sure that you send the traits to RudderStack in camelCase. If you're creating custom fields in snake\_case, ensure you send the traits in the same format.
{% endhint %}

## Updating Salesforce Objects

You can create or update any [Salesforce Object](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_list.htm) using the `identify` event. To specify the object type follow the schema below.

We'll look for the key `externalId` under `context` and determine the Salesforce Object type by removing the part `Salesforce-` from the field `type`. We'll make a `PATCH` request if there is an `id` present in the request to update the record. We'll create the record otherwise. You can pass multiple object types in a single request and we'll create that many requests to Salesforce.

```javascript
client.identify({
  userId: '123456',
  traits: {
    FirstName: "John",
    LastName: "Gibbs",
    Email: "john@peterson.com"
  },
  context: {
    externalId: [
      {
        type: "Salesforce-Contact",
        id: "sf-contact-id"
      }
    ]
  }
});
```

In the example above, we'll be updating the `Contact` object in Salesforce with `id` as `sf-contact-id` and send the `traits` object to Salesforce.

You can use this [sample user transformation](https://github.com/rudderlabs/sample-user-transformers/blob/master/SalesforceObjectTrackConversion.js) to convert a `track` call to an `identify` event that supports Salesforce Objects create or update operations.

{% hint style="info" %}
By default we'll create `Lead` objects to Salesforce and map the `traits` as mentioned above. For other objects we don't modify the `traits` object and sent to Salesforce as it is.
{% endhint %}

## FAQs

### Which Salesforce Edition should I use to access the API?

Before connecting to the Salesforce API with RudderStack, make sure you are using the right edition of Salesforce. Please follow [this guide](https://help.salesforce.com/articleView?id=000326486&type=1&mode=1) to know more about the supported editions with the API access. You must have either the **Enterprise**, **Unlimited**, **Developer** or **Performance** editions to access the API.

### Where do I get the Security Token?

You can find your Security Token under **Setup** - **Personal Setup** - **My Personal Information** - [Reset My Security Token](https://na15.salesforce.com/_ui/system/security/ResetApiTokenEdit).

If you still face any issues, feel free to [contact us](https://rudderstack.com/contact/).

### How to check the number of Salesforce API calls left for the day?

To check the number of Salesforce API calls, go to **Setup** - **Administration Setup** - **Company Profile** - **Company Information**. You should be able to see a field called **API Requests, Last 24 Hours**, which contains the number of APIs calls left for the day.

### What if I don't include `'Salesforce': true` in my `identify` call?

Salesforce has a very strict API limit. Moreover, RudderStack by default does not send identify calls to Salesforce. If you don't include `'Salesforce':true` in your `identify` call payload, the call will simply be ignored.

### What does the error `Failed with status code 400` mean?

RudderStack requests Salesforce for an `Authentication token` from its event transformation module with the credentials provided in the dashboard. If your credentials are incorrect, the request will fail with the error mentioned above.

## Contact Us

If you come across any issues while configuring Salesforce with RudderStack, please feel free to [contact us](mailto:%20docs@rudderstack.com). You can also start a conversation on our [Slack](https://resources.rudderstack.com/join-rudderstack-slack) channel; we will be happy to talk to you!


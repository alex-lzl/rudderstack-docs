---
description: >-
  Detailed technical documentation on ingesting your event data from your
  specified cloud extract sources into RudderStack.
---

# Cloud Extract Sources

## RudderStack Cloud Extract

With RudderStack Cloud Extract, you can collect your raw events and data from different cloud tools such as [Facebook Ads](https://www.facebook.com/business/ads), [Google Ads](https://ads.google.com/), [Google Analytics](https://analytics.google.com/), [Google Search Console](https://search.google.com/search-console/welcome), [Marketo](https://www.marketo.com/), [HubSpot](https://www.hubspot.com/), and more. You can then build efficient ELT pipelines from these cloud applications to your data warehouse.

## FAQs

#### Is it possible to have multiple Cloud Extract sources writing to the same schema?

Yes, it is. 

We have implemented a feature wherein RudderStack associates a table prefix for every Cloud Extract source writing to a warehouse schema. This way, multiple Cloud Extract sources can write to the same schema with different table prefixes.

## Popular Cloud Extract Sources

{% page-ref page="../stream-sources/rudderstack-event-streams/segment.md" %}

{% page-ref page="salesforce/" %}

{% page-ref page="zendesk.md" %}

{% page-ref page="../stream-sources/rudderstack-event-streams/customerio.md" %}

{% page-ref page="google-analytics.md" %}

{% hint style="success" %}
If you wish to contribute to developing more cloud source integrations for RudderStack, please refer to the contributing guide [here](https://github.com/rudderlabs/rudder-server/blob/master/CONTRIBUTING.md).
{% endhint %}

## Contact Us

For more information on the RudderStack cloud extract sources or the platform in general, feel free to [contact us](mailto:%20contact@rudderstack.com). You can also start a conversation on our [Slack](https://resources.rudderstack.com/join-rudderstack-slack) channel; we will be happy to talk to you!


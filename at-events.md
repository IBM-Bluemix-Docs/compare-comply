---

copyright:
  years: 2016, 2019
lastupdated: "2019-06-06"

subcollection: compare-comply

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:download: .download}
{:table: .aria-labeledby="caption"}


# {{site.data.keyword.cloudaccesstrailshort}} events
{: #at_events}

Use the {{site.data.keyword.cloudaccesstrailfull}} service to track how users and applications interact with the Compare and Comply service in {{site.data.keyword.Bluemix}}. 
{: shortdesc}

The {{site.data.keyword.cloudaccesstrailfull_notm}} service records user-initiated activities that change the state of a service in {{site.data.keyword.Bluemix_notm}}. For more information, see the [{{site.data.keyword.cloudaccesstrailshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla).

<!-- You can create different sections to group events by area. -->

## List of events
{: #events}

<!-- Make sure you introduce the table with a detailed description that immediately precedes it. For example, see https://cloud.ibm.com/docs/services/cloud-activity-tracker/services/at_events_cf.html#catalog. -->

| Action | Description | 
|:-----------------|:-----------------|
| `compare-comply.html-conversion.create` | Convert a PDF input document to HTML |
| `compare-comply.element-classification.create` | Classify an input document |
| `compare-comply.comparison.create` | Compare two input documents |
| `compare-comply.tables.create` | Classify an input document's tables only |
| `compare-comply.batch.create` | Create a batch request |
| `compare-comply.batch.update` | Update a batch request |
| `compare-comply.batch.delete` | Delete a batch request |
| `compare-comply.batch.read` | Get information about a batch request |
| `compare-comply.feedback.create` | Create feedback |
| `compare-comply.feedback.update` | Update feedback |
| `compare-comply.feedback.delete` | Delete feedback |
| `compare-comply.feedback.read` | Get information about feedback |
| `compare-comply.evaluation.read` | Internal use only |
{: caption="Table 1. Actions that generate events" caption-side="top"}

## Where to view the events
{: #ui}

{{site.data.keyword.cloudaccesstrailshort}} events are available in the {{site.data.keyword.cloudaccesstrailshort}} **account domain** that is available in the {{site.data.keyword.Bluemix_notm}} region where the events are generated.






---

copyright:
  years: 2019
lastupdated: "2019-11-11"

keywords: high availablilty,HA,disaster recovery,DR,backup,restore,custom models,online learning

subcollection: compare-comply

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:important: .important}
{:deprecated: .deprecated}
{:codeblock: .codeblock}
{:screen: .screen}
{:download: .download}
{:hide-dashboard: .hide-dashboard}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}
{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:swift: .ph data-hd-programlang='swift'}
{:go: .ph data-hd-programlang='go'}

# High availability and disaster recovery
{: #recovery}

{{site.data.keyword.cncfull}} is highly available within multiple {{site.data.keyword.cloud_notm}} locations (for example, Dallas and Washington, DC). However, recovering from potential disasters that affect an entire location requires planning and preparation.
{: shortdesc}

You are responsible for understanding your configuration, customization, and usage of the service. You are also responsible for being ready to re-create an instance of the service in a new location and to restore your data in any location. See [How do I ensure zero downtime?](/docs/overview?topic=overview-zero-downtime#zero-downtime){: external} for more information.

## High availability
{: #cnc-ha}

{{site.data.keyword.cncshort}} supports high availability with no single point of failure. The service achieves high availability automatically and transparently by using the multi-zone region (MZR) feature provided by {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.cloud_notm}} enables multiple zones that do not share a single point of failure within a single location. It also provides automatic load balancing across the zones within a region.

## Disaster recovery
{: #cnc-dr}

Disaster recovery can become an issue if an {{site.data.keyword.cloud_notm}}  location experiences a significant failure that includes the potential loss of data. Because MZR is not available across locations, you must wait for IBM to bring a location back online if it becomes unavailable. If underlying data services are compromised by the failure, you must also wait for IBM to restore those data services.

If a catastrophic failure occurs, IBM might not be able to recover data from database backups. In this case, you need to restore your data to return your service instance to its most recent state. You can restore the data to the same or to a different location.

Your disaster recovery plan includes knowing, preserving, and being prepared to restore all data that is maintained on {{site.data.keyword.cloud_notm}}. 

### The Registration API
{: #registration-api}

Some of the following tasks use the {{site.data.keyword.cncshort}} Registration API. The Registration API is currently an internal-only interface. Contact IBM Support for assistance.
{: note}

## Backing up your data 
{: #backup-data}

There are several methods for backing up the data stored in {{site.data.keyword.cncshort}}. These methods need to be included in your disaster recovery plan.

  - Data you need keep a copy of, such as source documents that were processed as [batches](/docs/services/compare-comply?topic=compare-comply-batching).
  - Data in {{site.data.keyword.cncshort}} data stores that you need to retrieve and store a copy of, such as customized models.
  
Some data must be recreated from scratch; for example, [feedback](/docs/services/compare-comply?topic=compare-comply-feedback) provided since the last backup.

### Batch API
{: #batch-api}

The [Batch API](/docs/services/compare-comply?topic=compare-comply-batching) uses customer-provided Cloud Object Storage (COS) buckets to read input documents and to store results.  It is recommended that you use cross-regional buckets for business-critical documents. Cross-regional buckets are automatically replicated across multiple regions and remain available if one region is offline. If you must use single-region or one-zone buckets, you must back up the data in those buckets.

If batches are a critical component of your application, some of the batch jobs might need to be run again after recovery. You must keep enough information in your application's log to be able to know, for each batch submitted, the batch ID returned by the Batch API, and the locations of the input and output buckets.

### Feedback
{: #feedback}

Feedback provided with the {{site.data.keyword.cncshort}} Tooling is stored in an internal data store. Feedback entered after the time of the last backup of this store is lost. To facilitate recovery, you need to keep a log with the name of the documents loaded into the Tooling and the time they were loaded.

### Custom models
{: #custom-models}

If you used the [Registration API](#registration-api) to store custom models into the service, you must keep a backup of those models. It is useful, but not critical, to keep a log of the name and version that you gave to the registered models. You might need to re-register one or more of the models during recovery.

### Online Learning
{: #online-learning}

Online Learning enables customers to semi-automatically generate and register new models based on feedback. These local models are generated by the system and are stored internally by using the [Registration API](#registration-api). In case of a disaster, {{site.data.keyword.cncshort}} is able to recover only custom models that were created before the last backup. For this reason, you need to regularly download the created models by using the `GET /v1/models/{id}/download` method in the Registration API and keep the downloaded models as backup.

## Restoring your data to a new {{site.data.keyword.cncshort}} instance
{: #restoring-data}

Consider using your backups to restore to a new {{site.data.keyword.cncshort}} instance in a different data center.

### Batches
{: #restore-batches}

The database containing the status of batch jobs can be up to 24 hours old. This means that {{site.data.keyword.cncshort}} does not know about batches that were started or were running up to 24 hours before the disaster. This does _not_ mean that data is lost, because the data is in COS buckets. What can be lost, however, are the IDs and statuses for batch jobs that were running or started after the time of the last backup.

Use the [`GET /v1/batches`](/docs/services/compare-comply?topic=compare-comply-batching#get-list-batch) method to get a list of batch statuses known by the recovered system. Then use your application's log to get the information about the batches that were started or were running close to the time of the disaster. For batch jobs that are not listed in the output of the `GET /v1/batches` method, look in the COS output bucket of that batch job. If the bucket contains a `{batch_ID}.json` file, the file lists the final status of the batch job. If the file does not exist, then the batch job did not finish and needs to be re-submitted. Use the [`POST /v1/batches`](/docs/services/compare-comply?topic=compare-comply-batching#post-batch) method to rerun the batch job from scratch. If it was a large batch that already processed a number of input files, as determined by the corresponding JSON output files in the output bucket, you can remove the corresponding input files from the input COS bucket. The new batch job processes the remaining input file or files and adds the results to the same output bucket.

### Feedback
{: #restore-feedback}

Feedback provided by using the {{site.data.keyword.cncshort}} Tooling after the time of the latest backup of the feedback data store is lost. You need to reload the documents you were using into the tool and manually redo the missing feedback. You can use the [`GET /v1/feedback`](/docs/services/compare-comply?topic=compare-comply-feedback#get_all_feedback) method to retrieve all feedback for a given document. Feedback objects include a creation timestamp that can tell you when the last feedback entry was made.

### Custom models
{: #restore-custom-models}

Use the [Registration API](#registration-api) to register models that are not know to the system after recovery. Use the `GET /v1/models` method to get a list of known models and verify that your model and its correct version are in the store after recovery. If it is not, find the model in your backup and register it again.

### Online Learning
{: #restore-online-learning}

Online Learning depends on Feedback to create new customized models. Because some feedback can be lost after recovery, you might need to manually enter the feedback again and then redo the recommendation review and model generation exercises by using the Online Learning tool.

However, if you have a backup of the models that were generated by Online Learning, you can use the [Registration API](#registration-api) to register them into the system as described in [Custom models](#custom-models). You can then resume the Online Learning cycle on these models with the understanding that:

  - Some of the feedback that you provided before the disaster might be lost.
  - Some of the recommendations that you already inspected and accepted or rejected before the disaster might also be lost and might need to be redone by using the Tooling.

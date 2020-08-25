---

copyright:
  years: 2018, 2020
lastupdated: "2020-08-25"

keywords: batch,batching,batch job

subcollection: compare-comply

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}

# Using batch processing
{: #batching}

The `/v1/batches` APIs enable you to run Compare and Comply methods over a collection of input documents. Batch processing is available only for the `POST /v1/html_conversion`, `POST /v1/element_classification`, `POST /v1/tables`, `POST /v1/mortgage_closing_disclosures`, and `POST /v1/invoices` methods. Batch processing is _not_ available for the `POST /v1/comparison` method.
{: shortdesc}

All batch requests return a batch status object that include a `batch_id`. The `batch_id` can be used to monitor the status of a request and to cancel a request.

Batch-processing requests require access credentials for a Cloud Object Storage (COS) instance and the name of an input and output bucket in that instance. Details are provided in [Before you begin](/docs/compare-comply?topic=compare-comply-batching#before-you-batch).
{: important}

Files submitted through the `/v1/batches` APIs can be up to 50 MB in size.
{: note}

## API endpoints
{: #batching_endpoints}

The batching API endpoints are as follows.

- `POST /v1/batches`: Submit a batch-processing request.
- `GET /v1/batches`: Gets the list of submitted batch-processing jobs.
- `GET /v1/batches/{batch_id}`: Gets information about a specified batch-prcoessing request.
- `PUT /v1/batches/{batch_id}`: Updates a pending or active batch-processing request.

## Before you begin
{: #before-you-batch}

Before you use batch processing, ensure the following are complete:

- All of the items listed in [Before you begin](/docs/compare-comply?topic=compare-comply-getting-started#gs-before-you-begin) in the **Getting started** topic.
- A [Cloud Object Storage (COS)](https://{DomainName}/catalog/cloud-object-storage){:external} instance on the IBM Cloud. For more, information, see the following topics in the COS documentation:
    - [About IBM Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-about-cos#about-cos)
    - [Order storage](/docs/cloud-object-storage?topic=cloud-object-storage-about-cos)
    - [Service credentials](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials)
    - [Bucket permissions](/docs/cloud-object-storage?topic=cloud-object-storage-iam-bucket-permissions)

## Create and run a batch processing request
{: #post-batch}

The `POST /v1/batches` method creates a batch request that takes all documents in a COS bucket and uses them as input documents; applies a {{site.data.keyword.cncshort}} method to them; and writes the resulting output into another COS bucket.

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/batches` method to create and run a batch processing request. The method takes the following input parameters:

- `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
- `function` (**required** `string`): The method to be run against the documents. Possible values are `html_conversion`, `element_classification`, `tables`, `mortgage_closing_disclosures`, and `invoices`.
- `model` (optional `string`): The ID of the data kit for the model to use. The default is `contracts` when the `function` is `html_conversion` or `element_classification`. The default is `tables` when the function is `tables`. The default is `mortgages` when the function is `mortgage_closing_disclosures`. The default is `invoices` when the function is `invoices`.
- `input_credentials_file` (**required** `file`): A JSON file that lists the COS input credentials. At a minimum, the credentials must enable `READ` permissions on the bucket defined by the `input_bucket_name` parameter.
- `input_bucket_location` (**required** `string`): The geographical location of the COS input bucket as listed on the **Endpoint** tab of your COS instance; for example, `us-geo`, `eu-geo`, or `ap-geo`.
- `input_bucket_name` (**required** `string`): The name of the COS input bucket.
- `output_credentials_file` (**required** `file`): A JSON file that lists the COS output credentials. At a minimum, the credentials must enable `READ` and `WRITE` permissions on the bucket defined by the `output_bucket_name` parameter.
- `output_bucket_location` (**required** `string`): The geographical location of the COS output bucket as listed on the **Endpoint** tab of your COS instance; for example, `us-geo`, `eu-geo`, or `ap-geo`.
- `output_bucket_name` (**required** `string`): The name of the COS output bucket.

```bash
curl -X POST -u "apikey:{apikey}" \
"{url}/v1/batches?version=2018-10-15&function=element_classification" \
-F input_credentials_file=@{path/to/input_credentials_file} \
-F input_bucket_location={geography} \
-F input_bucket_name={input_bucket_name} \
-F output_credentials_file=@{path/to/output_credentials_file} \
-F output_bucket_location={geography} \
-F output_bucket_name={output_bucket_name}
```
{: pre}

The values of the `input_credentials_file` and `output_credentials_file` are files that contain the COS service credentials as a JSON object. You can obtain the JSON from the COS web console page on the **Service credentials** tab. The JSON resembles the following:
{: note}

```json
{
  "apikey": "BoKNKUkhOKUIWh-E_0bQ9AAvVYZwaJf9SEG8FMT252p6",
  "endpoints": "https://control.cloud-object-storage.cloud.ibm.com/v2/endpoints",
  "iam_apikey_description": "Auto-generated for key d7cd684c-564b-4b2f-b601-8af688194b60",
  "iam_apikey_name": "Service credentials-1",
  "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Writer",
  "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/a15da04d40cf41c9d761eadd3812309c::serviceid:ServiceId-8729fee9-d6bd-4645-ac30-a321c5accce3",
  "resource_instance_id": "crn:v1:bluemix:public:cloud-object-storage:global:a/a15da04d40cf41c9d761eadd3812309c:391a651c-1cba-49a0-8e84-b5e6d07fe412::"
}
```

The following example command creates and runs a batch request with the following attributes:

- `function`: `element_classification`
- `input_credentials_file`: `/Users/jsmith/cos_input_creds.json`
- `input_bucket_location`: `us-geo`
- `input_bucket_name`: `my_cos_input_bucket`
- `output_credentials_file`: `/Users/jsmith/cos_output_creds.json`
- `output_bucket_location`: `us-geo`
- `output_bucket_name`: `my_cos_output_bucket`

```bash
curl -X POST -u "apikey:{apikey}" \
"{url}/v1/batches?version=2018-10-15&function=element_classification" \
-F input_credentials_file=@/Users/jsmith/cos_input_creds.json \
-F input_bucket_location=us-geo \
-F input_bucket_name=my_cos_input_bucket \
-F output_credentials_file=@/Users/jsmith/cos_output_creds.json \
-F output_bucket_location=us-geo \
-F output_bucket_name=my_cos_output_bucket
```
{: pre}

The method returns the batch-request status as a JSON object, as in the following example.

```json
{
  "function": "element_classification",
  "input_bucket_location": "us-geo",
  "input_bucket_name": "my_cos_input_bucket",
  "output_bucket_location": "us-geo",
  "output_bucket_name": "my_cos_output_bucket",
  "batch_id": "65698e79-d601-4d8f-b46d-dd1369020b58",
  "document_counts": {
    "total": 5,
    "pending": 0,
    "successful": 5,
    "failed": 0
  },
  "status": "completed",
  "created": "2018-07-30T20:26:03.411+0000",
  "updated": "2018-07-30T20:26:09.433+0000"
}
```

The returned status object, which is identical for all `/v1/batches` methods, contains the count of all documents in the batch (`total`), the count of documents that have not yet been processed (`pending`), the count of documents that have been successfully processed (`successful`), and the count of documents that were unable to be processed (`failed`).

### Batch processing workflow
{: #batch-workflow}

Batch processing proceeds as follows:

- When you first submit a batch request, the status is `pending`.
- When the batch engine starts handling documents, the status changes to `active` until the service attempts to process all documents in the batch or the batch job is cancelled by using the [`PUT /v1/batches/{batch_id}` method with the `cancel` action](#put-cancel-batch).
- When all documents in the batch job have been processed, the status changes to `completed`.

## Get a list of submitted batch requests
{: #get-list-batch}

You can get a list of batch requests you have submitted by using the `GET /v1/batches` method.

In a `bash` shell or equivalent environment such as Cygwin, use the `GET /v1/batches` method to get the status of all batch processing requests. The method takes the following input parameters:

- `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
- `status` (optional `string`): An optional status string used to filter the output. Permitted values are `pending`, `active`, `completed`, and `canceled`.

```bash
curl -X GET -u "apikey:{apikey}" "{url}/v1/batches?version=2018-10-15"
```
{: pre}

The method returns a JSON object that contains batch-request status objects, as in the following example:

```json
{
    "batches": [
        {
            "function": "element_classification",
            "input_bucket_location": "us-geo",
            "input_bucket_name": "my_cos_input_bucket",
            "output_bucket_location": "us-geo",
            "output_bucket_name": "my_cos_output_bucket",
            "batch_id": "65698e79-d601-4d8f-b46d-dd1369020b58",
            "document_counts": {
              "total": 5,
              "pending": 0,
              "successful": 5,
              "failed": 0
            },
            "status": "completed",
            "created": "2018-07-30T20:26:03.411+0000",
            "updated": "2018-07-30T20:26:09.433+0000"
        },
        {
            "function": "html_conversion",
            "input_bucket_location": "us-geo",
            "input_bucket_name": "my_cos_input_bucket",
            "output_bucket_location": "us-geo",
            "output_bucket_name": "my_cos_output_bucket",
            "batch_id": "0a7f8ab8-97a0-4b67-9fea-feacafbb0b20",
            "document_counts": {
              "total": 5,
              "pending": 0,
              "successful": 5,
              "failed": 0
            },
            "status": "completed",
            "created": "2018-07-30T19:15:30.197+0000",
            "updated": "2018-07-30T19:15:41.830+0000"
        }
    ]
}
```

## Get the status of a specified batch request
{: #get-spec-batch}

You can get a list of a specific batch requests you have submitted by using the `GET /v1/batches/{batch_id}` method.

In a `bash` shell or equivalent environment such as Cygwin, use the `GET /v1/batches/{batch_id}` method to get the status of a specified batch processing request. The method takes the following input parameters:

- `id` (**required** `string`): The `batch_id` of the batch request you want to query. Obtain the `batch_id` by using the `GET /v1/batches` method as described in [Get a list of submitted batch requests](#get-list-batch).
- `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.

```bash
curl -X GET -u "apikey:{apikey}" "{url}/v1/batches/0a7f8ab8-97a0-4b67-9fea-feacafbb0b20?version=2018-10-15"
```
{: pre}

The method returns a JSON object that provides the status of the specified batch request, as in the following example:

```json
{
    "function": "html_conversion",
    "input_bucket_location": "us-geo",
    "input_bucket_name": "my_cos_input_bucket",
    "output_bucket_name": "my_cos_output_bucket",
    "batch_id": "0a7f8ab8-97a0-4b67-9fea-feacafbb0b20",
    "document_counts": {
      "total": 5,
      "pending": 5,
      "successful": 0,
      "failed": 0
    },
    "status": "pending",
    "created": "2018-07-30T20:10:30.197+0000",
    "updated": "2018-07-30T20:10:41.830+0000"
}
```

## Rescan the input bucket
{: #put-add-batch}

You can have the service check for new documents in the input bucket by using the `PUT /v1/batches/{batch_id}` method with the `rescan` action. The batch request must already exist and have a status of either `pending` or `active`. The method has no effect on batch requests with a status of `completed` or `canceled`. When you run this method, the service scans the input bucket for documents that have been added or updated since the batch was created or since the last scan.

In a `bash` shell or equivalent environment such as Cygwin, use the `PUT /v1/batches/{batch_id}` method to rescan the input bucket. The method takes the following input parameters:

- `action` (**required** `string`): The action to be performed by the method. To check for new to documents to add a batch request, use the `rescan` action.
- `id` (**required** `string`): The `batch_id` of the batch request for which you want to rescan the input bucket. Obtain the `batch_id` by using the `GET /v1/batches` method as described in [Get a list of submitted batch requests](#get-list-batch).
- `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.

```bash
curl -X PUT -u "apikey:{apikey}" "{url}/v1/batches/0a7f8ab8-97a0-4b67-9fea-feacafbb0b20?version=2018-10-15&action=rescan"
```
{: pre}

The method returns a JSON object that provides the status of the specified batch request, as in the following example:

```json
{
  "function": "html_conversion",
  "input_bucket_location": "us-geo",
  "input_bucket_name": "my_cos_input_bucket",
  "output_bucket_name": "my_cos_output_bucket",
  "batch_id": "0a7f8ab8-97a0-4b67-9fea-feacafbb0b20",
  "document_counts": {
    "total": 8,
    "pending": 2,
    "successful": 5,
    "failed": 1
  },
  "status": "active",
  "created": "2018-07-30T20:15:30.197+0000",
  "updated": "2018-07-30T20:16:22.724+0000"
}
```

## Cancel a batch request
{: #put-cancel-batch}

You can cancel the execution of a batch request by using the `PUT /v1/batches/{batch_id}` method with the `cancel` action. If the batch has already completed or was already cancelled, the service disregards the request.

In a `bash` shell or equivalent environment such as Cygwin, use the `PUT /v1/batches/{batch_id}` method to cancel a batch processing request. The method takes the following input parameters:

- `action` (**required** `string`): The action to be performed by the method. To cancel a batch request, use the `cancel` action.
- `id` (**required** `string`): The `batch_id` of the batch request you want to cancel. Obtain the `batch_id` by using the `GET /v1/batches` method as described in [Get a list of submitted batch requests](#get-list-batch).
- `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.

```bash
curl -X PUT -u "apikey:{apikey}" "{url}/v1/batches/0a7f8ab8-97a0-4b67-9fea-feacafbb0b20?version=2018-10-15&action=cancel"
```
{: pre}

The method returns a JSON object that provides the status of the specified batch request, as in the following example:

```json
{
  "function": "html_conversion",
  "input_bucket_location": "us-geo",
  "input_bucket_name": "my_cos_input_bucket",
  "output_bucket_name": "my_cos_output_bucket",
  "batch_id": "0a7f8ab8-97a0-4b67-9fea-feacafbb0b20",
  "document_counts": {
    "total": 8,
    "pending": 0,
    "successful": 5,
    "failed": 0
  },
  "status": "canceled",
  "created": "2018-07-30T20:20:30.197+0000",
  "updated": "2018-07-30T20:21:22.724+0000"
}
```

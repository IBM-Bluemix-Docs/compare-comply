---

copyright:
  years: 2017, 2019
lastupdated: "2019-12-12"

keywords: release notes,beta,beta features,changes,issues

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

# Release notes
{: #release_notes}

The release notes provide information about changes to the {{site.data.keyword.cncfull}} service.

## Beta features
{: #beta_features}

IBM will release services, features, and language support that are classified as beta or experimental. These capacities can be unstable, can change frequently, and can be discontinued with short notice. They are provided so you can evaluate their functionality. A beta or experimental capacity might not provide the same level of performance or compatibility that generally released capacities provide. These capacities are not designed for use in a production environment, and any such use is at your own risk.

IBM is under no obligation to offer migration capabilities or services. You are responsible for removing content you want to retain prior to expiration or termination of the beta service.

## Service API Versioning
{: #api_versioning}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Do not default to the current date. Instead, specify a date that matches a version that is compatible with your application, and do not change it until your application is ready for a later version.

The current version is `2018-10-15`.

## Changes
{: #changes}

The following new features and changes to the service are available.

### 12 December 2019
{: #12-december-2019}

- **Full support for IBM Cloud IAM**

    - {{site.data.keyword.cncshort}} now supports the full implementation of {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). API keys for Watson services are no longer limited to a single service instance. You can create access policies and API keys that apply to more than one service, and you can grant access between services.
    - To support this change, the API service endpoints use a different domain and include the service instance ID. The pattern is `api.{location}.{offering}.watson.cloud.ibm.com/instances/{instance_id}`.

        Example URL for an instance hosted in the Dallas location: `api.us-south.compare-comply.watson.cloud.ibm.com/instances/6bbda3b3-d572-45e1-8c54-22d6ed9e52c2`

        The previous public endpoint domain was `watsonplatform.net`.

        For more information about the URLs, see the [API reference](https://cloud.ibm.com/apidocs/compare-comply/compare-comply#service-endpoint){: external}.

        These URLs do not introduce a breaking change. The new URLs work both for your existing service instances and for new instances. The original URLs continue to work on your existing service instances for at least one year (until December 2020).
    - For more information about IAM, see [Authenticating to Watson services](/docs/watson?topic=watson-iam).
- **New network and data security features**
    - **Support for data encryption with customer-managed keys**

      Users of new premium and dedicated instances can integrate {{site.data.keyword.keymanagementservicefull}} with {{site.data.keyword.cncshort}} to encrypt their data and manage encryption keys. For more information, see [Protecting sensitive information in your Watson service](/docs/compare-comply?topic=watson-keyservice).
    - **Support for private network endpoints**

      Users of Premium plans can create private network endpoints to connect to {{site.data.keyword.cncshort}} over a private network. Connections to private network endpoints do not require public internet access. For more information, see [Public and private network endpoints](/docs/compare-comply?topic=watson-public-private-endpoints).

### 30 August 2019
{: #30-august-2019}

The release includes the following updates:

 - The output of the **Purchase order understanding** method now includes the new fields `tax_ids`, `tax_totals`, and `quote_numbers`. For more information, see For more information, see [Understanding purchase-order parsing](/docs/compare-comply?topic=compare-comply-pos).

### 31 July 2019
{: #31-july-2019}

The release includes the following updates:

  - The addition of the `contract_currencies` array to the output of the **Element Classification** method. For more information, see [Classifying elements](/docs/compare-comply?topic=compare-comply-output_schema) and [Understanding element classification](/docs/compare-comply?topic=compare-comply-contract_parsing#contract-currencies).

   - The addition of the `text_normalized` element to the `purchase_order_dates` and `due_dates` arrays to the output of the **Purchase order understanding** method. For more information, see [Understanding purchase-order parsing](/docs/compare-comply?topic=compare-comply-pos).

### 16 July 2019
{: #16-july-2019}

The release includes the following updates:

  - The addition of the `text_normalized` and `interpretation` elements to the `contract_amounts`, `contract_terms`, and `payment_terms` arrays to the output of the **Element Classification** method. For more information, see [Classifying elements](/docs/compare-comply?topic=compare-comply-output_schema) and [Understanding element classification](/docs/compare-comply?topic=compare-comply-contract_parsing).

  - The addition of the `title` element to the `tables` array in the output of the **Element Classification** and **Tables** methods. For more information, see [Classifying tables](/docs/compare-comply?topic=compare-comply-understanding_tables) and [Classifying elements](/docs/compare-comply?topic=compare-comply-output_schema).

### 19 June 2019
{: #19-june-2019}

The release includes the following updates:

  - A new category, `Order of Precedence`, has been introduced. For more information, see [Understanding element classification](/docs/compare-comply?topic=compare-comply-contract_parsing#contract_categories).
  - The **Purchase Order Understanding** beta feature now includes `provenance_ids` arrays for each identified object. For more information, see [Understanding purchase-order parsing](/docs/compare-comply?topic=compare-comply-pos).
  - The **Invoice Understanding** beta feature now includes `text_normalized` fields for the `invoice_dates` and `due_dates` elements. For more information, see [Understanding invoice parsing](/docs/compare-comply?topic-conmpare-comply-invoices).

### 23 April 2019
{: #23-april-2019}

The release includes updates to the {{site.data.keyword.cncshort}} Tooling. For more information, see [Using the {{site.data.keyword.cncshort}} Tooling](/docs/compare-comply?topic=compare-comply-using_tool).

### 17 April 2019
{: #17-april-2019}

The release includes the following updates:

  - The addition of the `contract_terms` array to the output of the **Element Classification** method. For more information, see [Contract terms](/docs/compare-comply?topic=compare-comply-contract_parsing#contract-terms) and [Classifying elements](/docs/compare-comply?topic=compare-comply-output_schema).
  
  - The addition of the `text_normalized` element to the `effective_dates` and `termination_dates` arrays in the output of the **Element Classification** method. For more information, see [Other arrays](/docs/compare-comply?topic=compare-comply-getting-started#other_arrays), [Effective dates](/docs/compare-comply?topic=compare-comply-contract_parsing#effective_dates), and [Termination dates](/docs/compare-comply?topic=compare-comply-contract_parsing#termination_dates).

### 19 March 2019
{: #19-march-2019}

The release includes the following updates:

  - Updates to the `attributes` values available in Element Classification. For more information, see [Attributes](/docs/compare-comply?topic=compare-comply-contract_parsing#attributes). 

    The `Address` attribute value has been deprecated.
    {: important}
  
  - Documentation updates for metadata fields. For more information, see the descriptions beginning at [Effective dates](/docs/compare-comply?topic=compare-comply-contract_parsing#effective_dates).
  
  - Beta release of an analysis model for purchase orders, which you can access by using the `/v1/purchase_orders` method. For more information, see [Understanding purchase-order parsing](/docs/compare-comply?topic=compare-comply-pos). For important information about beta features, see [Beta features](#beta_features). <br/>
  **Note:** Only the `curl` API is available for this method. The method is not currently provided in other SDKs (Go, Java, Python, Node.js, Ruby, Swift).

### 5 March 2019
{: #5-march-2019}

The release includes the following updates:

  - Additional enhancements to the {{site.data.keyword.cncshort}} Tooling. For more information, see [Using the {{site.data.keyword.cncshort}} Tooling](/docs/compare-comply?topic=compare-comply-using_tool).
  - The output of the **Table understanding** method includes a new array called `key_value_pairs` that shows key-value pairs that the service extracted from different cells in a table. For more information, see [Understanding key-value pairs](/docs/compare-comply?topic=compare-comply-understanding_tables#key-value-pairs).

### 8 January 2019
{: #8-jan-2019}

The release includes the following updates:

  - The output of the `/v1/tables` method now includes an `attributes` array for each body cell. For more information, see [Classifying elements](/docs/compare-comply?topic=compare-comply-output_schema) and [Attributes](/docs/compare-comply?topic=compare-comply-output_schema#attributes).
  - The output of the `/v1/element_classification` method now includes the following:
    - The `parties` array now includes an `importance` field that indicates whether the party is a `Primary` party or an `Unknown` (non-primary) party.
    - The `effective_dates`, `contract_amounts`, and `termination_dates` arrays now each include a `confidence_level` field that indicates a value of `High`, `Medium`, or `Low`.
    For more information, see [Classifying elements](/docs/compare-comply?topic=compare-comply-output_schema) and [Understanding element classification](/docs/compare-comply?topic=compare-comply-contract_parsing).
  - The {{site.data.keyword.cncshort}} Tooling includes several enhancements and bug fixes. For more information, see [Using the {{site.data.keyword.cncshort}} Tooling](/docs/compare-comply?topic=compare-comply-using_tool).

### General Availability release, 6 December 2018
{: #6-dec-2018}

The {{site.data.keyword.cncshort}} service is now generally available. The GA release includes the following new features and enhancements:

  - Beta release of an analysis model for invoices, which you can access by using the `/v1/invoices` method. For more information, see [Understanding invoice parsing](/docs/compare-comply?topic=compare-comply-invoices). For important information about beta features, see [Beta features](#beta_features). <br/>
    **Note:** Only the `curl` API is available for this method. The method is not currently provided in other SDKs (Java, Python, Node.js, Ruby).
  - The output of the `POST /v1/comparison` method now includes a boolean named `significant_elements` that indicates if the aligned text contains contractual clauses of significance.. For more information, see [Comparing two documents](/docs/compare-comply?topic=compare-comply-compare).
  - The service accepts Microsoft Word files (DOC, DOCX). For more information, see [Supported input formats](/docs/compare-comply?topic=compare-comply-formats). 
  
The following table lists file-type support by method.

| Method           |PDF support   |Word support     |Image support        |Text support    |
|------------------|-----------------|-----------------------------------------|
|Tooling*           | Supported    | Supported | All supported image formats | **Not** supported |
|`/v1/html_conversion`| Supported | Supported | All supported image formats | Supported |
|`/v1/element_classification`| Supported | Supported | All supported image formats | **Not** supported |
|`/v1/tables`      | Supported | Supported | All supported image formats | Supported |
|`/v1/invoices`    | Supported | Supported | All supported image formats | **Not** supported |
|`/v1/comparison`*  | Supported | Supported | All supported image formats | **Not** supported |

\* The Tooling and `/v1/comparison` method also accept JSON files from the output of the `/v1/element_classification` method.
{: note}

### 9 November 2018
{: #9nov2018}

{{site.data.keyword.cncshort}} now has the ability to process certain image files and text files as listed at [Supported input formats](/docs/compare-comply?topic=compare-comply-formats).

The service can process "plain" text (ASCII) files that use a monospaced font and page breaks. Richer text formats that include non-monospaced fonts and style attributes such as bold and italics are not yet supported. If you need to process an enriched text file, convert it to PDF before submitting it to the service.

Supported image formats currently include the following. 
  - BMP
  - GIF
  - JPEG
  - JPEG2000
  - PNG
  - RAW
  - TIFF

See the table at [General Availability release, 6 December 2018](#6-dec-2018) for supported file types.

The `/v1/feedback` methods do not accept image or text files. 

The `/v1/batches` methods accept images and text files according to the method called in the batch job. For example, if your batch job calls the `/v1/html_conversion` method, it accepts both images and text files. Similarly, if your batch job calls the `/v1/element_classification` method, it accepts images but not text files.

### 30 October 2018
{: #30oct2018}

Changes to the service's API and output schema are ongoing throughout the course of the beta. The API and output schema will be stabilized when the service becomes generally available (GA). When writing applications that access the service, be aware that the API calls and output can change in a future release; applications that access the service therefore need not to be used in a production environment. The following list summarizes the major API and schema changes in the current release.
{: important}

  - A new API version date (`2018-10-15`). If you specify an API version date earlier than `2018-10-15`, you call an older API that most likely has different method names and parameters than those documented for the current release.
  - Changes to the output schema for the `/v1/element_classification` method. See [Getting started](/docs/compare-comply?topic=compare-comply-getting-started) and [Classifying elements](/docs/compare-comply?compare-comply-output_schema) for details.
  - Changes to the `/v1/tables` method's output schema. See [Classifying elements](/docs/compare-comply?topic=compare-comply-output_schema) and [Classifying tables](/docs/compare-comply?topic=compare-comply-understanding_tables) for information about the table parsing format.
  - Changes to the input and output parameters in the `/v1/feedback` and `/v1/feedback/{feedback_id}` methods. See [Using the feedback APIs](/docs/compare-comply?topic=compare-comply-feedback).

### 30 August 2018

Beta release with the following features that have been introduced since the experimental release:

  - Enhanced output including a structural analysis of input documents, as described at [Understanding document structure](/docs/compare-comply?topic=compare-comply-doc_struct).
  - New `batches` APIs to run {{site.data.keyword.cncshort}} methods over a set of documents, as described at [Using batch processing](/docs/compare-comply?topic=compare-comply-batching).
  - The ability to parse scanned PDF files that have been processed by an optical character reader (OCR).
  - New `feedback` APIs to enable subject-matter experts to suggest feedback for future updates to the training model, as described at [Using the feedback APIs](/docs/compare-comply?topic=compare-comply-feedback).
  - Improved table classification, as described at [Classifying tables](/docs/compare-comply?topic=compare-comply-understanding_tables).
  - Tooling for working visually on documents, as described at [Using the {{site.data.keyword.cncshort}} Tooling](/docs/compare-comply?topic=compare-comply-using_tool).


## Known issues
{: #known_issues}

- The maximum size of an input file that can be uploaded to the service in interactive mode (that is, with a method not called by the `/v1/batches` interface, as well as in the {{site.data.keyword.cncshort}} Tooling) is 1.5 MB. Files submitted through the `/v1/batches` interface can be up to 50 MB. See [Using batch processing](/docs/compare-comply?topic=compare-comply-batching) for information on the `/v1/batches` interface.
- PDFs with security enabled cannot be parsed.
- Documents with non-standard page layouts (such as 2 or 3 columns per page) do not parse correctly.

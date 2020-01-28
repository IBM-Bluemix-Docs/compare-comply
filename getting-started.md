---

copyright:
  years: 2017, 2020
lastupdated: "2020-01-28"

keywords: compare and comply,compare,comply,compare comply,getting started,analyze contracts,analyze documents,classify contracts,analyze contracts,element classification,classify elements,analyze tables,tables,CompareComply,CompareAndComply,compare and comply project,compare comply project

subcollection: compare-comply

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:hide-dashboard: .hide-dashboard}
{:apikey: data-credential-placeholder='apikey'}
{:url: data-credential-placeholder='url'}

# Getting started with {{site.data.keyword.cncshort}}
{: #getting-started}

This tutorial guides you through classifying the elements of a contract with {{site.data.keyword.cncfull}}. You use the default model to analyze the contract.
{: shortdesc}

## Where to start
{: #start-options}

You can get started with the service by using either the API or the service tooling.

### Get started with the API
{: #gs-api}

This tutorial uses the **Element classification** feature. Other service methods have similar input syntax and output formats. For more information, see the pages for other methods.

### Get started with the tooling
{: #gs-tool}

Optionally, you can explore the service's features by using the {{site.data.keyword.cncshort}} tooling. See [Using the {{site.data.keyword.cncshort}} Tooling ](/docs/services/compare-comply?topic=compare-comply-using_tool) for more information.

### Request limited preview features
{: #request-preview-features}

{{site.data.keyword.cncshort}} has the following beta and experimental features that can be accessed by request:

  -  Invoice Understanding: {{site.data.keyword.cncshort}} finds and extracts important information such as buyer, supplier, invoice date, and amount owed. Fill out the following [form](http://ibm.biz/invoices){: external} to request access to this feature.
  - Purchase Order Understanding: {{site.data.keyword.cncshort}}extracts important information in purchase orders. Fill out the following [form](https://datasciencex.typeform.com/to/Fjyf6t){: external} to request access to this feature.

## Before you begin
{: #gs-before-you-begin}

- {: hide-dashboard} Create an instance of the service:
    1.  Go to the [{{site.data.keyword.cncshort}} page](https://{DomainName}/catalog/services/compare-comply){: external} in the {{site.data.keyword.cloud_notm}} catalog.
    1.  Sign up for a free {{site.data.keyword.cloud_notm}} account or log in.
    1.  Click **Create**.
- {: hide-dashboard} Copy the credentials to authenticate to your service instance:
    1.  On the **Manage** page, click **Show Credentials**.
    1.  Copy the `API Key` and `URL` values.
- Make sure that you have the `curl` command.
    - Test whether `curl` is installed. Run the following command on the command line. If the output lists the `curl` version with SSL support, you are set for the tutorial.

        ```sh
        curl -V
        ```
        {: pre}

    - If necessary, install a version with SSL enabled from [curl.haxx.se](https://curl.haxx.se/){: external}. Add the location of the file to your PATH environment variables if you want to run `curl` from any command-line location.

When you enter a command, replace `{apikey}` and `{url}` with your actual API key and URL. Omit the braces, which indicate a variable value, from the command. An actual value resembles the following example. The URL in the example is for a service instance in the US South (Dallas, Texas).
{: hide-dashboard}

```bash
curl -X POST -u "apikey:L_HALhLVIksh1b73l97LSs6R_3gLo4xkujAaxm7i-b9x"
. . .
"https://api.us-south.compare-comply.watson.cloud.ibm.com/compare-comply/api/v1/html_conversion?version=2018-10-15"
```
{:pre}
{: hide-dashboard}

## Step 1: Identify content
{: #identify_content}

Identify appropriate documents to analyze. {{site.data.keyword.cncshort}} can analyze contractual documents that meet the criteria that are listed in [Supported input formats](/docs/services/compare-comply?topic=compare-comply-formats).

For the example in this tutorial, the file must be in PDF or a supported image format.
  
  You can submit PDF files that were scanned and processed by an optical character reader (OCR).
  {: tip}

- Files can be up to 1.5 MB when submitted to the service with individual methods. If you submit files through the [`/v1/batches` interface](/docs/services/compare-comply?topic=compare-comply-batching), files can be up to 50 MB.
- The service cannot process secure PDFs (with a password to open) or restricted PDFs (with a password to edit).

## Step 2: Classify a contract's elements
{: #parse_contract}

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/element_classification` method to classify your contract. The method takes the following input parameters:
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use to process the request.
  - `file` (**required** `file`): The input file that is to be classified.
  - `model` (optional `string`): If this parameter is specified, the service runs the specified type of element classification. Currently, the only supported value is `contracts`.

Replace `{apikey}` with the API key you copied earlier and `{input_file}` with the path to the file to parse.

```sh
curl -X POST -u "apikey:{apikey}" -F "file=@{input_file}" https://{url}/compare-comply/api/v1/element_classification?version=2018-10-15
```
{: codeblock}

The method returns a JSON object that contains:

  - [A `documents` object](#documents) that includes the input document's title, an HTML version of the input document, and the MD5 hash of the input document.
  - Information about the model used to classify the input document.  
  - [An `elements` array](#elements) that details semantic elements that are identified in the input document.
  - [A `tables` array](#tables) that breaks down the tables that are identified in the input document.
  - [A `document_structure` object](#doc_structure) that lists section titles and leading sentences that are identified in the input document.
  - [A `parties` array](#parties) that lists the parties and each party's roles, addresses, contact information, and mentions identified in the input document.
  - [Arrays defining `effective_dates`, `contract_amounts`, `termination_dates`, `contract_types`, `contract_terms`, `payment_terms`, and `contract_currencies`](#other_arrays).

## Step 3: Review the analysis
{: #review_analysis}

This section provides a high-level overview of the output of the `POST /v1/element_classification` method, focusing on the major sections. See [Classifying elements](/docs/services/compare-comply?topic=compare-comply-output_schema) for a detailed discussion of the method's output.

### Documents
{: #documents}

The `documents` object provides basic information about the input document.

### Elements
{: #elements}

Each object in the `elements` array describes an element of the contract that {{site.data.keyword.cncshort}} identified. The following code shows a typical element:

```json
{
    "location" : {
      "begin" : 134323,
      "end" : 135109
    },
    "text" : "9. In the event that the Participant's total vested account balance is determined to be less than or equal to $2,000.00 as of the date that the Order is received, the parties will be informed in writing that the QDRO determination fee may potentially liquidate the account.",
    "types" : [ {
      "label" : {
        "nature" : "Obligation",
        "party" : "All Parties"
      },
      "provenance_ids" : ["Nlu0ogWAEGms4vjhhzpMv3iXhm8b8fBqMBNtT/bXH8JI=", "PlyERkjg5is36RpFjVUFXp69eDmGmCxLCXRs1sDMDUCo="
      ]
    } ],
    "categories" : [ {
      "label" : "Communication",
      "provenance_ids" : [ "Cs38YyU6VBFtJK1/bgtEJBlqqWmX5F6OYUciRxQXf7HrN5TOCPuI7QXbkbj4LRXoxVuB3/i9H15q5TU+vFxorhUBeWFfF998OYQiPYViD2yI="
      ]
    } ],
    "attributes" : [ {
      "type" : "Currency",
      "text" : "$2,000.00",
      "attribute" : {
        "begin" : 134780,
        "end" : 134789
      }
    } ]
}
```

Each element has five important sections:
  - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document. 
  - `text`: The text of the classified element.
  - `types`: An array that includes zero or more `label` objects. Each `label` object includes a `nature` field that lists the effect of the element on the identified party (for example, `Right` or `Exclusion`) and a `party` field that identifies the party or parties that are affected by the element. For more information, see [Types](/docs/services/compare-comply?topic=compare-comply-contract_parsing#types) in [Understanding element classification](/docs/services/compare-comply?topic=compare-comply-contract_parsing). 
  - `categories`: An array that contains zero or more `label` objects. The value of each `label` object lists a functional category into which the identified element falls. For more information, see [Categories](/docs/services/compare-comply?topic=compare-comply-contract_parsing#contract_categories) in [Understanding element classification](/docs/services/compare-comply?topic=compare-comply-contract_parsing). 
  - `attributes`: An array that lists zero or more objects that define attributes of  the element. Currently supported attribute types include `Currency`, `DateTime`, `DefinedTerm`, `Duration`, `Location`, `Number`, `Organization`, `Percentage`, and `Person`. Each object in the attributes array also includes the identified element's text and location; location is defined by the begin and end indexes of the text in the input document. For more information, see [Attributes](/docs/services/compare-comply?topic=compare-comply-contract_parsing#attributes) in [Understanding element classification](/docs/services/compare-comply?topic=compare-comply-contract_parsing).
  
Additionally, each object in the `types` and `categories` arrays includes a `provenance_ids` array. The values that are listed in the `provenance_ids` array are hashed values that you can send to IBM to provide feedback or receive support about the part of the analysis that is associated with the element.

Some sentences do not fall under any type or category, in which case the service returns the `types` and `categories` arrays as empty objects.
{: note}

Some sentences cover multiple topics, in which case the service returns multiple sets of `types` and `categories` objects.
{: note}
  
Some sentences do not contain any identifiable attributes, in which case the service returns the `attributes` array as empty objects.
{: note}


### Tables
{: tables}

The `tables` array details the structure and content of any tables that are found in the input document. See [Classifying tables](/docs/services/compare-comply?topic=compare-comply-understanding_tables) and [Classifying elements](/docs/services/compare-comply?topic=compare-comply-output_schema) for details.

### Document structure
{: #doc_structure}

The `document_structure` object identifies the section titles and leading sentences of the input document. See [Understanding document structure](/docs/services/compare-comply?topic=compare-comply-doc_struct) for details.

### Parties
{: #parties}

The `parties` array lists available information about parties that are affected by the input document, including the party's name, role, importance, address or addresses, and contacts. The array also lists all mentions of the party in the input document. For more information, see [Parties](/docs/services/compare-comply?topic=compare-comply-contract_parsing#contract_parties) in [Understanding element classification](/docs/services/compare-comply?topic=compare-comply-contract_parsing).

```json
{
  ...
  "parties": [
      {
      "party": "Wolfbone Investments, LLC",
      "role": "Supplier",
      "importance": "Primary",
      "addresses": [ ],
      "contacts": [
        {
         "name": "Will Smith",
         "role": "business contact"
        },
        ...
      ],
      "mentions": [ ]
    },
    {
      "party": "Torchlight Energy, Inc.",
      "role": "Buyer",
      "importance": "Primary",
      "addresses": [
       {
       "text": "5700 W. Plano Pkwy., Ste. 3600, Plano, Texas 75093",
       "location": {
          "begin": 150,
          "end": 200
        }
       },
       ...
      ],
      "contacts": [ ],
      "mentions": [ ]
    }
  ]
  ...
}
```
  
### Other arrays
{: #other_arrays}

The following arrays provide useful information about the input document. 

  - The `effective_dates` array lists the date or dates on which the document becomes effective.
  - The `contract_amounts` array lists monetary amounts that identify the total amount of the contract that needs to be paid from one party to another.
  - The `termination_dates` array lists the date or dates on which the document is to be terminated.
  - The `contract_types` array lists the contract type or types as specified in the document.
  - The `contact_terms` array lists the contract duration or durations as specified in the document.
  - The `payment_terms` array lists the duration or durations of expected payment as specified in the document.
  - The `contract_currencies` array lists the contract currency or currencies as specified in the document.
  
Each of the arrays contains zero or more objects that list the following items:

  - The text in which the information was identified.
  - The normalized text, if applicable. This element is currently listed only in the `effective_dates`, `termination_dates`, `contract_amounts`, `contract_terms`, `payment_terms`, and `contract_currencies` arrays.
  - The interpretation of the normalized text, if applicable. This element is currently listed only in the `contract_amounts`, `contract_terms`, and `payment_terms` arrays.
  - The confidence level of the identification (`High`, `Medium`, or `Low`).
  - The `location` of that text as defined by the text's `begin` and `end` indexes.
  - A list of `provenance_ids`, which are hashed values that you can send to IBM to provide feedback or receive support.

## Next steps
{: #next_steps}

You successfully classified a contract to identify its elements, tables, structure, parties, and other information. You can use the analysis to quickly understand and enforce the classified contract. The next steps are:

 - [Understanding element classification](/docs/services/compare-comply?topic=compare-comply-contract_parsing)
 - [Classifying elements](/docs/services/compare-comply?topic=compare-comply-output_schema) and [classifying tables](/docs/services/compare-comply?topic=compare-comply-understanding_tables)



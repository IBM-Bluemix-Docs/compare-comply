---

copyright:
  years: 2019
lastupdated: "2019-06-19"

subcollection: compare-comply

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:important: .important}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Understanding purchase-order parsing
{: #pos}

You can analyze purchase orders by using the **Purchase order understanding** feature.
{: shortdesc}

Purchase Order Understanding is a beta feature. For information about beta features, see [Beta features](/docs/services/compare-comply?topic=compare-comply-release_notes#beta_features) in the [Release notes](/docs/services/compare-comply?topic=compare-comply-release_notes). You must request access to purchase-order parsing by completing the following [form](https://datasciencex.typeform.com/to/Fjyf6t).
{: important}

You can parse and classify the contents of purchase orders in your [input document](/docs/services/compare-comply?topic=compare-comply-formats) by calling the `POST /v1/purchase_orders` method. 

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/purchase_orders` method to classify a purchase order. The method takes the following input parameters:
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `file` (**required** `file`): The input file that is to be parsed for purchase-order information.

Replace `{apikey}` with the API key you copied earlier and `{input_file}` with the path to the input file to parse.

```bash
curl -X POST -u "apikey:{apikey}" -F 'file=@{input_file}' https://gateway.watsonplatform.net/compare-comply/api/v1/purchase_orders?version=2018-10-15
```
{: codeblock}

The command output uses the following schema.

```json
{
   "model_id": "purchase_orders",
   "model_version": string,
   "document": {
      "title": string,
      "html": string,
      "hash": string
   },
   "buyers": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "purchase_order_numbers":[  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "purchase_order_dates": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "due_dates": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "payment_terms": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "currencies": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "suppliers": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "supplier_ids": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "bill_to": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "ship_to": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "invoice_to": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ],
   "line_items": [  
      {  
         "item_description": {  
            "location": {  
               "begin": int,
               "end": int
            },
            "text": string,
            "provenance_ids": [string, string, ...]
         },
         "quantity_ordered": {  
            "location": {  
               "begin": int,
               "end": int
            },
            "text": string,
            "provenance_ids": [string, string, ...]
         },
         "unit_price": {  
            "location": {  
               "begin": int,
               "end": int
            },
            "text": string,
            "provenance_ids": [string, string, ...]
         }
      }
   ],
   "total_amounts": [  
      {  
         "location": {  
            "begin": int,
            "end": int
         },
         "text": string,
         "provenance_ids": [string, string, ...]
      }
   ]
}

```

The schema is arranged as follows.

  - `model_id`: The analysis model used by the service. For the `/v1/purchase_orders` method, the value is `purchase_orders`.
  - `model_version`: The version of the analysis model specified by the value of the `model_id` parameter.
  - `document`: An object listing basic information about the document, including:
    - `title`: The document title, if detected.
    - `html`: The full text of the input document in HTML format.
    - `hash`: The MD5 hash of the input document.
  - `buyers`: An array of buyers that are listed in the purchase order. A buyer is defined as a party responsible paying for the goods, services, or both.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text that pertains to a buyer or buyers.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `purchase_order_numbers`: An array of purchase-order numbers that are listed in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text that identifies a purchase-order number.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `purchase_order_dates`: An array of purchase-order dates that are listed in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a purchase-order date.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `due_dates`: An array of due dates that are listed in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a due date.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `payment_terms`: An array of payment terms that are listed in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding payment terms.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `currencies`: An array of currencies that are listed in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding currencies, in the form of a [currency code](https://www.iso.org/iso-4217-currency-codes.html){: external}.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `suppliers`: An array of suppliers that are listed in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a supplier or suppliers.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `supplier_ids`: An array of supplier identifiers that are listed in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a supplier identifier or identifiers.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `bill_to`: An array of buyer names that are listed in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a buyer name or names.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `ship_to`: An array of shipping addresses that are identified in the input document.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a shipping address or addresses.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `invoice_to`: An array of buyers who receive purchase orders.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a buyer or buyers who receive purchase orders.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `line_items`: An array that describes the line items that are identified in the input document. Each line item consists of the following objects:
    - `item_description`: A description of the item.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
      - `text`: Text describing the line item.
      - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
    - `quantity_ordered`: The quantity of the item that is being ordered.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
      - `text`: The quantity ordered.
      - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
    - `unit_price`: The unit price per item.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
      - `text`: The unit price of the specified item.
      - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `total_amounts`: An array that specifies the total amount or amounts due for the purchase order.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: The total amount or amounts.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
    
Sample output resembles the following:

```json
{
  "document" : {
    "title" : "Microsoft Word - City of Bhopal-PO.docx",
    "html" : "<?xml version='1.0' encoding='UTF-8' standalone='yes'?><html>...</html>",
    "hash" : "3683b8cca234462b39674440db07f7eb"
  },
  "model_id" : "purchase_orders",
  "model_version" : "0.1.4",
  "due_dates" : [ ],
  "currencies" : [ {
    "location" : {
      "begin" : 8960,
      "end" : 8963
    },
    "text" : "USD",
    "provenance_ids": ["P12234"]
  } ],
  "payment_terms" : [ {
    "location" : {
      "begin" : 6627,
      "end" : 6633
    },
    "text" : "30 Net",
    "provenance_ids": ["P12234"]
  } ],
  "total_amounts" : [ {
    "location" : {
      "begin" : 10833,
      "end" : 10840
    },
    "text" : "1000.00",
    "provenance_ids": ["P12234"]
  } ],
  "bill_to" : [ ],
  "purchase_order_dates" : [ {
    "location" : {
      "begin" : 3741,
      "end" : 3752
    },
    "text" : "27-JAN-2018",
    "provenance_ids": ["P12234"]
  } ],
  "invoice_to" : [ ],
  "purchase_order_numbers" : [ {
    "location" : {
      "begin" : 2796,
      "end" : 2804
    },
    "text" : "78237634",
    "provenance_ids": ["P12234"]
  } ],
  "buyers" : [ {
    "location" : {
      "begin" : 11180,
      "end" : 11191
    },
    "text" : "Vinay Kumar",
    "provenance_ids": ["P12234"]
  } ],
  "ship_to" : [ {
    "location" : {
      "begin" : 1612,
      "end" : 1633
    },
    "text" : "45 Main Street Bhopal",
    "provenance_ids": ["P12234"]
  } ],
  "suppliers" : [ {
    "location" : {
      "begin" : 2011,
      "end" : 2026
    },
    "text" : "IBM Corporation",
    "provenance_ids": ["P12234"]
  } ],
  "line_items" : [ {
    "quantity_ordered" : {
      "location" : {
        "begin" : 9669,
        "end" : 9670
      },
      "text" : "1",
      "provenance_ids": ["P12234"]
    },
    "item_description" : {
      "location" : {
        "begin" : 9420,
        "end" : 9442
      },
      "text" : "IBM SW Annual License ",
      "provenance_ids": ["P12234"]
    },
    "unit_price" : {
      "location" : {
        "begin" : 10128,
        "end" : 10136
      },
      "text" : "1000.00 ",
      "provenance_ids": ["P12234"]
    }
  }, {
    "quantity_ordered" : {
      "location" : {
        "begin" : 10361,
        "end" : 10362
      },
      "text" : "1",
      "provenance_ids": ["P12234"]
    },
    "item_description" : {
      "location" : {
        "begin" : 9420,
        "end" : 9442
      },
      "text" : "IBM SW Annual License ",
      "provenance_ids": ["P12234"]
    },
    "unit_price" : {
      "location" : {
        "begin" : 10128,
        "end" : 10136
      },
      "text" : "1000.00 ",
      "provenance_ids": ["P12234"]
    }
  } ],
  "supplier_ids" : [ {
    "location" : {
      "begin" : 5124,
      "end" : 5129
    },
    "text" : "98457",
    "provenance_ids": ["P12234"]
  } ]
}
```

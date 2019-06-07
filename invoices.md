---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-06"

subcollection: compare-comply

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:note: .note}
{:important: .important}

# Understanding invoice parsing
{: #invoices}

You can analyze invoice documents by calling the `POST /v1/invoices` method.
{: shortdesc}

Invoice parsing is a beta feature. For information about beta features, see [Beta features](/docs/services/compare-comply?topic=compare-comply-release_notes#beta_features) in the [Release notes](/docs/services/compare-comply?topic=compare-comply-release_notes). You must request access to invoice parsing by completing the following [form](http://ibm.biz/invoices).
{: important}

You can classify the contents of invoices in your [input document](/docs/services/compare-comply?topic=compare-comply-formats) by using the `POST /v1/invoices` method. 

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/invoices` method to classify an invoice. The method takes the following input parameters:
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `file` (**required** `file`): The input file that is to be classified for invoice information.

Replace `{apikey}` with the API key you copied earlier and `{input_file}` with the path to the input file to parse.

```bash
curl -X POST -u "apikey:{apikey}" -F 'file=@{input_file}' https://gateway.watsonplatform.net/compare-comply/api/v1/invoices?version=2018-10-15
```
{: codeblock}

The command output uses the following schema.

```json
{
    "model_id": "invoices",
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
            "provenance_ids": [ string, string, ... ]
        }
    ],
    "suppliers": [
        {
            "location": {
                "begin": int,
                "end": int
            },
            "text": string,
            "provenance_ids": [ string, string, ... ]
        }
    ],
    "totals_due": [
        {
            "amount": {
                "location": {
                    "begin": int,
                    "end": int
                },
                "text": string,
                "provenance_ids": [ string, string, ... ]
            },
            "currency": {
                "location": {
                    "begin": int,
                    "end": int
                },
                "text": string,
                "provenance_ids": [ string, string, ... ]
            }
        }
    ],
    "currencies": [
        {
            "location": {
                "begin": int,
                "end": int
            },
            "text": string,
            "provenance_ids": [ string, string, ... ]
        }
    ],
    "purchase_order_numbers": [
        {
            "location": {
                "begin": int,
                "end": int
            },
            "text": string,
            "provenance_ids": [ string, string, ... ]
        }
    ],
    "invoice_numbers": [
        {
            "location": {
                "begin": int,
                "end": int
            },
            "text": string,
            "provenance_ids": [ string, string, ... ]            
        }
    ],
    "invoice_dates": [
        {
            "location": {
                "begin": int,
                "end": int
            },
            "text": string,
            "provenance_ids": [ string, string, ... ]
        }
    ],
    "due_dates": [
        {
            "location": {
                "begin": int,
                "end": int
            },
            "text": string,
            "provenance_ids": [ string, string, ... ]
        }
    ],
    "invoice_parts": [
        {
            "quantity_ordered": {
                "location": {
                    "begin": int,
                    "end": int
                },
                "text": string,
                "provenance_ids": [ string, string, ... ]
            },
            "part_description": {
                "location": {
                    "begin": int,
                    "end": int
                },
                "text": string,
                "provenance_ids": [ string, string, ... ]
            },
            "unit_price": {
                "amount": {
                    "location": {
                        "begin": int,
                        "end": int
                    },
                    "text": string,
                    "provenance_ids": [ string, string, ... ]
                },
                "currency": {
                    "location": {
                        "begin": int,
                        "end": int
                    },
                    "text": string,
                    "provenance_ids": [ string, string, ... ]
                }
            }
        }
    ]
}
```

The schema is arranged as follows.

  - `model_id`: The analysis model used by the service. For the `/v1/invoices` method, the value is `invoices`.
  - `model_version`: The version of the analysis model specified by the value of the `model_id` parameter.
  - `document`: An object listing basic information about the document, including:
    - `title`: The document title, if detected.
    - `html`: The full text of the input document in HTML format.
    - `hash`: The MD5 hash of the input document.
  - `buyers`: An array of buyers listed in the invoice. A buyer is defined as a party responsible paying for the goods, services, or both.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a buyer or buyers.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `suppliers`: An array of suppliers listed in the invoice. A supplier is defined as a party responsible for providing the goods, services, or both.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: Text regarding a supplier or suppliers.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `totals_due`: Total amount due after taxes.
    - `amount`: An amount due.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
      - `text`: The amount due.
      - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
    - `currency`: The currency in which the specified amount is due.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
      - `text`: The name of the currency in which the specified amount is due.
      - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `currencies`: An array of currencies listed in the invoice.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: The name of the currency in which the specified amount is due.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `purchase_order_numbers`: An array listing purchase-order (PO) numbers in the invoice. The PO is a document that is generated by the buyer to authorize a purchase transaction. The PO number uniquely identifies a purchase order and is generally defined by the buyer. The buyer matches the PO number in the invoice to the purchase order.
    - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `text`: A PO number.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `invoice_numbers`: An array of invoice numbers. An invoice number is a number assigned by a part to identify the unique invoice.
    - `location`: The location of the invoice number as defined by its `begin` and `end` indexes.
    - `text`: An invoice number.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `invoice_dates`: An array of invoice dates. An invoice date is the date on which the invoice was issued.
    - `location`: The location of the invoice date as defined by its `begin` and `end` indexes.
    - `text`: The invoice date.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `due_dates`: An array of due dates. A due date is the date on which a payment for a certain invoice is due. It is typically defined in the contract as a certain number of days following the invoice date.
    - `location`: The location of the due date as defined by its `begin` and `end` indexes.
    - `text`: The due date.
    - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
  - `invoice_parts`: An array of invoice parts. An invoice part is a unit of goods, services, or both specified in the invoice.
    - `quantity_ordered`: The number of a specified invoice part ordered by the buyer from the supplier.
      - `location`: The location of the quantity ordered. as defined by its `begin` and `end` indexes.
      - `text`: The quantity ordered.
      - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
    - `part_description`: The description of the specified invoice part.
      - `location`: The location of the part description as defined by its `begin` and `end` indexes.
      - `text`: The part description.
      - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
    - `unit_price`: The price per unit of the ordered good or service.
      - `amount`: A unit price.
        - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
        - `text`: The unit price.
        - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.
      - `currency`: The currency used by the unit price.
        - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
        - `text`: The name of the currency.
        - `provenance_ids`: An array of one or more hashed values that you can send to IBM to provide feedback or receive support.

Sample output resembles the following:

```json
{
  "model_id": "invoices",
  "model_version": "",
  "document": {
    "title": "the extracted title",
    "html": "the html of the document...",
    "hash": "hash value"
  },
  "buyers": [ 
    {
      "location": {
        "begin": 6701,
        "end": 6723
    },
    "text": "IBM Middle East-FZ LLC",
    "provenance_ids": ["I755","I112"]  
    } 
  ],
  "suppliers": [ 
    {
      "location": {
        "begin": 36695,
        "end": 36717
    },
    "text": "Ricoh International BV",
    "provenance_ids": ["I234","I123"] 
    } 
  ],
  "totals_due": [ 
    {
      "amount": {
        "location" : {
          "begin": 35400,
          "end": 35410
        },
        "text": "145,641.91",
        "provenance_ids": ["I212"]
      },
      "currency": {
        "location": {
          "begin": 33827,
          "end": 33830
        },
        "text": "EUR",
        "provenance_ids": ["I3143"]
      }
    } 
  ],
  "currencies": [
    {
      "location": {
        "begin": 33827,
        "end": 33830
      },
      "text": "EUR",
      "provenance_ids": ["I3143"]
    }, 
    {
      "location": {
        "begin": 34605,
        "end": 34608
      },
      "text": "EUR",
      "provenance_ids": ["I7678", "I2832"]
    }
  ],
  "purchase_order_numbers" : [ 
    {
      "location": {
        "begin": 11501,
        "end": 11511
      },
      "text": "4603344312",
      "provenance_ids": ["I7795"]
    }
  ],
  "invoice_numbers": [ 
    {
      "location": {
        "begin": 2982,
        "end": 2990
      },
      "text": "52156277",
      "provenance_ids": ["I3143", "I676", "I1142"]
    }
  ],
  "invoice_dates": [ 
    {
      "location": {
        "begin": 3237,
        "end": 3246
      },
      "text": "28-FEB-18",
      "provenance_ids": ["I222", "I354"]
    }
  ],
  "due_dates": [ 
    {
      "location": {
        "begin": 36424,
        "end": 36433
      },
      "text": "29-APR-18",
      "provenance_ids": ["I420", "I6060"]
    } 
  ],
  "invoice_parts": [ 
    {
      "quantity_ordered": {
        "location": {
          "begin": 14441,
          "end": 14442
        },
       "text": "1",
       "provenance_ids": ["I890", "I1100", "I456"]
      },
      "part_description": {
        "location": {
          "begin": 13873,
          "end": 13889
        },
        "text": "On-Site Services",
        "provenance_ids": ["I242", "I1181"]
      },
      "unit_price": {
        "amount": {
          "location": {
            "begin": 14926,
            "end": 14935
          },
          "text": "38,720.60",
          "provenance_ids": ["I3122", "I4098", "I909"]
        },
        "currency": {
          "location": {
            "begin": 33827,
            "end": 33830
          },
          "text": "EUR",
          "provenance_ids": ["I1122", "I567"]
        }
      }
    }
  ]
}
```

---

copyright:
  years: 2019
lastupdated: "2019-02-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:important: .important}

# Understanding mortgage parsing
{: #mortgages}

You can analyze mortgage closing disclosure documents by calling the `POST /v1/mortgages` method.
{: shortdesc}

Mortgage parsing is a beta feature. For information about beta features, see [Beta features](/docs/services/compare-comply/relnotes.html#beta_features) in the [Release notes](/docs/services/compare-comply/relnotes.html#release_notes). You must request access to mortgage parsing by completing the following [form](http://ibm.biz/mortgage).
{: important}

You can classify the contents of mortgage closure disclosure documents in your [input document](/docs/services/compare-comply/formats.html#formats) by using the `POST /v1/mortgages` method.

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/mortgages` method to classify a mortgage. The method takes the following input parameters:
  - `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
  - `file` (**required** `file`): The input file that is to be classified for mortgage information.

Replace `{apikey}` with the API key you copied earlier and `{input_file}` with the path to the input file to parse.

```bash
curl -X POST -u "apikey:{apikey}" -F 'file=@{input_file}' https://gateway.watsonplatform.net/compare-comply/api/v1/mortgages?version=2018-10-15
```
{: codeblock}

The command output uses the following schema.

```json
{
    "model_id": "mortgages",
    "model_version": string,
    "document": {
        "title": string,
        "html": string,
        "hash": string
    },
    "closing_information": {
        "date_issued": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "closing_date": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "disbursement_date": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "settlement_agent": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "file_id": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "property": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "sale_price": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ]
    },
    "transaction_information": {
        "borrower": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "seller": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "lender": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ]
    },
    "loan_information": {
        "loan_term": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "loan_purpose": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "loan_product": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "loan_id": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "loan_mic": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ]
    },
    "loan_terms": {
        "loan_amount": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "interest_rate": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "balloon_payment": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ]
    },
    "projected_payments": {
        "estimated_total_monthly_payment": {
            "years": [
                {
                    "year": {
                        "text": string,
                        "location": {
                            "begin": int,
                            "end": int
                        }
                    },
                    "amount": {
                        "text": string,
                        "location": {
                            "begin": int,
                            "end": int
                        }
                    }
                }
            ]
        }
    },
    "other_costs": {
        "prepaids": {
            "homeowners_insurance": [
                {
                    "text": string,
                    "location": {
                        "begin": int,
                        "end": int
                    }
                }
            ],
            "mortgage_insurance": [
                {
                    "text": string,
                    "location": {
                        "begin": int,
                        "end": int
                    }
                }
            ]
        },
        "total_closing_costs": {
            "closing_costs_subtotals": {
                "borrower_paid": {
                    "at_closing": [
                        {
                            "text": string,
                            "location": {
                                "begin": int,
                                "end": int
                            }
                        }
                    ],
                    "before_closing": [
                        {
                            "text": string,
                            "location": {
                                "begin": int,
                                "end": int
                            }
                        }
                    ]
                },
                "seller_paid": {
                    "at_closing": [
                        {
                            "text": string,
                            "location": {
                                "begin": int,
                                "end": int
                            }
                        }
                    ],
                    "before_closing": [
                        {
                            "text": string,
                            "location": {
                                "begin": int,
                                "end": int
                            }
                        }
                    ]
                },
                "paid_by_others": [
                    {
                        "text": string,
                        "location": {
                            "begin": int,
                            "end": int
                        }
                    }
                ]
            }
        }
    },
    "cash_to_close": {
        "cash_to_close_final": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "cash_to_close_loan_estimate": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ]
    },
    "transaction_summary": {
        "borrower_transactions": {
            "due_from_borrower_at_closing": [
                {
                    "text": string,
                    "location": {
                        "begin": int,
                        "end": int
                    }
                }
            ]
        },
        "seller_transactions": {
            "due_to_seller_at_closing": [
                {
                    "text": string,
                    "location": {
                        "begin": int,
                        "end": int
                    }
                }
            ]
        }
    },
    "loan_calculations": {
        "total_payments": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "finance_charge": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "annual_percentage_rate": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "total_interest_percentage": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ],
        "amount_financed": [
            {
                "text": string,
                "location": {
                    "begin": int,
                    "end": int
                }
            }
        ]
    }
}
```

The schema is arranged as follows.

  - `model_id`: The analysis model used by the service. For the `/v1/mortgages` method, the value is `mortgage_closing_disclosures`.
  - `model_version`: The version of the analysis model specified by the value of the `model_id` parameter.
  - `document`: An object that lists basic information about the document, including:
    - `title`: The document title, if detected.
    - `html`: The full text of the input document in HTML format.
    - `hash`: The MD5 hash of the input document.
  - `closing_information`: An object that contains information related to the closing transaction, relevant dates, and the collateral associated with the loan.
    - `date_issued`: An array that lists the date on which the closing disclosure was provided to the seller and borrower.
      - `text`: The date on which the closing disclosure was issued, in the format `MM/DD/YYYY`.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `closing_date`: An array that lists the date on which the borrower becomes contractually obligated to pay the mortgage debt.
      - `text`: The date in the format `MM/DD/YYYY`.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `disbursement_date`: An array that lists the date on which the amounts due are to be paid to the borrower or borrowers, to the property seller or sellers, or both.
      - `text`: The disbursement date in the format `MM/DD/YYYY`.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `settlement_agent`: An array that lists the name of the entity that employs the settlement agent who carries out the closing.
      - `text`: The settlement agent.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `file_id`: An array that lists the identification number assigned to the transaction by the closing agent.
      - `text`: The transaction identification number.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `property`: An array that lists the street address or legal description of the subject property.
      - `text`: The street address or legal description of the subject property.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `sale_price`: An array that lists the property's sale price as recorded on the sales contract.
      - `text`: The sale price.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
  - `transaction_information`: An object that identifies the parties involved in the mortgage, including the following:
    - `borrower`: An array of borrower references that includes the name and address of the borrower or borrowers.
      - `text`: The name and address of the borrower or borrowers.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `seller`: An array of seller references that includes the name and address of the seller or sellers.
      - `text`: The name and address of the seller or sellers.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `lender`: An array of lender references that includes the name and address of the lender. The lender is the party who provides the funding; the lender is also known as the creditor. 
      - `text`: The name and address of the lender.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
  - `loan_information`: An object that provides identifying information about the loan, including the following:
    - `loan_term`: An array that lists the length of the loan term, which can be fixed, extendable, or periodic (for example, a construction loan)
      - `text`: The loan term expressed in months or years.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `loan_purpose`: An array that describes the borrower's or borrowers' intended use of the loan.
      - `text`: The intended use of the loan. Possible values include `Purchase`, `Refinance`, `Construction`, and others.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `loan_product`: An array that lists the loan product type. Possible values include `Fixed Rate`, `Adjustable`, and `Step Rate`.
      - `text`: The loan product type.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `loan_id`: An array that lists the lender's loan identifier number (Loan ID#).
      - `text`: The loan's identifier number.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `loan_mic`: An array that lists the assigned Mortgage Insurance Certificate Number (MIC #).
      - `text`: The assigned MIC number.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
  - `loan_terms`: An object that details the legal obligation entered into by the borrower or borrowers based on information that is known or that should reasonably be known by the lender.
    - `loan_amount`: An array that lists the amount of credit given to the borrower or borrowers by the lender.
      - `text`: The loan amount.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `interest_rate`: An array that defines the interest rate that the lender charges for lending the money to the borrower or borrowers, expressed as a percentage value.
      - `text`: The interest rate.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `balloon_payment`: A string that specifies whether or not the loan has a balloon payment feature, which is denoted by a `YES` or `NO` in the mortgage. If `YES`, a description of the balloon payment is required. A balloon payment is a one-time payment that is larger than the usual payment and that is due at the end of the loan term. The payment is generally two times the loan's average monthly payment. If the mortgage has a balloon payment, the payments can be lower in the years before the balloon payment is due; however, this typically means that a bigger amount is owed at the end of the loan.
      - `text`: A string that specifies whether or not the loan has a balloon payment feature, which is denoted by a `YES` or `NO` in the mortgage. If `YES`, a description of the balloon payment is required.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
  - `projected_payments`: An object that defines the periodic mortgage payment due between `year n` and `year m` of the mortgage life. It must contain no more than four payment ranges.
    - `estimated_total_monthly_payment`: An object that specifies the estimated total monthly payment calculated by summing the amounts of Principal and Interest, Mortgage Insurance, and Estimated Escrow for each column.
      - `years`: An array that lists the number of years in a periodic mortgage payment.
        - `year`: An object that lists a particular year in a periodic mortgage payment.
          - `text`: The year.
          - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
        - `amount`: An object that lists the amount due in a periodic mortgage payment.
          - `text`: The amount due.
          - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
  - `other_costs`: An object that categorizes all fees or payments that are not disclosed in the Loan Costs table. The only category of costs shown is `prepaids`.
    - `prepaids`: An object that defines items that are to be paid in advance of the first scheduled payment of the loan.
      - `homeowners_insurance`: An array that specifies any insurance against loss or damage, or against liability arising from the property.
        - `text`: The homeowners' insurance premium paid by the borrower at closing, before the first scheduled mortgage payment.
        - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
      - `mortgage_insurance`: An array that specifies mortgage insurance. Mortgage insurance lowers the risk to the lender of making a loan to the borrower, so the borrower can qualify for a loan that they might not otherwise be able to obtain. However, it increases the cost of the loan. If the borrower is required to pay mortgage insurance, it is included in the total monthly payment that the borrower pays to the lender, in the closing costs, or both.
        - `text`: The cost of mortgage insurance within a range of years.
        - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `total_closing_costs`: An object that specifies the total of the up-front costs associated with the loan, including the down payment. This differs from `cash_to_close`.
      - `closing_costs_subtotals`: An object that lists subtotals for closing costs.
        - `borrower_paid`: An object that lists the costs that are charged to the borrower.
          - `at_closing`: An array that lists the total amount the borrower pays before closing and at closing.
            - `text`: The total payments paid by the borrower before closing and at closing.
            - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
          - `before_closing`: An array that lists borrower payments before closing.
            - `text`: The total payments made by the borrower before closing.
            - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
        - `seller_paid`: An object that lists the costs that are charged to the seller.
          - `at_closing`: An array that lists the subtotals of all amounts paid by the seller before closing and at closing.
            - `text`: A subtotal paid by the seller before closing and at closing.
            - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
          - `before_closing`: An array that lists the amounts the seller pays before closing.
            - `text`: A subtotal paid by the seller before closing.
            - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
          - `at_closing`: An array that lists the costs charged to others at closing.
            - `text`: A cost charged to others at closing.
            - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
          - `before_closing`: An array that lists the the costs charged to others before closing.
            - `text`: A cost charged to others. before closing.
            - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
        - `paid_by_others`: An array that lists any costs that are payable by others.
          - `text`: A cost that is payable by others.
          - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
  - `cash_to_close`: An object that lists the total amount the borrower or borrowers need to pay at closing in addition to any cash they have already paid.
    - `cash_to_close_final`: An array that lists the final amount the borrower or borrowers need to pay at closing.
      - `text`: The final amount of the cash that is required to close.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `cash_to_close_loan_estimate`: An array that provides an estimate of the amount the borrower or borrowers need to pay at closing.
      - `text`: The estimated amount of the cash needed to close.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
  - `transaction_summary`: An object that provides an overview of the borrower's and seller's transaction details.
    - `borrower_transactions`: An object that lists all of the borrower's transactions.
      - `due_from_borrower_at_closing`: An array that lists the amount due from the borrower or borrowers at closing.
        - `text`: The amount due from the borrower or borrowers at closing.
        - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
      - `seller_transactions`: An object that lists all of the seller's transactions.
        - `due_to_seller_at_closing`: An array that lists the amount due to the seller at closing.
          - `text`: The amount due to the seller at closing.
          - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
  - `loan_calculations`: An object that provides the results of the five calculations related to the borrower's cost of financing.
    - `total_payments`: An array that lists the total the borrower will have paid after making all of the payments of principal and interest, mortgage insurance, and loan costs as scheduled.
      - `text`: The amount of the total payments.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `finance_charge`: An array that lists the cost of the mortgage loan to the borrower.
      - `text`: The amount of the finance charge.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `annual_percentage_rate`: An array that lists the cost of the loan to the borrower, expressed as a rate. This is not the interest rate.
      - `text`: The annual percentage rate (APR).
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `total_interest_percentage`: An array that lists the total amount of interest that the borrower will pay over the loan term, expressed as a percentage of the loan amount.
      - `text`: The total amount of interest that the borrower will pay over the loan term, expressed as a percentage of the loan amount.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
    - `amount_financed`: An array that lists the loan amount available after the borrower pays the up-front finance charge.
      - `text`: The amount that is financed.
      - `location`: An object that identifies the location of the element. The object contains two index numbers, `begin` and `end`. The index numbers indicate the beginning and ending positions, respectively, of the element as character numbers in the HTML document that the service created from your input document.
      

Sample output resembles the following:

```json
{
  "model_id" : "mortgage_closing_disclosures",
  "model_version" : "0.1.1",
  "document" : {
    "title" : "no title",
    "html" : "<?xml version='1.0' encoding='UTF-8' standalone='yes'?><html>\n<head>\n ... </html>",
    "hash" : "20a6b4e7b81090d58d1772d66c3ccd18"
  },
  "closing_information" : {
    "date_issued" : [ {
      "location" : {
        "begin" : 4752,
        "end" : 4761
      },
      "text" : "4/15/2013"
    } ],
    "closing_date" : [ {
      "location" : {
        "begin" : 6200,
        "end" : 6209
      },
      "text" : "4/15/2013"
    } ],
    "disbursement_date" : [ {
      "location" : {
        "begin" : 7469,
        "end" : 7478
      },
      "text" : "4/15/2013"
    } ],
    "settlement_agent" : [ {
      "location" : {
        "begin" : 8742,
        "end" : 8759
      },
      "text" : "Epsilon Title Co."
    } ],
    "file_id" : [ {
      "location" : {
        "begin" : 9838,
        "end" : 9845
      },
      "text" : "12-3456"
    } ],
    "property" : [ {
      "location" : {
        "begin" : 11562,
        "end" : 11579
      },
      "text" : "456 Somewhere Ave"
    }, {
      "location" : {
        "begin" : 12944,
        "end" : 12961
      },
      "text" : "Anytown, ST 12345"
    } ],
    "sale_price" : [ {
      "location" : {
        "begin" : 14408,
        "end" : 14416
      },
      "text" : "$180,000"
    } ]
  },
  "transaction_information" : {
    "borrower" : [ {
      "location" : {
        "begin" : 5216,
        "end" : 5244
      },
      "text" : "Michael Jones and Mary Stone"
    }, {
      "location" : {
        "begin" : 6495,
        "end" : 6514
      },
      "text" : "123 Anywhere Street"
    }, {
      "location" : {
        "begin" : 7767,
        "end" : 7784
      },
      "text" : "Anytown, ST 12345"
    } ],
    "seller" : [ {
      "location" : {
        "begin" : 9216,
        "end" : 9238
      },
      "text" : "Steve Cole and Amy Doe"
    }, {
      "location" : {
        "begin" : 10134,
        "end" : 10153
      },
      "text" : "321 Somewhere Drive"
    }, {
      "location" : {
        "begin" : 11868,
        "end" : 11885
      },
      "text" : "Anytown, ST 12345"
    } ],
    "lender" : [ {
      "location" : {
        "begin" : 13431,
        "end" : 13441
      },
      "text" : "Ficus Bank"
    } ]
  },
  "loan_information" : {
    "loan_term" : [ {
      "location" : {
        "begin" : 5705,
        "end" : 5713
      },
      "text" : "30 years"
    } ],
    "loan_purpose" : [ {
      "location" : {
        "begin" : 6974,
        "end" : 6982
      },
      "text" : "Purchase"
    } ],
    "loan_product" : [ {
      "location" : {
        "begin" : 8242,
        "end" : 8252
      },
      "text" : "Fixed Rate"
    } ],
    "loan_id" : [ {
      "location" : {
        "begin" : 13915,
        "end" : 13924
      },
      "text" : "123456789"
    } ],
    "loan_mic" : [ {
      "location" : {
        "begin" : 14998,
        "end" : 15007
      },
      "text" : "000654321"
    } ]
  },
  "loan_terms" : {
    "loan_amount" : [ {
      "location" : {
        "begin" : 16354,
        "end" : 16362
      },
      "text" : "$162,000"
    } ],
    "interest_rate" : [ {
      "location" : {
        "begin" : 17050,
        "end" : 17056
      },
      "text" : "3.875%"
    } ],
    "balloon_payment" : [ {
      "location" : {
        "begin" : 20335,
        "end" : 20337
      },
      "text" : "NO"
    } ]
  },
  "projected_payments" : {
    "estimated_total_monthly_payment" : {
      "years" : [ {
        "year" : {
          "text" : "Years 1-7 ",
          "location" : {
            "begin" : 21769,
            "end" : 21779
          }
        },
        "amount" : {
          "text" : "$1,050.26 ",
          "location" : {
            "begin" : 26559,
            "end" : 26569
          }
        }
      }, {
        "year" : {
          "text" : "Years 8-30\n",
          "location" : {
            "begin" : 22074,
            "end" : 22085
          }
        },
        "amount" : {
          "text" : "$967.91\n",
          "location" : {
            "begin" : 26852,
            "end" : 26860
          }
        }
      } ]
    }
  },
  "other_costs" : {
    "prepaids" : {
      "mortgage_insurance" : [ ],
      "homeowners_insurance" : [ ]
    },
    "total_closing_costs" : {
      "closing_costs_subtotals" : {
        "borrower_paid" : {
          "at_closing" : [ {
            "location" : {
              "begin" : 90532,
              "end" : 90541
            },
            "text" : "$9,682.30"
          } ],
          "before_closing" : [ {
            "location" : {
              "begin" : 90768,
              "end" : 90774
            },
            "text" : "$29.80"
          } ]
        },
        "seller_paid" : {
          "at_closing" : [ {
            "location" : {
              "begin" : 91000,
              "end" : 91010
            },
            "text" : "$12,800.00"
          } ],
          "before_closing" : [ {
            "location" : {
              "begin" : 91236,
              "end" : 91243
            },
            "text" : "$750.00"
          } ]
        },
        "paid_by_others" : [ {
          "location" : {
            "begin" : 91466,
            "end" : 91473
          },
          "text" : "$405.00"
        } ]
      }
    }
  },
  "cash_to_close" : {
    "cash_to_close_final" : [ {
      "location" : {
        "begin" : 104924,
        "end" : 104934
      },
      "text" : "$14,147.26"
    } ],
    "cash_to_close_loan_estimate" : [ {
      "location" : {
        "begin" : 104682,
        "end" : 104692
      },
      "text" : "$16,054.00"
    } ]
  },
  "transaction_summary" : {
    "borrower_transactions" : {
      "due_from_borrower_at_closing" : [ {
        "location" : {
          "begin" : 106383,
          "end" : 106394
        },
        "text" : "$189,762.30"
      } ]
    },
    "seller_transactions" : {
      "due_to_seller_at_closing" : [ {
        "location" : {
          "begin" : 114203,
          "end" : 114214
        },
        "text" : "$180,080.00"
      } ]
    }
  },
  "loan_calculations" : {
    "total_payments" : [ {
      "location" : {
        "begin" : 146610,
        "end" : 146621
      },
      "text" : "$285,803.36"
    } ],
    "finance_charge" : [ {
      "location" : {
        "begin" : 148000,
        "end" : 148011
      },
      "text" : "$118,830.27"
    } ],
    "annual_percentage_rate" : [ {
      "location" : {
        "begin" : 151077,
        "end" : 151083
      },
      "text" : "4.174%"
    } ],
    "total_interest_percentage" : [ {
      "location" : {
        "begin" : 152795,
        "end" : 152801
      },
      "text" : "69.46%"
    } ],
    "amount_financed" : [ {
      "location" : {
        "begin" : 149208,
        "end" : 149219
      },
      "text" : "$162,000.00"
    } ]
  }
}
```

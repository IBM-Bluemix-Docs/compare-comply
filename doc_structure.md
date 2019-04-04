---

copyright:
years: 2018, 2019
lastupdated: "2019-03-29"

subcollection: compare-comply

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# Understanding document structure
{: #doc_struct}

The output of the `POST /v1/element_classification` method includes a `document_structure` object that details the structural composition of the input document. See [Classifying elements](/docs/services/compare-comply?topic=compare-comply-output_schema) for the placement of the `document_structure` information in the output of the method.

## Document structure output
{: #struct_output}

Document structure information is represented in the output as follows. The object is located immediately after the top-level `tables` array.

```json
  "document_structure": {
    "section_titles": [
      {
        "text": string,
        "location": {
          "begin": int,
          "end": int
        },
        "level": int
        "element_locations": [
          {
            "begin": int,
            "end": int
          },
          ...
        ]
      },
      ...
    ],
    "leading_sentences": [
      {
        "text": string,
        "location": {
          "begin": int,
          "end": int
        },
        "element_locations": [
          {
            "begin": int,
            "end": int
          },
          ...
        ]
      },
      ...
    ],
    "paragraphs": [
       {
         "location": {
           "begin": int,
           "end": int
         }
       },
       ...
    ]
  }
```

## Document structure elements
{: #struct_elements}

The elements of the `document_structure` object contain the following information:

  - `document_structure`: An object describing the structure of the input document.
    - `section_titles`: An array containing one object per section or subsection detected in the input document. Sections and subsections are not nested; instead, they are flattened out and can be placed back in order by using the `begin` and `end` values of the element and the `level` value of the section.
      - `text`: A string listing the section title, if detected.
      - `location`: The location of the title in the input document as defined by its `begin` and `end` indexes.
      - `level`: An integer indicating the level at which the section is located in the input document. For example, `1` represents a top-level section, `2` represents a subsection within the level `1` section, and so forth.
      - `element_locations`: An array containing objects that specify the `begin` and `end` values of the sentences in the section.
    - `leading_sentences`: An array that contains one object per leading sentence of a list or subsection, in parallel with the `section_titles` and `paragraph` arrays. The object details the leading sentences in the matching section or subsection. As in the `section_titles` array, the objects are not nested; instead, they are flattened out and can be placed back in order by using the `begin` and `end` values of the element or any level markers in the input document.
      - `text`: A string listing the leading sentence, if detected.
      - `location`: The location of the leading sentence in the input document as defined by its `begin` and `end` indexes.
      - `element_locations`: An array containing objects that specify the `begin` and `end` values of the leading sentences in the section.
    - `paragraphs`: An array containing one object per paragraph, in parallel with the `section_titles` and `leading_sentences` arrays. Each object lists the span (beginning and end location) of the corresponding paragraph.
      - `location`: The location of the paragraph in the input document as defined by its `begin` and `end` indexes.
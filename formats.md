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
{:note: .note}
{:important: .important}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Supported input formats
{: #formats}

IBM Watson&reg; Compare and Comply supports a variety of input formats, including PDF, JSON, Microsoft Word, images, and text.
{: shortdesc}

## General support
{: #general}

Observe the following notes regarding files submitted to Compare and Comply.

  - Not all input formats are accepted by all methods. For more information, see [Support by method](#methods) and the lists of supported methods in the details of each input type described in the following sections.
  - For optimal results, specify the MIME type when submitting a file. For example, if you submit a PDF file, it is recommended that you specify the file as follows:
     ```
     curl -u "apikey:{apikey}" -F "file=@myFile.pdf;type=application/pdf" https://gateway.watsonplatform.net/compare-comply/api/v1/{method_name}?version=2018-10-15
     ```
     {: pre}
    MIME types are provided with the supported file types listed in the following sections.
  - Files can be up to 1.5 MB in size when submitted to the service with individual methods. If you submit files through the [`/v1/batches` interface](/docs/services/compare-comply?topic=compare-comply-batching), files can be up to 50 MB in size.
  - Documents with non-standard page layouts (such as 2 or 3 columns per page) do not parse correctly.
  - The [Compare and Comply Tooling](/docs/services/compare-comply?topic=compare-comply-using_tool) accepts all file types other than text.
  
## PDF support
{: #pdfs}

Compare and Comply can process PDF files. Note the following.

  - The MIME type is `application/pdf`.
  - Both programmatic and scanned PDF files are supported. Files that have been scanned and processed by an optical character reader (OCR) are also supported.
  - Secure PDFs, which require a password to open, and restricted PDFs, which require a password to edit, cannot be processed.
  
Compare and Comply methods that support PDF include the following.

  - `/v1/html_conversion`
  - `/v1/element_classification`
  - `/v1/tables`
  - `/v1/invoices`
  - `/v1/comparison`

## Microsoft Word support
{: #word}

Compare and Comply can process Microsoft Word files in the following formats.
  - DOC (`application/msword`)
  - DOCX (`application/vnd.openxmlformats-officedocument.wordprocessingml.document`)
  
Compare and Comply methods that support Microsoft Word files include the following.

  - `/v1/html_conversion`
  - `/v1/element_classification`
  - `/v1/tables`
  - `/v1/invoices`
  - `/v1/comparison`

## Image support
{: #images}

Compare and Comply can process image files in the following formats.

  - BMP (`image/bmp`)
  - GIF (`image/gif`)
  - JPEG (`image/jpeg`)
  - JPEG2000 (`image/jpeg`)
  - PNG (`image/png`)
  - RAW (no specific MIME type)
  - TIFF (`image/tiff`)

For best results, use image files with a resolution of 300 DPI or higher. Using image files with a resolution under 300 DPI can result in non-optimal output.
{: note}

Compare and Comply methods that support images include the following.

  - `/v1/html_conversion`
  - `/v1/element_classification`
  - `/v1/tables`
  - `/v1/invoices`
  - `/v1/comparison`
  
## Text support
{: #text}

The service can process "plain" text (ASCII) files that use a monospaced font and page breaks. Richer text formats that include non-monospaced fonts and style attributes such as bold and italics are not yet supported. If you need to process an enriched text file, convert it to PDF before submitting it to the service.

The MIME type is `text/plain`.

Compare and Comply methods that support text include the following.

  - `/v1/html_conversion`
  - `/v1/tables`

## Support by method
{: #methods}

The service's methods can accept different types of files as specified in the following table.

| Method           |PDF support   |Word support     |Image support        |Text support    |
|------------------|-----------------|-----------------------------------------|
|Tooling*           | Supported    | Supported | All supported image formats | **Not** supported |
|`/v1/html_conversion`| Supported | Supported | All supported image formats | Supported |
|`/v1/element_classification`| Supported | Supported | All supported image formats | **Not** supported |
|`/v1/tables`      | Supported | Supported | All supported image formats | Supported |
|`/v1/invoices`    | Supported | Supported | All supported image formats | **Not** supported |
|`/v1/comparison`* | Supported | Supported | All supported image formats | **Not** supported |

\* The Tooling and `/v1/comparison` method also accept JSON files from the output of the `/v1/element_classification` method.
{: note}

The `/v1/feedback` methods do not accept image or text files. 

The `/v1/batches` methods accept images and text files according to the method called in the batch job. For example, if your batch job calls the `/v1/html_conversion` method, it accepts both images and text files. Similarly, if your batch job calls the `/v1/element_classification` method, it accepts images but not text files.

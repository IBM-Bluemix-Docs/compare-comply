---

copyright:
  years: 2018, 2021
lastupdated: "2021-01-25"

keywords: convert,HTML convert,conversion,HTML conversion,document conversion

subcollection: compare-comply

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:preview: .preview}
{:beta: .beta}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Converting an input document into HTML
{: #html_conversion}

{{site.data.keyword.cncfull}} is discontinued. Existing instances are supported until 30 November 2021, but as of 1 December 2020, you can't create instances. Any instance that exists on 30 November 2021 will be deleted. Consider migrating to {{site.data.keyword.discoveryshort}} Premium on {{site.data.keyword.cloud_notm}} for your {{site.data.keyword.cncshort}} use cases. For more information, see the [announcement](/status?query=Compare+and+Comply&selected=announcement){: external}.
{: deprecated}

You can transform an [input document](/docs/compare-comply?topic=compare-comply-formats) into HTML by using the **HTML conversion** feature. This example uses the `POST /v1/html_conversion` method.
{: shortdesc}

The **HTML conversion** feature is available only on `Standard` and `Premium` plans. It is not available on the free `Lite` plan. See [https://cloud.ibm.com/account/settings](https://cloud.ibm.com/account/settings){: external} for information about your plan.
{: important}

In a `bash` shell or equivalent environment such as Cygwin, use the `POST /v1/html_conversion` method to convert an input document into HTML. The method takes the following input parameters:

- `version` (**required** `string`): A date in the format `YYYY-MM-DD` that identifies the specific version of the API to use when processing the request.
- `file` (**required** `file`): The input file that is to be converted into HTML.
- `model` (optional `string`): If this parameter is specified, the service runs the specified type of element classification. Currently, the only supported value is `contracts`.

You can specify the response content type to return the converted HTML in either JSON (the default) or raw HTML. See the examples following the command example for the different formats.

- To return JSON explicitly, specify the header `-H "Accept: application/json"`. This is the default.
- To return raw HTML, specify the header `-H "Accept: text/html"`.

Replace `{apikey}` with the API key you copied earlier, `{url}` with the URL you copied earlier, and `{input_file}` with the path to the input file to convert.

```bash
curl -X POST -u "apikey:{apikey}" -H "Accept: application/json" \
-F "file=@{input_file}" "{url}/v1/html_conversion?version=2018-10-15"
```
{: pre}

In this example, the header `-H "Accept: application/json"` returns output in the following format:

```json
{
  "num_pages": "18",
  "author": "SOME_USER",
  "publication_date": "2017-04-04",
  "title": "no title",
  "html": "<?xml version='1.0' encoding='UTF-8' standalone='yes'?><html>...</html>"
}
```
{: screen}

If you specify `-H "Accept: text/html"`, the service returns only the contents of the `"html"` field from the previous output example:

```html
<?xml version='1.0' encoding='UTF-8' standalone='yes'?><html>...</html>
```
{: screen}

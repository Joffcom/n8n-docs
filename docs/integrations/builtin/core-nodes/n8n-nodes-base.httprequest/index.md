---
#https://www.notion.so/n8n/Frontmatter-432c2b8dff1f43d4b1c8d20075510fe4
title: HTTP Request node documentation
description: Learn how to use the HTTP Request node in n8n. Follow technical documentation to integrate HTTP Request node into your workflows.
contentType: integration
priority: critical
---

# HTTP Request node

The HTTP Request node is one of the most versatile nodes in n8n. It allows you to make HTTP requests to query data from any app or service with a REST API.

When using this node, you're creating a REST API call. You need some understanding of basic API terminology and concepts.

There are two ways to create an HTTP request: configure the [node parameters](#node-parameters) or [import a curl command](#import-curl-command).

/// note | Credentials
Refer to [HTTP Request credentials](/integrations/builtin/credentials/httprequest/) for guidance on setting up authentication. 
///

## Node parameters

### Method

Select the method to use for the request:

- DELETE
- GET
- HEAD
- OPTIONS
- PATCH
- POST
- PUT

### URL

Enter the endpoint you want to use.

### Authentication

n8n recommends using the **Predefined Credential Type** option when it's available. It offers an easier way to set up and manage credentials, compared to configuring generic credentials.

#### Predefined credentials

Credentials for integrations supported by n8n, including both built-in and community nodes. Use **Predefined Credential Type** for custom operations without extra setup. Refer to [Custom API operations](/integrations/custom-operations/) for more information.

#### Generic credentials

Credentials for integrations not supported by n8n. You'll need to manually configure the authentication process, including specifying the required API endpoints, necessary parameters, and the authentication method. 

You can select one of the following methods:

* Basic auth
* Custom auth
* Digest auth
* Header auth
* OAuth1 API
* OAuth2 API
* Query auth

Refer to [HTTP request credentials](/integrations/builtin/credentials/httprequest/) for more information on setting up each credential type.

### Send Query Parameters

Query parameters act as filters on HTTP requests. If the API you're interacting with supports them and the request you're making needs a filter, turn this option on.

**Specify your query parameters** using one of the available options:

* **Using Fields Below**: Enter **Name**/**Value** pairs of **Query Parameters**. To enter more query parameter name/value pairs, select **Add Parameter**. The name is the name of the field you're filtering on, and the value is the filter value.
* **Using JSON**: Enter **JSON** to define your query parameters.

Refer to your service's API documentation for detailed guidance.

### Send Headers

Use this parameter to send headers with your request. Headers contain metadata or context about your request.

**Specify Headers** using one of the available options:

* **Using Fields Below**: Enter **Name**/**Value** pairs of **Header Parameters**. To enter more header parameter name/value pairs, select **Add Parameter**. The name is the header you wish to set, and the value is the value you want to pass for that header.
* **Using JSON**: Enter **JSON** to define your header parameters.

Refer to your service's API documentation for detailed guidance.

### Send Body

If you need to send a body with your API request, turn this option on.

Then select the **Body Content Type** that best matches the format for the body content you wish to send.

#### Form URLencoded

Use this option to send your body as `application/x-www-form-urlencoded`.

**Specify Body** using one of the available options:

* **Using Fields Below**: Enter **Name**/**Value** pairs of **Body Parameters**. To enter more body parameter name/value pairs, select **Add Parameter**. The name should be the form field name, and the value is what you wish to set that field to.
* **Using Single Field**: Enter your name/value pairs in a single **Body** parameter with format `fieldname1=value1&fieldname2=value2`.

Refer to your service's API documentation for detailed guidance.

#### Form-Data

Use this option to send your body as `multipart/form-data`.

Configure your **Body Parameters** by selecting the **Parameter Type**:

* Choose **Form Data** to enter **Name**/**Value** pairs.
* Choose **n8n Binary File** to pull the body from a file the node has access to.
    * **Name**: Enter the ID of the field to set.
    * **Input Data Field Name**: Enter the name of the incoming field containing the binary file data you want to process.

Select **Add Parameter** to enter more parameters.

Refer to your service's API documentation for detailed guidance.

#### JSON

Use this option to send your body as JSON.

**Specify Body** using one of the available options:

* **Using Fields Below**: Enter **Name**/**Value** pairs of **Body Parameters**. To enter more body parameter name/value pairs, select **Add Parameter**.
* **Using JSON**: Enter **JSON** to define your body.

Refer to your service's API documentation for detailed guidance.

#### n8n Binary File

Use this option to send the contents of a file stored in n8n as the body.

Enter the name of the incoming field that contains the file as the **Input Data Field Name**.

Refer to your service's API documentation for detailed guidance on how to format the file.

#### Raw

Use this option to send raw data in the body.

* **Content Type**: Enter the `Content-Type` header to use for the raw body content. Refer to the IANA [Media types](https://www.iana.org/assignments/media-types/media-types.xhtml){:target=_blank .external-link} documentation for a full list of MIME content types.
* **Body**: Enter the raw body content to send.

Refer to your service's API documentation for detailed guidance.

## Node options

Select **Add Option** to view and select these options. Options are available to all parameters unless otherwise noted.

### Array Format in Query Parameters

/// note | Option availability
This option is only available when you turn on **Send Query Parameters**.
///

Use this option to control the format for arrays included in query parameters. Choose from these options:

* **No Brackets**: Arrays will format as the name=value for each item in the array, for example: `foo=bar&foo=qux`.
* **Brackets Only**: The node adds square brackets after each array name, for example: `foo[]=bar&foo[]=qux`.
* **Brackets with Indices**: The node adds square brackets with an index value after each array name, for example: `foo[0]=bar&foo[1]=qux`.

Refer to your service's API documentation for guidance on which option to use.

### Batching

Control how to batch large numbers of input items:

* **Items per Batch**: Enter the number of input items to include in each batch.
* **Batch Interval**: Enter the time to wait between each batch of requests in milliseconds. Enter 0 for no batch interval.

### Ignore SSL Issues

By default, n8n only downloads the response if SSL certificate validation succeeds. If you'd like to download the response even if SSL certificate validation fails, turn this option on.

### Lowercase Headers

Choose whether to lowercase header names (turned on, default) or not (turned off).

### Redirects

Choose whether to follow redirects (turned on by default) or not (turned off). If turned on, enter the maximum number of redirects the request should follow in **Max Redirects**.

### Response

Use this option to set some details about the expected API response, including:

* **Include Response Headers and Status**: By default, the node returns only the body. Turn this option on to return the full response (headers and response status code) as well as the body.
* **Never Error**: By default, the node returns success only when the response returns with a 2xx code. Turn this option on to return success regardless of the code returned.
* **Response Format**: Select the format in which the data gets returned. Choose from:
    * **Autodetect** (default): The node detects and formats the response based on the data returned.
    * **File**: Select this option to put the response into a file. Enter the field name where you want the file returned in **Put Output in Field**.
    * **JSON**: Select this option to format the response as JSON.
    * **Text**: Select this option to format the response as plain text. Enter the field name where you want the file returned in **Put Output in Field**.

### Pagination

Use this option to paginate results, useful for handling query results that are too big for the API to return in a single call.

/// note | Inspect the API data first
Some options for pagination require knowledge of the data returned by the API you're using. Before setting up pagination, either check the API documentation, or do an API call without pagination, to see the data it returns.
///
??? Details "Understand pagination"
    Pagination means splitting a large set of data into multiple pages. The amount of data on each page depends on the limit you set.
  
    For example, you make an API call to an endpoint called `/users`. The API wants to send back information on 300 users, but this is too much data for the API to send in one response. 
  
    If the API supports pagination, you can incrementally fetch the data. To do this, you call `/users` with a pagination limit, and a page number or URL to tell the API which page to send. In this example, say you use a limit of 10, and start from page 0. The API sends the first 10 users in its response. You then call the API again, increasing the page number by 1, to get the next 10 results.

Configure the pagination settings:

* **Pagination Mode**:
    * **Off**: Turn off pagination.
    * **Update a Parameter in Each Request**: Use this when you need to dynamically set parameters for each request.
    * **Response Contains Next URL**: Use this when the API response includes the URL of the next page. Use an expression to set **Next URL**.

For example setups, refer to [HTTP Request node cookbook | Pagination](/code/cookbook/http-node/pagination/).

n8n provides built-in variables for working with HTTP node requests and responses when using pagination:

--8<-- "_snippets/integrations/builtin/core-nodes/http/pagination-variables.md"

--8<-- "_snippets/integrations/builtin/core-nodes/http/pagination-api-differences.md"

### Proxy

Use this option if you need to specify an HTTP proxy.

Enter the **Proxy** the request should use.

### Timeout

Use this option to set how long the node should wait for the server to send response headers (and start the response body). The node aborts requests that exceed this value for the initial response.

Enter the **Timeout** time to wait in milliseconds.

## Import curl command

[curl](https://curl.se/){:target=_blank .external-link} is a command line tool and library for transferring data with URLs.

You can use curl to call REST APIs. If the API documentation of the service you want to use provides curl examples, you can copy them out of the documentation and into n8n to configure the HTTP Request node.

Import a curl command:

1. From the HTTP Request node's **Parameters** tab, select **Import cURL**. The **Import cURL command** modal opens.
2. Paste your curl command into the text box.
3. Select **Import**. n8n loads the request configuration into the node fields. This overwrites any existing configuration.

## Templates and examples

<!-- see https://www.notion.so/n8n/Pull-in-templates-for-the-integrations-pages-37c716837b804d30a33b47475f6e3780 -->
[[ templatesWidget(page.title, 'http-request') ]]

## Common issues

For common questions or issues and suggested solutions, refer to [Common Issues](/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/common-issues/).
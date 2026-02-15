Crawler WebAPI Reference
========================

This page contains the complete API reference documentation for the Web Crawler API.

Overview
--------

The Crawler WebAPI is a powerful web scraping and data extraction service that allows you to:

* Fetch raw or rendered HTML from any URL
* Capture screenshots (viewport or full page)
* Extract data using CSS selectors
* Use AI-powered extraction with custom prompts
* Configure proxy settings (datacenter or residential)
* Execute JavaScript scenarios for dynamic content

Authentication
--------------

All API requests require authentication via the ``X-Api-Key`` header.

.. code-block:: http

   POST /api/v1 HTTP/1.1
   X-Api-Key: your-api-key-here
   Content-Type: application/json

Endpoints
---------

POST /api/v1
~~~~~~~~~~~~

The main scraping endpoint. Submit a URL and configuration to extract data from web pages.

**Headers:**

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Parameter
     - Type
     - Description
   * - ``X-Api-Key``
     - string
     - Your API authentication key

**Query Parameters:**

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Parameter
     - Type
     - Description
   * - ``url``
     - string
     - The URL of the web page to scrape

**Request Body:**

The request body contains configuration options for the scraping operation.

GET /
~~~~~

Health check endpoint. Returns a simple string indicating the service status.

Request Schema (RequestV1)
--------------------------

The main request object with optional configuration sections:

.. code-block:: json

   {
     "request": { /* RequestOptions */ },
     "proxy": { /* ProxyOptions */ },
     "jsRender": { /* JsOptions */ },
     "extractData": { /* ExtractDataOptions */ }
   }

RequestOptions
~~~~~~~~~~~~~~

Configure HTTP request settings.

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Property
     - Type
     - Description
   * - ``headers``
     - object
     - Custom HTTP headers as key-value pairs
   * - ``cookies``
     - string
     - Cookies to include in the request

ProxyOptions
~~~~~~~~~~~~

Configure proxy routing for the request.

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Property
     - Type
     - Description
   * - ``country``
     - string
     - Country code for geo-targeting
   * - ``type``
     - EProxyType
     - Proxy type: ``Datacenter`` or ``Residential``

JsOptions
~~~~~~~~~

Configure JavaScript rendering and dynamic content handling.

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Property
     - Type
     - Description
   * - ``waitMs``
     - integer
     - Milliseconds to wait after page load
   * - ``waitForBrowserEvent``
     - string
     - Browser event to wait for
   * - ``waitForSelector``
     - string
     - CSS selector to wait for before extraction
   * - ``window``
     - WindowSize
     - Browser window dimensions (width, height)
   * - ``jsScenario``
     - array
     - List of JavaScript actions to execute

JsScenarioItem
^^^^^^^^^^^^^^

Defines a single JavaScript action in a scenario.

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Property
     - Type
     - Description
   * - ``action``
     - string
     - The action to perform (e.g., "click", "type", "scroll")
   * - ``value``
     - any
     - Action-specific value or parameters

ExtractDataOptions
~~~~~~~~~~~~~~~~~~

Configure data extraction behavior.

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Property
     - Type
     - Description
   * - ``dataToReturn``
     - EDataToReturn
     - Type of data to extract (see values below)
   * - ``rules``
     - array
     - List of extraction rules
   * - ``screenshot``
     - ScreenshotOptions
     - Screenshot configuration

**EDataToReturn Values:**

* ``RawHtml`` - Return the raw HTML source
* ``RenderedHtml`` - Return the fully rendered HTML after JavaScript execution
* ``Screenshot`` - Return a screenshot of the page
* ``ExtractRules`` - Apply CSS selector extraction rules
* ``AiText`` - AI-powered text extraction
* ``AiMarkdown`` - AI-powered markdown extraction
* ``AiExtract`` - AI-powered structured data extraction
* ``AiExtractRules`` - AI-powered extraction with custom rules

ExtractDataRule
^^^^^^^^^^^^^^^

Defines an extraction rule for data extraction.

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Property
     - Type
     - Description
   * - ``selector``
     - string
     - CSS selector to target elements
   * - ``aiPrompt``
     - string
     - AI prompt for intelligent extraction

ScreenshotOptions
^^^^^^^^^^^^^^^^^

Configure screenshot capture settings.

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Property
     - Type
     - Description
   * - ``type``
     - EScreenshotType
     - ``ViewPort`` (visible area) or ``FullPage`` (entire page)
   * - ``scrollToSelector``
     - string
     - CSS selector to scroll to before capture

Response Schema (ResponseV1)
----------------------------

The response object returned by the API.

.. list-table::
   :header-rows: 1
   :widths: 20 15 65

   * - Property
     - Type
     - Description
   * - ``statusCode``
     - integer
     - HTTP status code of the scraped page
   * - ``rawHtml``
     - string
     - Raw HTML content (if requested)
   * - ``renderedHtml``
     - string
     - Rendered HTML after JavaScript execution (if requested)
   * - ``screenshot``
     - string
     - Base64-encoded screenshot image (if requested)
   * - ``extractedText``
     - string
     - AI-extracted text content (if requested)
   * - ``extractedData``
     - any
     - Structured extracted data (if requested)
   * - ``errors``
     - array
     - List of error messages (if any occurred)

Examples
--------

Basic HTML Scraping
~~~~~~~~~~~~~~~~~~~

.. code-block:: json

   {
     "extractData": {
       "dataToReturn": "RawHtml"
     }
   }

Rendered HTML with Wait
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json

   {
     "jsRender": {
       "waitMs": 2000,
       "waitForSelector": ".content-loaded"
     },
     "extractData": {
       "dataToReturn": "RenderedHtml"
     }
   }

Full Page Screenshot
~~~~~~~~~~~~~~~~~~~~

.. code-block:: json

   {
     "jsRender": {
       "waitMs": 1000
     },
     "extractData": {
       "dataToReturn": "Screenshot",
       "screenshot": {
         "type": "FullPage"
       }
     }
   }

CSS Selector Extraction
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json

   {
     "extractData": {
       "dataToReturn": "ExtractRules",
       "rules": [
         { "selector": "h1.title" },
         { "selector": ".product-price" }
       ]
     }
   }

AI-Powered Extraction
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json

   {
     "extractData": {
       "dataToReturn": "AiExtract",
       "rules": [
         { "aiPrompt": "Extract all product names and prices" }
       ]
     }
   }

With Residential Proxy
~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: json

   {
     "proxy": {
       "type": "Residential",
       "country": "US"
     },
     "extractData": {
       "dataToReturn": "RenderedHtml"
     }
   }

Interactive API Documentation
-----------------------------

Below is the interactive API documentation generated from the OpenAPI specification file.

.. raw:: html

   <div id="swagger-ui"></div>
   <script src="https://unpkg.com/swagger-ui-dist@5.9.0/swagger-ui-bundle.js" crossorigin></script>
   <script src="https://unpkg.com/swagger-ui-dist@5.9.0/swagger-ui-standalone-preset.js" crossorigin></script>
   <link rel="stylesheet" href="https://unpkg.com/swagger-ui-dist@5.9.0/swagger-ui.css" />
   <script>
   window.onload = () => {
     window.ui = SwaggerUIBundle({
       url: 'swagger.json',
       dom_id: '#swagger-ui',
       deepLinking: true,
       presets: [
         SwaggerUIBundle.presets.apis,
         SwaggerUIStandalonePreset
       ],
       plugins: [
         SwaggerUIBundle.plugins.DownloadUrl
       ],
       layout: "StandaloneLayout"
     });
   };
   </script>

API Specification File
----------------------

The OpenAPI specification is available as a downloadable file: :download:`swagger.json <swagger.json>`

API Reference (OpenAPI/Swagger)
================================

This page contains the API reference documentation generated from the OpenAPI/Swagger specification.

Overview
--------

The Lumache API provides endpoints for retrieving random ingredients for recipes.
The API follows RESTful principles and returns data in JSON format.

OpenAPI Specification
---------------------

Below is the interactive API documentation generated from the OpenAPI specification file.

.. raw:: html

   <div id="swagger-ui"></div>
   <script src="https://unpkg.com/swagger-ui-dist@5.9.0/swagger-ui-bundle.js" crossorigin></script>
   <script src="https://unpkg.com/swagger-ui-dist@5.9.0/swagger-ui-standalone-preset.js" crossorigin></script>
   <link rel="stylesheet" href="https://unpkg.com/swagger-ui-dist@5.9.0/swagger-ui.css" />
   <script>
   window.onload = () => {
     window.ui = SwaggerUIBundle({
       url: 'openapi.yaml',
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

The OpenAPI specification is available as a downloadable file: :download:`openapi.yaml <openapi.yaml>`

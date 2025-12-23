# OpenAPI Specification Sample

This page demonstrates my ability to write comprehensive OpenAPI (Swagger) specifications. Below you'll find an interactive API explorer for the PokeAPI specification I created.

---

## About This Sample

This is a complete OpenAPI 3.0.3 specification documenting the [PokeAPI](https://pokeapi.co) - a RESTful Pokemon API.

**Skills demonstrated:**

- OpenAPI 3.0 specification writing
- Schema design and data modeling
- API documentation best practices
- Interactive documentation tooling

---

## Interactive API Explorer

!!! tip "Try it out!"
    Use the interactive explorer below to browse endpoints, view schemas, and test API calls directly in your browser.

<div id="swagger-ui"></div>

<link rel="stylesheet" type="text/css" href="https://unpkg.com/swagger-ui-dist@5/swagger-ui.css">
<script src="https://unpkg.com/swagger-ui-dist@5/swagger-ui-bundle.js"></script>
<script>
window.onload = function() {
  SwaggerUIBundle({
    url: "pokeapi-spec.yaml",
    dom_id: '#swagger-ui',
    deepLinking: true,
    presets: [
      SwaggerUIBundle.presets.apis,
      SwaggerUIBundle.SwaggerUIStandalonePreset
    ],
    layout: "BaseLayout",
    defaultModelsExpandDepth: 2,
    defaultModelExpandDepth: 2,
    docExpansion: "list",
    showExtensions: true,
    showCommonExtensions: true,
    tryItOutEnabled: true
  });
};
</script>

<style>
#swagger-ui {
  border: 1px solid var(--md-default-fg-color--lightest);
  border-radius: 8px;
  margin: 1rem 0;
  padding: 1rem;
  background: var(--md-default-bg-color);
}

.swagger-ui .topbar { display: none; }

.swagger-ui .info { margin: 20px 0; }

.swagger-ui .scheme-container {
  background: transparent;
  box-shadow: none;
  padding: 0;
}

.swagger-ui .opblock-tag {
  border-bottom: 1px solid var(--md-default-fg-color--lightest);
}

.swagger-ui .opblock {
  border-radius: 8px;
  margin-bottom: 8px;
}

.swagger-ui .opblock .opblock-summary {
  border-radius: 8px;
}

.swagger-ui .btn {
  border-radius: 4px;
}

/* Dark mode support */
[data-md-color-scheme="slate"] .swagger-ui {
  filter: invert(88%) hue-rotate(180deg);
}

[data-md-color-scheme="slate"] .swagger-ui img {
  filter: invert(100%) hue-rotate(180deg);
}
</style>

---

## Specification Highlights

### Endpoints Documented

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/pokemon` | GET | List all Pokemon with pagination |
| `/pokemon/{idOrName}` | GET | Get detailed Pokemon information |
| `/ability` | GET | List all abilities |
| `/ability/{idOrName}` | GET | Get ability details |
| `/type` | GET | List all Pokemon types |
| `/type/{idOrName}` | GET | Get type with damage relations |
| `/move/{idOrName}` | GET | Get move details |
| `/evolution-chain/{id}` | GET | Get evolution chain |

### Key Features

=== "Request/Response Examples"

    ```yaml
    # Example: Get Pokemon endpoint
    /pokemon/{idOrName}:
      get:
        summary: Get Pokemon by ID or name
        parameters:
          - name: idOrName
            in: path
            required: true
            schema:
              oneOf:
                - type: integer
                - type: string
            examples:
              byId:
                value: 25
              byName:
                value: pikachu
    ```

=== "Schema Definitions"

    ```yaml
    Pokemon:
      type: object
      properties:
        id:
          type: integer
          description: National Pokedex number
        name:
          type: string
          description: Pokemon name (lowercase)
        types:
          type: array
          items:
            $ref: '#/components/schemas/PokemonType'
        stats:
          type: array
          items:
            $ref: '#/components/schemas/PokemonStat'
    ```

=== "Error Handling"

    ```yaml
    responses:
      '200':
        description: Pokemon details
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Pokemon'
      '404':
        description: Pokemon not found
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Error'
    ```

---

## Download Specification

<div class="grid cards" markdown>

-   :material-file-code:{ .lg .middle } **YAML Format**

    ---

    Download the complete OpenAPI specification in YAML format.

    [:octicons-download-16: Download pokeapi-spec.yaml](pokeapi-spec.yaml){ .md-button }

-   :material-code-json:{ .lg .middle } **View Raw**

    ---

    View the raw specification file in a new tab.

    [:octicons-link-external-16: View Raw](pokeapi-spec.yaml){ .md-button target="_blank" }

</div>

---

## Tools Used

| Tool | Purpose |
|------|---------|
| **Swagger Editor** | Initial spec development and validation |
| **Swagger UI** | Interactive documentation (embedded above) |
| **Postman** | API testing and verification |
| **VS Code + OpenAPI Extension** | Editing with IntelliSense |

---

## Best Practices Applied

!!! success "Documentation Standards"

    - **Consistent naming**: All endpoints use lowercase, kebab-case paths
    - **Comprehensive descriptions**: Every endpoint, parameter, and schema documented
    - **Multiple examples**: Query by ID and name shown for flexible endpoints
    - **Reusable components**: Common schemas and parameters defined once, referenced everywhere
    - **Error documentation**: All error responses documented with examples
    - **Pagination support**: List endpoints include limit/offset parameters

---

*This specification was created as a portfolio sample to demonstrate OpenAPI documentation skills.*

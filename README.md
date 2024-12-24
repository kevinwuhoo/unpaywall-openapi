# Unpaywall API OpenAPI Specification

This repository contains an OpenAPI 3.1.1 specification for the [Unpaywall REST API](https://unpaywall.org/products/api).

## Purpose

This OpenAPI specification serves as a machine-readable description of the Unpaywall API, enabling automatic generation of typed client libraries across various programming languages. While this specification aims to be accurate and up-to-date, it is an unofficial community contribution created to facilitate API integration.

## Viewing the Specification

To view the specification, you can open this spec in the [Swagger Editor](https://editor-next.swagger.io/?url=https://raw.githubusercontent.com/kevinwuhoo/unpaywall-openapi/refs/heads/main/unpaywall-openapi.yaml).

## Example

To generate a client library in TypeScript, you can use the Docker image `openapitools/openapi-generator-cli` with the following command.

```bash
docker run --rm \
    -v $PWD:/local openapitools/openapi-generator-cli generate \
    -i https://raw.githubusercontent.com/kevinwuhoo/unpaywall-openapi/main/unpaywall-openapi.yaml \
    -g typescript-axios \
    -o /local/unpaywall-typescript-axios
```

You'll also need to install the following dependencies.

```bash
npm install axios typescript ts-node @types/node
```

Create a file named `test.ts` with the following content. Notice that all responses are fully typed.

```typescript
import { DefaultApi, Configuration } from "./unpaywall-typescript-axios";

async function main() {
  const config = new Configuration({
    basePath: "https://api.unpaywall.org/v2",
  });

  const api = new DefaultApi(config);

  const email = "your.email@example.com"; // Replace with your email

  try {
    const response = await api.doiGet("10.1016/j.cell.2016.11.038", email);

    const paper = response.data;
    console.log("Title:", paper.title);
    console.log("Is Open Access:", paper.is_oa);
    console.log("OA Status:", paper.oa_status);
    console.log("Url:", paper.best_oa_location?.url);
    console.log("PDF URL:", paper.best_oa_location?.url_for_pdf);

    if (paper.z_authors) {
      console.log("\nAuthors:");
      paper.z_authors.forEach((author) => {
        console.log(`- ${author.given} ${author.family}`);
      });
    }
  } catch (error) {
    console.error("Error fetching paper:", error);
  }
}

main();
```

Run the script with.

```bash
npx ts-node test.ts
```

You should see the following output:

```
Title: Perturb-Seq: Dissecting Molecular Circuits with Scalable Single-Cell RNA Profiling of Pooled Genetic Screens
Is Open Access: true
OA Status: bronze
Url: https://doi.org/10.1016/j.cell.2016.11.038
PDF URL: null

Authors:
- Atray Dixit
- Oren Parnas
- Biyu Li
- Jenny Chen
- Charles P. Fulco
- Livnat Jerby-Arnon
- Nemanja D. Marjanovic
- Danielle Dionne
- Tyler Burks
- Raktima Raychowdhury
- Britt Adamson
- Thomas M. Norman
- Eric S. Lander
- Jonathan S. Weissman
- Nir Friedman
- Aviv Regev
```

## Official Documentation

For the most current and authoritative information about the Unpaywall API, please refer to:

- [Official API Documentation](https://unpaywall.org/products/api)
- [Unpaywall Contact](https://unpaywall.org/contact)

## License

MIT License

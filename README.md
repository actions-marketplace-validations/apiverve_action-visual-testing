# APIVerve Visual Testing Action

> Capture screenshots and generate PDFs for visual regression testing and documentation

> **Beta Release** - This action is in beta. We'd love your feedback! [Open an issue](https://github.com/apiverve/action-visual-testing/issues) if you encounter any problems.

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Visual_Testing-blue?logo=github)](https://github.com/apiverve/action-visual-testing)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**[Browse All APIs](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=visual-testing)** | **[Get Free API Key](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=visual-testing)** | **[Documentation](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=visual-testing)**

---

## What does this action do?

This action provides access to APIVerve's Visual Testing APIs directly in your GitHub workflows:

- Capture screenshots for visual regression testing
- Generate PDF snapshots of documentation
- Create visual previews for PR reviews

### Available APIs

| API | Description |
|-----|-------------|
| `webscreenshots` | Web Screenshots is a simple tool for capturing screenshots of web pages. It returns an image screenshot of the web page provided. |
| `websitetopdf` | Website to PDF is a simple tool for converting a website to PDF. It returns the PDF file generated from the website. |
| `htmltopdf` | HTML to PDF is a simple tool for converting HTML to PDF. It returns the PDF file generated from the HTML. |
| `imageconverter` | Image Converter transforms images between formats. Convert HEIC from iPhones, modern WebP and AVIF formats, or classic PNG, JPG, GIF, and TIFF. Includes optional resizing and quality control. |

---

## Quick Start

```yaml
- name: Visual Testing
  uses: apiverve/action-visual-testing@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: webscreenshots
    params: '{&quot;url&quot;: &quot;https://example.com&quot;, &quot;width&quot;: 1280, &quot;height&quot;: 800, &quot;fullpage&quot;: false}'
    output_file: ./screenshot.png
```

---

## Setup

### 1. Get Your API Key

Sign up for a free account at [dashboard.apiverve.com/signup](https://dashboard.apiverve.com/signup?utm_source=github&utm_medium=action&utm_campaign=visual-testing) and create an API key.

### 2. Add Secret to Repository

Go to your repository **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

- Name: `APIVERVE_KEY`
- Value: Your API key from the dashboard

### 3. Use in Workflow

```yaml
- name: Visual Testing
  uses: apiverve/action-visual-testing@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: webscreenshots
    params: '{"your": "parameters"}'
```

---

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `api_key` | Your APIVerve API key (or set `APIVERVE_API_KEY` env var) | Yes* | - |
| `api` | API to use: `webscreenshots`, `websitetopdf`, `htmltopdf`, `imageconverter` | No | `webscreenshots` |
| `params` | JSON parameters for the API | No | `{}` |
| `output_file` | Path to save binary output (images, PDFs) | No | - |
| `format` | Response format: `json`, `yaml`, or `xml` | No | `json` |
| `fail_on_error` | Fail workflow if API returns error | No | `true` |

*\*API key is required but can be provided via input OR `APIVERVE_API_KEY` / `APIVERVE_KEY` environment variable.*

## Outputs

| Output | Description |
|--------|-------------|
| `result` | Full API response as JSON |
| `data` | The `data` field from response as JSON |
| `status` | API status (`ok` or `error`) |
| `file` | Path to downloaded file (if `output_file` was used) |

---

## Examples

### Screenshot Preview

Capture a screenshot of your deployed site

```yaml
- name: Screenshot Preview
  id: visual-testing-0
  uses: apiverve/action-visual-testing@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: webscreenshots
    params: '{&quot;url&quot;: &quot;https://example.com&quot;, &quot;width&quot;: 1280, &quot;height&quot;: 800, &quot;fullpage&quot;: false}'
    output_file: ./screenshot.png

- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: webscreenshots-output
    path: ./screenshot.png
```

### PDF Documentation

Convert a webpage to PDF

```yaml
- name: PDF Documentation
  id: visual-testing-1
  uses: apiverve/action-visual-testing@v1
  with:
    api_key: ${{ secrets.APIVERVE_KEY }}
    api: websitetopdf
    params: '{&quot;url&quot;: &quot;https://docs.example.com&quot;}'
    output_file: ./docs.pdf

- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: websitetopdf-output
    path: ./docs.pdf
```


---

## Full Workflow Example

```yaml
name: Visual Testing Workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  visual-testing:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Visual Testing
        id: result
        uses: apiverve/action-visual-testing@v1
        with:
          api_key: ${{ secrets.APIVERVE_KEY }}
          api: webscreenshots
          params: '{&quot;url&quot;: &quot;https://example.com&quot;, &quot;width&quot;: 1280, &quot;height&quot;: 800, &quot;fullpage&quot;: false}'
          output_file: ./screenshot.png

      - name: Show result
        run: |
          echo "Status: ${{ steps.result.outputs.status }}"
          echo "Data: ${{ steps.result.outputs.data }}"
```

---

## Related Actions

Looking for more APIVerve actions?

- [apiverve/action](https://github.com/apiverve/action) - Generic action for all 350+ APIs
- [apiverve/action-release-assets](https://github.com/apiverve/action-release-assets) - Generate QR codes, barcodes, and badges for your GitHub releases
- [apiverve/action-dns-monitor](https://github.com/apiverve/action-dns-monitor) - Verify DNS configuration, check propagation, and validate DNSSEC after deployments
- [apiverve/action-domain-health](https://github.com/apiverve/action-domain-health) - Monitor domain expiration, WHOIS changes, and domain availability

**[Browse all APIVerve Actions →](https://github.com/marketplace?query=apiverve)**

---

## Pricing

- **Free tier** - Get started with generous free limits
- **Pro plans** - Higher rate limits and priority support for production use

Check out [pricing details](https://apiverve.com/pricing?utm_source=github&utm_medium=action&utm_campaign=visual-testing).

---

## Resources

- **API Documentation**: [docs.apiverve.com](https://docs.apiverve.com?utm_source=github&utm_medium=action&utm_campaign=visual-testing)
- **API Marketplace**: [apiverve.com/marketplace](https://apiverve.com/marketplace?utm_source=github&utm_medium=action&utm_campaign=visual-testing)
- **Issues & Support**: [GitHub Issues](https://github.com/apiverve/action-visual-testing/issues)
- **Email**: support@apiverve.com

---

## License

MIT - see [LICENSE](LICENSE)

---

Built by [APIVerve](https://apiverve.com?utm_source=github&utm_medium=action&utm_campaign=visual-testing) - 350+ APIs for developers

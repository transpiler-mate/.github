# Transpiler-Mate

**Turn CWL workflows into documentation, diagrams, metadata, and service descriptions.**

Transpiler-Mate is a collection of open-source tools built around Common Workflow Language (CWL). Describe your workflow and its software metadata once, then use independent plugins to generate the artifacts you need to document, share, and publish it.

[Explore the repositories](https://github.com/orgs/transpiler-mate/repositories) · [Get started](#get-started) · [Build a plugin](#build-a-plugin)

## What can you do with Transpiler-Mate?

| Task | Project | What it provides |
| --- | --- | --- |
| Document workflows | [cwl2markdown](https://github.com/transpiler-mate/cwl2markdown) | Markdown pages with workflow details and software metadata. |
| Visualize workflows | [cwl2puml](https://github.com/transpiler-mate/cwl2puml) | PlantUML diagrams, with optional PNG or SVG rendering. |
| Generate command-line interfaces | [cwl2click](https://github.com/transpiler-mate/cwl2click) | Python Click CLI scaffolding from CWL command-line tools. |
| Describe processing services | [cwl2ogc](https://github.com/transpiler-mate/cwl2ogc) | OGC API – Processes input/output descriptors and JSON Schemas. |
| Export software metadata | [cwl2codemeta](https://github.com/transpiler-mate/cwl2codemeta) | CodeMeta JSON-LD derived from embedded Schema.org metadata. |
| Prepare citation metadata | [cwl2datacite](https://github.com/transpiler-mate/cwl2datacite) | DataCite metadata JSON for workflow software. |
| Annotate container images | [cwl2oci](https://github.com/transpiler-mate/cwl2oci) | OCI image annotation JSON with software and CWL process metadata. |
| Publish research software | [invenio-publish](https://github.com/transpiler-mate/invenio-publish) | Records, attachments, DOIs, and new versions in InvenioRDM or Zenodo. |

## Get started

Use Python 3.10 or newer. Install the runtime and the plugins you need in the same Python environment:

```console
python -m pip install transpiler-mate-runtime cwl2markdown cwl2puml
transpiler-mate --help
```

With a CWL document containing the required Schema.org `SoftwareApplication` metadata, generate documentation and diagrams:

```console
transpiler-mate cwl2markdown --output build/docs workflow.cwl
transpiler-mate cwl2puml --output build/diagrams workflow.cwl
```

Installed plugins become subcommands of `transpiler-mate`. Run `transpiler-mate <plugin> --help` for their options. See the [cwl2markdown documentation](https://github.com/transpiler-mate/cwl2markdown#documentation) for a complete metadata example, or explore the [runtime repository](https://github.com/transpiler-mate/transpiler-mate-runtime) for loading and bundling CWL documents.

## The foundations

The runtime loads CWL documents, prepares a shared context, and discovers installed plugins. A separate API package defines the contracts that let plugins be developed and distributed independently.

| Project | Role |
| --- | --- |
| [transpiler-mate-runtime](https://github.com/transpiler-mate/transpiler-mate-runtime) | The `transpiler-mate` CLI, plugin discovery and execution, source loading, and built-in CWL bundling. |
| [transpiler-mate-api](https://github.com/transpiler-mate/transpiler-mate-api) | Shared plugin contracts and models for plugin authors and runtime implementations. |
| [cwl-loader](https://github.com/transpiler-mate/cwl-loader) | Python utilities for loading, normalizing, and serializing CWL documents. |

## Author and validate

These companion tools help prepare workflows before conversion or execution:

| Project | Role |
| --- | --- |
| [cwl-metadata-editor](https://github.com/transpiler-mate/cwl-metadata-editor) | A VS Code extension for editing Schema.org metadata directly in CWL files while preserving surrounding text and formatting. |
| [assertions-mate](https://github.com/transpiler-mate/assertions-mate) | Input validation using JSON Schema, Rego policies, and CQL2 assertions embedded as CWL hints. |

## Build a plugin

Start with the [transpiler-mate-plugin-project-template](https://github.com/transpiler-mate/transpiler-mate-plugin-project-template), a Copier template with Python packaging, tests, documentation, and CI. Implement the [plugin API](https://github.com/transpiler-mate/transpiler-mate-api) and register your package in the `transpiler_mate.plugins` entry-point group so the runtime can discover it.

## Get involved

Bug reports, examples, documentation improvements, and new plugins are welcome. Open an issue or pull request in the relevant repository, and follow its contribution and development instructions.

This [.github repository](https://github.com/transpiler-mate/.github) hosts the organization's landing-page and profile content.

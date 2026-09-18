# Transpiler-Mate

**Make CWL workflows easier to discover, cite, reuse, and inspect.**

Transpiler-Mate is a collection of open-source tools built around Common Workflow Language (CWL). Describe your workflow and its software metadata once, then generate documentation, input templates, citations, research objects, service descriptions, and software bills of materials with independent plugins. These artifacts support FAIR research software practices and supply-chain assessment.

[Explore the repositories](https://github.com/orgs/transpiler-mate/repositories) · [Get started](#get-started) · [Build a plugin](#build-a-plugin)

## What can you do with Transpiler-Mate?

| Task | Project | What it provides | Documentation |
| --- | --- | --- | --- |
| Document workflows | [cwl2markdown](https://github.com/transpiler-mate/cwl2markdown) | Markdown pages with workflow details and software metadata. | [Docs](https://transpiler-mate.github.io/cwl2markdown/) |
| Visualize workflows | [cwl2puml](https://github.com/transpiler-mate/cwl2puml) | PlantUML diagrams, with optional PNG or SVG rendering. | [Docs](https://transpiler-mate.github.io/cwl2puml/) |
| Prepare workflow inputs | [cwl2inputs](https://github.com/transpiler-mate/cwl2inputs) | YAML input templates generated with cwltool for a selected CWL process. | [Docs](https://transpiler-mate.github.io/cwl2inputs/) |
| Generate command-line interfaces | [cwl2click](https://github.com/transpiler-mate/cwl2click) | Python Click CLI scaffolding from CWL command-line tools. | [Docs](https://transpiler-mate.github.io/cwl2click/) |
| Describe processing services | [cwl2ogc](https://github.com/transpiler-mate/cwl2ogc) | OGC API – Processes input/output descriptors and JSON Schemas. | [Docs](https://transpiler-mate.github.io/cwl2ogc/) |
| Describe catalog records | [cwl2ogcrecords](https://github.com/transpiler-mate/cwl2ogcrecords) | CWL as OGC API – Records. | [Docs](https://transpiler-mate.github.io/cwl2ogcrecords/) |
| Export software metadata | [cwl2codemeta](https://github.com/transpiler-mate/cwl2codemeta) | CodeMeta JSON-LD derived from embedded Schema.org metadata. | [Docs](https://transpiler-mate.github.io/cwl2codemeta/) |
| Prepare publication metadata | [cwl2datacite](https://github.com/transpiler-mate/cwl2datacite) | DataCite metadata JSON for workflow software. | [Docs](https://transpiler-mate.github.io/cwl2datacite/) |
| Generate citations | [cwl2citation](https://github.com/transpiler-mate/cwl2citation) | CFF, BibTeX, RIS, CSL-JSON, and styled text, with configurable CSL styles. | [Docs](https://transpiler-mate.github.io/cwl2citation/) |
| Package research objects | [cwl2ro-crate](https://github.com/transpiler-mate/cwl2ro-crate) | Workflow RO-Crates, or Provenance Run Crates from existing CWLProv execution records. | [Docs](https://transpiler-mate.github.io/cwl2ro-crate/) |
| Inventory container dependencies | [cwl2sbom](https://github.com/transpiler-mate/cwl2sbom) | Local Trivy CycloneDX SBOMs, workflow inventory, image identity lock, and coverage report. | [Docs](https://transpiler-mate.github.io/cwl2sbom/) |
| Annotate container images | [cwl2oci](https://github.com/transpiler-mate/cwl2oci) | OCI image annotation JSON with software and CWL process metadata. | [Docs](https://transpiler-mate.github.io/cwl2oci/) |
| Publish research software | [invenio-publish](https://github.com/transpiler-mate/invenio-publish) | Records, attachments, DOIs, and new versions in InvenioRDM or Zenodo. | [Docs](https://transpiler-mate.github.io/invenio-publish/) |

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

Installed plugins become subcommands of `transpiler-mate`. Run `transpiler-mate <plugin> --help` for their options. See the [cwl2markdown documentation](https://transpiler-mate.github.io/cwl2markdown/) for a complete metadata example, or explore the [runtime documentation](https://transpiler-mate.github.io/transpiler-mate-runtime/) for loading and bundling CWL documents.

## Support FAIR research software

Use CodeMeta, DataCite, and OGC Records exports to describe workflows for discovery; provide citations with `cwl2citation`; and package workflows or recorded executions with `cwl2ro-crate`. Input templates, documentation, and diagrams help others understand and reuse the software. `invenio-publish` handles publication to InvenioRDM or Zenodo.

After installing the corresponding plugins alongside the runtime:

```console
transpiler-mate cwl2inputs --output build/inputs.yaml 'workflow.cwl#main'
transpiler-mate cwl2citation --output build/citations 'workflow.cwl#main'
transpiler-mate cwl2rocrate --output build/crate --zip 'workflow.cwl#main'
```

The `cwl2ro-crate` distribution registers the command `cwl2rocrate`. Its optional `--run` argument accepts an existing CWLProv directory to package execution provenance.

## Inspect the software supply chain

With Trivy installed, generate SBOMs for the containers referenced by a selected workflow:

```console
transpiler-mate cwl2sbom --platform linux/amd64 --output build/sbom 'workflow.cwl#main'
```

`cwl2sbom` follows nested workflows, inspects declared images with Trivy, and records image identities and coverage gaps. It produces local artifacts for your pipeline: use **ORAS** for OCI publication and attachment, then **Trivy** for downstream vulnerability and license assessment of the exported image SBOMs. Offline vulnerability scanning requires a provisioned database.

See the [offline Trivy scanning guide](https://transpiler-mate.github.io/cwl2sbom/how-to/offline-scanning/) for database preparation, per-image reports, and policy checks.

## The foundations

The runtime loads CWL documents, prepares a shared context, and discovers installed plugins. A separate API package defines the contracts that let plugins be developed and distributed independently.

| Project | Role | Documentation |
| --- | --- | --- |
| [transpiler-mate-runtime](https://github.com/transpiler-mate/transpiler-mate-runtime) | The `transpiler-mate` CLI, plugin discovery and execution, source loading, and built-in CWL bundling. | [Docs](https://transpiler-mate.github.io/transpiler-mate-runtime/) |
| [transpiler-mate-api](https://github.com/transpiler-mate/transpiler-mate-api) | Shared plugin contracts and models for plugin authors and runtime implementations. | [Docs](https://transpiler-mate.github.io/transpiler-mate-api/) |
| [cwl-loader](https://github.com/transpiler-mate/cwl-loader) | Python utilities for loading, normalizing, and serializing CWL documents. | [Docs](https://transpiler-mate.github.io/cwl-loader/) |

## Author and validate

These companion tools help prepare workflows before conversion or execution:

| Project | Role | Documentation |
| --- | --- | --- |
| [cwl-metadata-editor](https://github.com/transpiler-mate/cwl-metadata-editor) | A VS Code extension for editing Schema.org metadata directly in CWL files while preserving surrounding text and formatting. | [Docs](https://transpiler-mate.github.io/cwl-metadata-editor/) |
| [assertions-mate](https://github.com/transpiler-mate/assertions-mate) | Input validation using JSON Schema, Rego policies, and CQL2 assertions embedded as CWL hints. | [Docs](https://transpiler-mate.github.io/assertions-mate/) |

## Build a plugin

Start with the [transpiler-mate-plugin-project-template](https://github.com/transpiler-mate/transpiler-mate-plugin-project-template), a Copier template with Python packaging, tests, documentation, and CI. Implement the [plugin API](https://transpiler-mate.github.io/transpiler-mate-api/) and register your package in the `transpiler_mate.plugins` entry-point group so the runtime can discover it.

## Get involved

Bug reports, examples, documentation improvements, and new plugins are welcome. Open an issue or pull request in the relevant repository, and follow its contribution and development instructions.

This [.github repository](https://github.com/transpiler-mate/.github) hosts the organization's landing-page and profile content.

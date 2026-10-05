# Transpiler-Mate

**Make CWL workflows easier to discover, cite, reuse, and inspect.**

Transpiler-Mate is a collection of open-source tools built around Common Workflow Language (CWL). Describe your workflow and its [Software Application metadata](https://transpiler-mate.github.io/transpiler-mate-api/reference/software-metadata/) once, then generate documentation, input templates, citations, research objects, service descriptions, and software bills of materials with independent plugins. These artifacts support FAIR research software practices and supply-chain assessment.

- [Explore the repositories](https://github.com/orgs/transpiler-mate/repositories)
- [Get started](#get-started)
- [What can you do with Transpiler-Mate?](#what-can-you-do-with-transpiler-mate)
- [Build a plugin](#build-a-plugin)
- [Edit Software Application metadata](https://transpiler-mate.github.io/.github/metadata-generator.html)

## Get started

Use Python 3.10 or newer. Install the runtime and the plugins you need in the same Python environment:

```bash
python -m pip install transpiler-mate-runtime ${TM_PLUGIN_1} ... ${TM_PLUGIN_N}
transpiler-mate --help
```

Installed plugins become subcommands of `transpiler-mate`. Run `transpiler-mate <plugin> --help` for their options.

## The foundations

The runtime loads CWL documents, prepares a shared context, and discovers installed plugins. A separate API package defines the contracts that let plugins be developed and distributed independently.

| Project | Role | Documentation |
| --- | --- | --- |
| [transpiler-mate-runtime](https://github.com/transpiler-mate/transpiler-mate-runtime) | The `transpiler-mate` CLI, plugin discovery and execution, source loading, and built-in CWL bundling. | [Docs](https://transpiler-mate.github.io/transpiler-mate-runtime/) |
| [transpiler-mate-api](https://github.com/transpiler-mate/transpiler-mate-api) | Shared plugin contracts and models for plugin authors and runtime implementations. | [Docs](https://transpiler-mate.github.io/transpiler-mate-api/) |
| [cwl-loader](https://github.com/transpiler-mate/cwl-loader) | Python utilities for loading, normalizing, and serializing CWL documents. | [Docs](https://transpiler-mate.github.io/cwl-loader/) |

## What can you do with Transpiler-Mate?

### Inspect the software supply chain

| Task | Project | What it provides | Documentation |
| --- | --- | --- | --- |
| Inventory container dependencies | [cwl2sbom](https://github.com/transpiler-mate/cwl2sbom) | Local Trivy CycloneDX SBOMs, workflow inventory, image identity lock, and coverage report. | [Docs](https://transpiler-mate.github.io/cwl2sbom/) |

With Trivy installed, generate SBOMs for the containers referenced by a selected workflow.

`cwl2sbom` follows nested workflows, inspects declared images with Trivy, and records image identities and coverage gaps. It produces local artifacts for your pipeline: use **ORAS** for OCI publication and attachment, then **Trivy** for downstream vulnerability and license assessment of the exported image SBOMs. Offline vulnerability scanning requires a provisioned database.

See the [offline Trivy scanning guide](https://transpiler-mate.github.io/cwl2sbom/how-to/offline-scanning/) for database preparation, per-image reports, and policy checks.

### Analysis and reporting

| Task | Project | What it provides | Documentation |
| --- | --- | --- | --- |
| Compare CWL releases | [cwl-baseline-plugin](https://github.com/transpiler-mate/cwl-baseline-plugin) | An explainable JSON report comparing resolved CWL releases, with a minimum SemVer increment. | [Docs](https://transpiler-mate.github.io/cwl-baseline-plugin/) |

### Documentation generation

| Task | Project | What it provides | Documentation |
| --- | --- | --- | --- |
| Document workflows | [cwl2markdown](https://github.com/transpiler-mate/cwl2markdown) | Markdown pages with workflow details and software metadata. | [Docs](https://transpiler-mate.github.io/cwl2markdown/) |
| Visualize workflows | [cwl2puml](https://github.com/transpiler-mate/cwl2puml) | PlantUML diagrams, with optional PNG or SVG rendering. | [Docs](https://transpiler-mate.github.io/cwl2puml/) |
| Explore workflows interactively | [cwl2webgl](https://github.com/transpiler-mate/cwl2webgl) | A self-contained, offline HTML explorer for workflow dependencies, nested workflows, ports, and step bindings. | [Docs](https://transpiler-mate.github.io/cwl2webgl/) |

With a CWL document containing the required Schema.org `SoftwareApplication` metadata, generate documentation and diagrams.

### Formats conversion

| Task | Project | What it provides | Documentation |
| --- | --- | --- | --- |
| Describe processing services | [cwl2ogc](https://github.com/transpiler-mate/cwl2ogc) | OGC API – Processes input/output descriptors and JSON Schemas. | [Docs](https://transpiler-mate.github.io/cwl2ogc/) |

### Support FAIR research software

| Task | Project | What it provides | Documentation |
| --- | --- | --- | --- |
| Prepare publication metadata | [cwl2datacite](https://github.com/transpiler-mate/cwl2datacite) | DataCite metadata JSON for workflow software. | [Docs](https://transpiler-mate.github.io/cwl2datacite/) |
| Generate citations | [cwl2citation](https://github.com/transpiler-mate/cwl2citation) | CFF, BibTeX, RIS, CSL-JSON, and styled text, with configurable CSL styles. | [Docs](https://transpiler-mate.github.io/cwl2citation/) |
| Publish research software | [invenio-publish](https://github.com/transpiler-mate/invenio-publish) | Records, attachments, DOIs, and new versions in InvenioRDM or Zenodo. | [Docs](https://transpiler-mate.github.io/invenio-publish/) |
| Package research objects | [cwl2ro-crate](https://github.com/transpiler-mate/cwl2ro-crate) | Workflow RO-Crates, or Provenance Run Crates from existing CWLProv execution records. | [Docs](https://transpiler-mate.github.io/cwl2ro-crate/) |
| Describe catalog records | [cwl2ogcrecords](https://github.com/transpiler-mate/cwl2ogcrecords) | CWL as OGC API – Records. | [Docs](https://transpiler-mate.github.io/cwl2ogcrecords/) |
| Export software metadata | [cwl2codemeta](https://github.com/transpiler-mate/cwl2codemeta) | CodeMeta JSON-LD derived from embedded Schema.org metadata. | [Docs](https://transpiler-mate.github.io/cwl2codemeta/) |

Use CodeMeta, DataCite, and OGC Records exports to describe workflows for discovery; provide citations with `cwl2citation`; and package workflows or recorded executions with `cwl2ro-crate`. `invenio-publish` handles publication to InvenioRDM or Zenodo.

The `cwl2ro-crate` distribution registers the command `cwl2rocrate`. Its optional `--run` argument accepts an existing CWLProv directory to package execution provenance.

### Software generation

| Task | Project | What it provides | Documentation |
| --- | --- | --- | --- |
| Dereference a CWL document and create a uber-CWL | [bundle](https://github.com/transpiler-mate/transpiler-mate-runtime/) | A bundled, dereferenced CWL document | [Docs](https://transpiler-mate.github.io/transpiler-mate-runtime/reference/bundle-plugin/) |
| Generate command-line interfaces | [cwl2click](https://github.com/transpiler-mate/cwl2click) | Python Click CLI scaffolding from CWL command-line tools. | [Docs](https://transpiler-mate.github.io/cwl2click/) |
| Annotate container images | [cwl2oci](https://github.com/transpiler-mate/cwl2oci) | OCI image annotation JSON with software and CWL process metadata. | [Docs](https://transpiler-mate.github.io/cwl2oci/) |
| Prepare workflow inputs | [cwl2inputs](https://github.com/transpiler-mate/cwl2inputs) | YAML input templates generated with cwltool for a selected CWL process. | [Docs](https://transpiler-mate.github.io/cwl2inputs/) |
| Compose Earth observation workflows | [eoap-cwlwrap](https://github.com/EOEPCA/eoap-cwlwrap) | Type-safe composition of CWL steps with stage-in and stage-out patterns, packed into a self-contained CWL document. | [Docs](https://eoepca.github.io/eoap-cwlwrap/) |

## Validate workflow inputs

Validate workflow inputs before execution:

| Project | Role | Documentation |
| --- | --- | --- |
| [assertions-mate](https://github.com/transpiler-mate/assertions-mate) | Input validation using JSON Schema, Rego policies, and CQL2 assertions embedded as CWL hints. | [Docs](https://transpiler-mate.github.io/assertions-mate/) |

### Plugins batch execution

> [!WARNING]
> Available since version **1.1.0** of the transpiler-mate-runtime

The [batch](https://transpiler-mate.github.io/transpiler-mate-runtime/reference/batch-plugin/) plugin runs multiple plugin executions sequentially with the same resolved CWL context. An execution plan in YAML specifies the plugins and their inputs.

Create `tmom.yaml` in your working directory:

```yaml
cwl2sbom:
- platform: 'linux/amd64'
  output: 'build/sbom'

cwl2markdown:
- output: 'build/docs'

cwl2puml:
- output: 'build/diagrams'

cwl2webgl:
- output: 'build/explorers'

baseline:
- previous: 'oci://mycompany.org/released.cwl'
  output: 'baseline.json'
```

Then run:

```bash
transpiler-mate batch workflow.cwl#main
```

## Build a plugin

Start with the [transpiler-mate-plugin-project-template](https://github.com/transpiler-mate/transpiler-mate-plugin-project-template), a Copier template with Python packaging, tests, documentation, and CI. Implement the [plugin API](https://transpiler-mate.github.io/transpiler-mate-api/) and register your package in the `transpiler_mate.plugins` entry-point group so the runtime can discover it.

## Get involved

Bug reports, examples, documentation improvements, and new plugins are welcome. Open an issue or pull request in the relevant repository, and follow its contribution and development instructions.

This [.github repository](https://github.com/transpiler-mate/.github) hosts the organization's landing-page and profile content.

# ROADMAP

__Radiology Ontology of AI Datasets, Models and Projects__

The Radiology Ontology of AI Models, Datasets, and Projects (ROADMAP) 
provides a standardized language for the metadata that describe artificial intelligence
resources.  

ROADMAP builds on "model cards" and "datasheets for datasets" to provide a structured
taxonomy. Concepts include:
* preferred name (`rdfs:label`)
* synonyms and abbreviations (`skos:altLabel`)
* Spanish and Chinese translations
* definitions (`skos:definition`)
* authoritative reference(s)
* links to other knowledge resources

ROADMAP highlights features specific to medical imaging and references concepts from related ontologies, coding schemes, and common data elements. In accordance with the FAIR guiding principles, application of the ontology will allow AI resources to be more readily discoverable and reusable. Its application improves the ability to match AI models with relevant datasets and  facilitates detection of potential biases in released AI models.  

## Ontology

* The [ontology](ROADMAP.ttl) in Turtle (OWL) syntax.
  Open with Prot&eacute;g&eacute;.
 
* The Metrics components of the ontology provides a compendium of AI model performance measures
    * View an [alphabetical list](ROADMAP%20-%20Metrics.txt) of 216 scalar metrics.

ROADMAP builds on "model cards" and "datasheets for datasets" to provide a structured
taxonomy. Concepts include:
* preferred name
* synonyms and abbreviations
* Spanish and Chinese translations
* definition
* authoritative reference(s)

## What's New

Release **2026-11** is a substantial revision of the 2026-05 release
(`old versions/ROADMAP-2026-05.omn`). The ontology roughly doubled in size, gained a
governance layer linking metadata to reporting guidelines and ethical principles, and
moved from a largely taxonomic representation to one with explicit logical axioms.

### At a glance

| | 2026-05 | 2026-11 |
|---|---:|---:|
| Classes | 545 | 686 |
| Object properties | 22 | 40 |
| Datatype properties | 0 | 19 |
| Named individuals | 4 | 623 |
| **Total entities** | **575** | **1,372** |
|---|---:|---:|
| Definitions (`skos:definition`) | 571 | 1,349 |
| External mappings (`skos:exactMatch`) | 0 | 156 |
| Logical restrictions | 240 | 1,145 |

### Serialization and identifiers

* The ontology is now published in **Turtle** (`ROADMAP.ttl`) rather than Manchester
  syntax (`.omn`).
* Entity IRIs dropped the redundant `ROADMAP_` infix:
  `https://rsna.org/roadmap/ROADMAP_00011` is now
  `https://rsna.org/roadmap/00011`. **This is a breaking change for anything that
  referenced the old IRIs.**
* The ontology IRI is now `https://rsna.org/roadmap/` rather than a WebProtégé URN.
* Ontology-level metadata uses Dublin Core Terms (`dcterms:title`, `dcterms:creator`,
  `dcterms:license`, `dcterms:publisher`, `dcterms:source`, `dcterms:modified`) and
  carries `owl:versionInfo`. The 2026-05 file recorded only an OBO format version.

### A principled upper level

The 2026-05 release placed 24 classes directly beneath the root, mixing resources
(*dataset*, *model*, *project*) with their attributes (*count*, *descriptor*,
*image characteristic*) and with evaluation concepts (*performance criterion*).

The release reorganizes these under **eight upper-level classes**: *AI resource*,
*agent*, *data entity*, *evaluation entity*, *governance entity*, *information entity*,
*process*, and *unit*. The eight are asserted pairwise disjoint, so a reasoner now
detects misplaced entities instead of silently accepting them.

### Reporting guidelines and ethical principles (new)

An entire governance layer was added under *governance entity*:

* **21 reporting guidelines**, of which 11 are itemized into **391 guideline items** —
  CLAIM (44), CONSORT-AI (37), DECIDE-AI (27), MI-CLAIM (19), MINIMAR (21),
  PROBAST+AI (34), REFINE (44), SPIRIT-AI (51), STARD-AI (48), TRAM-AI (14) and
  TRIPOD+AI (52).
* **4 ethical frameworks** comprising **21 principles**: the OECD AI Principles, the
  WHO guidance, the FAIR Guiding Principles and FUTURE-AI. FAIR and FUTURE-AI are
  modeled as *specific ethical principles* and are the targets of the `supports`
  relation; the OECD and WHO principles are modeled as *general ethical principles*.

### The `supports` relation (new)

A new `supports` property (`:00198`) and its inverse `is supported by` (`:00222`)
connect metadata to reporting requirements and guiding principles. The release asserts
**1,330 `supports` axioms**:

| Relationship | Encoding | Count |
|---|---|---:|
| AI resource metadata → guideline item | `owl:hasValue` restriction | 555 |
| AI resource metadata → ethical principle | `owl:hasValue` restriction | 57 |
| guideline item → ethical principle | property assertion | 718 |

Metadata links are class restrictions, so subclasses inherit them. `supports` is
deliberately **not transitive** and has no property chain: guideline items are often
disjunctive ("report A or B"), so composing the two hops would equivocate across the
disjunction. Four sub-properties (`:03918`–`:03921`) are declared and reserved for a
future split; all axioms are currently asserted on `:00198` itself.

### Richer logical axioms

The 2026-05 release used almost exclusively existential restrictions
(`owl:someValuesFrom`) and asserted no disjointness or equivalence at all. The release
adds:

* 997 `owl:hasValue` restrictions (chiefly the `supports` links),
* 49 `owl:equivalentClass` definitions — including MeSH-aligned age groups defined by
  numeric interval, so that *adult 19 – 44 years* and the rest are computed rather than
  asserted,
* 15 `owl:AllDisjointClasses` axioms and 14 pairwise `owl:disjointWith` assertions,
* qualified cardinality constraints on confidence intervals, model performance tuples
  and age ranges.

The ontology is **consistent under HermiT with no unsatisfiable classes**.

### Expanded content

| Area | 2026-05 | 2026-11 |
|---|---:|---:|
| AI resource metadata (all descriptors) | — | 240 |
| Dataset properties | 12 | 125 |
| Model properties | 35 | 90 |
| Performance metrics | 230 | 269 |
| Regulatory information | 5 | 21 |
| Patient descriptors | 38 | 51 |

Notable additions include a **regulatory subtree** (pathway, authorization status,
jurisdiction, authority, risk class and submission/decision/expiration dates) that
replaces the two flags *FDA cleared* and *CE marked*; a **deployment and integration
subtree** (FHIR resources, DICOM and DICOMweb services, HL7 v2 message types, IHE
Radiology integration profiles); the seven **race and ethnicity** categories, which
populate a class that was present but empty in 2026-05; and the first
**19 datatype properties**, which let numeric and string values be queried directly
rather than encoded as annotations.

### Model performance and value representation, reworked

The 2026-05 classes *model performance value*, *observed value*, *lower bound value*
and *upper bound value*, together with the `has lower bound` / `has upper bound`
properties and the `lowerBound` / `upperBound` annotation properties, were removed.
Model performance is now a tuple — split into *model performance by scalar metric* and
*model performance by non-scalar metric* — that links a metric, an observed value, an
evaluation dataset and an optional confidence interval, with confidence limits and
confidence level expressed as datatype properties.

### Alignment with external vocabularies (new)

The release adds **156 `skos:exactMatch` mappings**, where the 2026-05 release had
none: OBO ontologies such as OBI, STATO and IAO (44), SNOMED CT (29), RadLex (29),
MeSH (14), the Units Ontology (3) and LOINC (2).

### Documentation

Definitions more than doubled, from 571 to 1,349. English labels grew from 576 to
1,373; Spanish and Chinese labels were extended to 570 and 552 respectively. Alternate
labels grew from 941 to 1,265.

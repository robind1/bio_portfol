---
title: "SARS-CoV-2 Genomic Analysis to FHIR"
excerpt: "Pipeline for SARS-CoV-2 variant, lineage, and clade analysis with FHIR Genomics clinical reporting."
teaser: "/images/cov2tofhir.png"
date: 2026-08-22
tags:
  - Nextflow
  - SARS-CoV-2
  - FHIR
  - Pathogen genomics
---

## Overview

This project develops a platform-agnostic pipeline for SARS-CoV-2 sequencing data from Illumina, Oxford Nanopore to identify variants, assign Pango lineage and Nextstrain clade. Adapted from [nf-core/viralrecon](https://nf-co.re/viralrecon/3.0.0), the workflow converts its results into [HL7 FHIR Genomics resources](https://build.fhir.org/genomics.html) so that both the genomic findings and the clinical metadata attached to them can be shared in a structured, interoperable format.

## Workflow

- The pipeline automatically detects whether the input is Illumina or Nanopore and routes samples to the correct sub-workflow.
- Performs quality control, trimming, and human read depletion before alignment.
- Handles amplicon data with per-sample primer scheme detection and clipping.
- Calls variants and builds consensus genomes with platform-specific tools then annotates them against the reference for gene, HGVS, and Sequence Ontology.
- Assigns Pango lineage, Nextstrain clade, WHO variant label, and QC.
- Converts annotated results into HL7 FHIR R4 resources including variant observations, a SARS-CoV-2 panel observation, lineage and genome quality observations, and a diagnostic report.
- Merges the genomic bundle with patient, organization, and practitioner metadata.

## Why This Matters Clinically

SARS-CoV-2 genomic surveillance is limited by how fast a genome travels from swab to a shareable, interpretable record. This pipeline targets that gap.

**Timeliness is the binding constraint, not sequencing volume alone.** Analysis of 189 countries found that sequencing around 0.5% of confirmed cases with a turnaround time below 21 days is a workable benchmark for surveillance, yet only 25% of genomes from high-income countries and 5% from low- and middle-income countries were submitted within that window ([Brito et al. 2022](https://doi.org/10.1038/s41467-022-33713-y)). Turnaround matters because it changes what can be detected in time to act.

**Platform heterogeneity mirrors capacity disparity.** High-income countries submitted roughly ten times more sequences per COVID-19 case than low- and middle-income countries, and low-income countries sequencing a mean of about 10 genomes per week may miss a lineage circulating at up to 21.7% prevalence ([Brito et al. 2022](https://doi.org/10.1038/s41467-022-33713-y)). That capacity gap is also a platform gap, with Illumina dominating well-resourced centres and Nanopore deployed at decentralized and resource-limited sites.

**Missing metadata is what makes shared genomes hard to use.** Across GISAID submissions, about 63% of sequences lacked demographic information, 84% lacked a sampling strategy, and more than 95% lacked patient-level clinical information, with a global average metadata completeness of 5.6 out of 10 ([Chen et al. 2022](https://doi.org/10.1038/s41588-022-01033-y)). The same analysis found that more than one-third of countries with official variant counts had shared fewer than half of their VOC sequences publicly. A sequence without collection context cannot support severity, vaccine breakthrough, or transmission analyses.

**FHIR interoperability connects the genome to the health system.** Encoding variants, lineage, WHO variant designation, and consensus quality as HL7 FHIR Genomics resources makes them machine-readable by electronic health records, clinical decision support, and national surveillance registries without format translation.

```json
{
  "conclusion": "SARS-CoV-2 lineage B.1.258 detected. Nextstrain clade 20A. Not designated: Lineage B.1.258 carries no WHO variant designation. Spike changes: S:T22I, S:N439K, S:D614G. Lineage assigned by nextclade 3.18.1 (database 2026-06-16--14-30-45Z). Reference genome: NC_045512.2",
  "conclusionCode": [
    {
      "text": "SARS-CoV-2 identified by whole-genome sequencing",
      "coding": [
        {
          "system": "http://snomed.info/sct",
          "code": "840533007",
          "display": "Severe acute respiratory syndrome coronavirus 2"
        }
      ]
    },
    {
      "text": "Pango lineage B.1.258"
    },
    {
      "text": "WHO variant designation: Not designated"
    }
  ]
}
```

```json
{
  "code": {
    "coding": [
      {
        "system": "http://loinc.org",
        "code": "96895-8",
        "display": "SARS-CoV-2 (COVID-19) lineage [Identifier] in Specimen by Molecular genetics method"
      }
    ]
  },
  "valueCodeableConcept": {
    "coding": [
      {
        "system": "https://cov-lineages.org/lineage_list.html",
        "code": "B.1.258",
        "display": "Pango lineage B.1.258"
      }
    ],
    "text": "B.1.258"
  },
  "method": {
    "text": "nextclade 3.18.1 (database 2026-06-16--14-30-45Z)"
  },
  "component": [
    {
      "code": {
        "text": "WHO variant classification"
      },
      "valueCodeableConcept": {
        "coding": [
          {
            "system": "https://www.who.int/activities/tracking-SARS-CoV-2-variants",
            "code": "not_designated",
            "display": "Not a WHO-designated variant"
          }
        ],
        "text": "not_designated"
      }
    }
  ]
}
```

```json
{
  "component": [
    {
      "code": {
        "coding": [
          {
            "system": "http://loinc.org",
            "code": "81290-9",
            "display": "Genomic DNA change (gHGVS)"
          }
        ]
      },
      "valueCodeableConcept": {
        "coding": [
          {
            "system": "http://varnomen.hgvs.org",
            "code": "NC_045512.2:g.22879C>A",
            "display": "NC_045512.2:g.22879C>A"
          }
        ]
      }
    },
    {
      "code": {
        "coding": [
          {
            "system": "http://loinc.org",
            "code": "48018-6",
            "display": "Gene studied [ID]"
          }
        ]
      },
      "valueCodeableConcept": {
        "coding": [
          {
            "system": "https://www.ncbi.nlm.nih.gov/gene",
            "code": "43740568",
            "display": "Surface glycoprotein (spike)"
          }
        ],
        "text": "S"
      }
    },
    {
      "code": {
        "coding": [
          {
            "system": "http://loinc.org",
            "code": "48005-3",
            "display": "Amino acid change (pHGVS)"
          }
        ]
      },
      "valueCodeableConcept": {
        "coding": [
          {
            "system": "http://varnomen.hgvs.org",
            "code": "QHD43416.1:p.Asn439Lys",
            "display": "QHD43416.1:p.Asn439Lys"
          }
        ],
        "text": "QHD43416.1:p.Asn439Lys"
      }
    }
  ]
}
```

## Links

- GitHub repository: [SARS-CoV-2-to-fhir-full](https://github.com/oucru-id/SARS-CoV-2-to-fhir-full)

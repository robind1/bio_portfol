---
layout: splash
title: "Bioinformatics Portfolio"
permalink: /
author_profile: false
header:
  overlay_filter: 0.25
  caption: "Pathogen genomics workflows with interoperable FHIR reporting"
  actions:
    - label: "View Projects"
      url: "/projects/"
    - label: "About Me"
      url: "/about/"
feature_row_pathogens:
  - image_path: "/assets/images/FHIR_logo.png"
    alt: "Platform-agnostic TB to FHIR pipeline"
    title: "Platform-agnostic TB to FHIR pipeline"
    excerpt: "Illumina, Nanopore, and VCF workflow for TB genomic analysis."
    url: "/projects/2026-04-25-platform-agnostic-tb-genomic-analysis-to-fhir/"
    btn_label: "Details"
    btn_class: "btn--primary"
  - image_path: "/assets/images/Image-Deeplex6.png"
    alt: "Deeplex Myc-TB to FHIR pipeline"
    title: "Deeplex Myc-TB to FHIR pipeline"
    excerpt: "tNGS from Deeplex Excel reports into standardized FHIR Genomics reporting."
    url: "/projects/2026-04-25-deeplex-myc-tb-to-fhir-pipeline/"
    btn_label: "Details"
    btn_class: "btn--primary"
  - image_path: "/assets/images/FHIR_logo.png"
    alt: "Klebsiella pneumoniae genomic to FHIR pipeline"
    title: "KP Genomic to FHIR pipeline"
    excerpt: "Kleborate and cgMLST analysis into FHIR Genomics reporting."
    url: "/projects/2026-04-25-kp-genomic-to-fhir-mutation-analysis-pipeline/"
    btn_label: "Details"
    btn_class: "btn--primary"
  - image_path: "/assets/images/FHIR_logo.png"
    alt: "Dengue genomic to FHIR analysis pipeline"
    title: "Dengue Genomic to FHIR Analysis"
    excerpt: "Pan-serotype dengue workflow with serotype, genotype, lineage, and mutation reporting in FHIR Genomics."
    url: "/projects/2026-04-25-dengue-genomic-to-fhir-analysis-pipeline/"
    btn_label: "Details"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}

## Featured Works

{% include feature_row id="feature_row_pathogens" %}

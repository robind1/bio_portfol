---
title: "PhIP-Seq Analysis Pipeline for ONT"
excerpt: "Nextflow pipeline for Phage Immunoprecipitation Sequencing (PhIP-Seq) analysis using Oxford Nanopore."
header:
  teaser: "/assets/images/phip-seq1.png"
tags:
  - PhIP-Seq
  - Nextflow
  - Immunology
toc: true
---

## Overview

This project provides an end-to-end Nextflow pipeline for processing Phage Immunoprecipitation Sequencing (PhIP-Seq) data generated from Oxford Nanopore Technologies (ONT) sequencing. PhIP-Seq is a highly multiplexed method for antibody profiling that enables the characterization of an individual's immunological characteristics, including viral exposures and autoantibody repertoires.

The pipeline automates the entire analytical workflow, from raw sequence alignment against custom peptide reference libraries to robust statistical evaluation and functional annotation utilizing Immune Epitope Database ([IEDB](https://www.iedb.org/)). To maximize the utility of the results, the pipeline generates an interactive Streamlit dashboard for multidimensional data exploration. The pipeline take reference from Matsen Group [pipeline](https://github.com/matsengrp/phip-flow) and adapt it for ONT data.

![PhIP-seq workflow diagram]({{ '/assets/images/phip-seq2.png' | relative_url }})

## Dataset and Inputs

- Oxford Nanopore (ONT) long-read sequencing data
- Sample metadata table
- Peptide reference library table

## Methods and Workflow

- **Alignment:** Align ONT long reads against custom peptide reference libraries.
- **Statistical Analysis:** Calculates enrichment statistics and false discovery rates to identify antibody-peptide bindings.
- **Virus Scoring:** Computes aggregated, virus-level scores from individual enriched peptide hits.
- **Annotation (IEDB):** Integrates the Immune Epitope Database (IEDB) to provide validated immunological context to the enriched peptides.
- **Visualization:** Produces wide data matrices and a prototype 3D Streamlit dashboard for interactive serological profiling.

![Streamlit screenshoot]({{ '/assets/images/streamlit.png' | relative_url }})

## Why This Matters Clinically

High-throughput serological profiling with PhIP-Seq offers an unprecedented analysis into the immune system. This bridges the gap between immune data and genomics. Furthermore, the interactive dashboard enables researchers and clinicians to rapidly explore and interpret highly multidimensional serology data.

![PhIP-seq general workflow diagram]({{ '/assets/images/General-workflow-of-PhIP-Seq.jpg' | relative_url }})

Tiu, C.K.; Zhu, F.; Wang,L.-F.; de Alwis, R. PhageImmunoPrecipitation Sequencing(PhIP-Seq): The Promise of HighThroughput Serology. Pathogens 2022,11, 568.

## Links

- GitHub repository: [phip-seq-ont](https://github.com/oucru-id/phip-seq-ont)
- Technical documentation: [phipseq-pipeline-docs](https://phipseq-pipeline-docs.readthedocs.io/en/latest/index.html)
- Interactive Dashboard Prototype: [PhIP-Seq 3D Viz](https://phipseq3dvizprototype-test.streamlit.app/)

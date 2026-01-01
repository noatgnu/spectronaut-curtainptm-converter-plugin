# Spectronaut to CurtainPTM Converter


## Installation

**[⬇️ Click here to install in Cauldron](http://localhost:50060/install?repo=https%3A%2F%2Fgithub.com%2Fnoatgnu%2Fspectronaut-curtainptm-converter-plugin)** _(requires Cauldron to be running)_

> **Repository**: `https://github.com/noatgnu/spectronaut-curtainptm-converter-plugin`

**Manual installation:**

1. Open Cauldron
2. Go to **Plugins** → **Install from Repository**
3. Paste: `https://github.com/noatgnu/spectronaut-curtainptm-converter-plugin`
4. Click **Install**

**ID**: `spectronaut-curtainptm-converter`  
**Version**: 1.0.0  
**Category**: utilities  
**Author**: CauldronGO Team

## Description

Convert Spectronaut PTM output to CurtainPTM upload format. Supports multiple processing modes for different data formats - basic PTM processing, PTM group processing with site probabilities, and ProForma sequence processing with position validation.

## Runtime

- **Environments**: `python`

- **Entrypoint**: `convert.py`

## Inputs

| Name | Label | Type | Required | Default | Visibility |
|------|-------|------|----------|---------|------------|
| `input_file` | Spectronaut Output File | file | Yes | - | Always visible |
| `index_col` | Index Column Name | text | Yes | PTM_collapse_key | Always visible |
| `peptide_col` | Peptide Column Name | text | Yes | PEP.StrippedSequence | Always visible |
| `uniprot_id_col` | UniProt ID Column Name | text | No | UniprotID | Always visible |
| `processing_mode` | Processing Mode | select (1, 2, 3) | Yes | 1 | Always visible |
| `modification_type` | Modification Type (Modes 2 & 3) | text | No | Phospho (STY) | Visible when `processing_mode` is one of: `2`, `3` |
| `fasta_file` | FASTA File (Optional) | file | No | - | Always visible |
| `uniprot_columns` | UniProt Data Columns | text | No | accession,id,sequence,protein_name | Always visible |
| `output_filename` | Output Filename | text | No | curtainptm_input.txt | Always visible |
| `sequence_window_size` | Sequence Window Size | number (min: 1, max: 101, step: 2) | No | 21 | Always visible |

### Input Details

#### Spectronaut Output File (`input_file`)

Spectronaut PTM output file containing differential analysis data


#### Index Column Name (`index_col`)

Column name containing position information in the format *_*X123_* where X is the residue and 123 is the position

- **Placeholder**: `PTM_collapse_key`

#### Peptide Column Name (`peptide_col`)

Column name containing peptide sequences (format depends on processing mode)

- **Placeholder**: `PEP.StrippedSequence`

#### UniProt ID Column Name (`uniprot_id_col`)

Column name containing UniProt accession IDs

- **Placeholder**: `UniprotID`

#### Processing Mode (`processing_mode`)

Processing mode selection:
Mode 1: Basic PTM processing with position extraction from index column
Mode 2: PTM group processing with site probability calculations
Mode 3: ProForma sequence processing with enhanced position validation


- **Options**: `1`, `2`, `3`

#### Modification Type (Modes 2 & 3) (`modification_type`)

Type of modification to process (e.g., Phospho (STY), Acetyl (K), Methyl (K)). Only used in modes 2 and 3.

- **Placeholder**: `Phospho (STY)`

#### FASTA File (Optional) (`fasta_file`)

Protein sequence FASTA file. If not provided, sequences will be fetched from UniProt automatically.


#### UniProt Data Columns (`uniprot_columns`)

Comma-separated list of UniProt columns to retrieve when fetching sequences online

- **Placeholder**: `accession,id,sequence,protein_name`

#### Output Filename (`output_filename`)

Name of the output file (will be created in the output folder)

- **Placeholder**: `curtainptm_input.txt`

#### Sequence Window Size (`sequence_window_size`)

Size of the sequence window around modification sites (must be odd number). Default is 21 (10 residues before + modification + 10 after).


## Outputs

| Name | File | Type | Format | Description |
|------|------|------|--------|-------------|
| `converted_file` | `*.txt` | data | tsv | Converted file in CurtainPTM upload format with PTM positions, sequence windows, and protein annotations |

## Requirements

- **Python Version**: >=3.10

### Python Dependencies (External File)

Dependencies are defined in: `requirements.txt`

- `curtainutils>=0.2.0`
- `pandas>=2.0.0`
- `click>=8.0.0`

> **Note**: When you create a custom environment for this plugin, these dependencies will be automatically installed.

## Usage

### Via UI

1. Navigate to **utilities** → **Spectronaut to CurtainPTM Converter**
2. Fill in the required inputs
3. Click **Run Analysis**

### Via Plugin System

```typescript
const jobId = await pluginService.executePlugin('spectronaut-curtainptm-converter', {
  // Add parameters here
});
```

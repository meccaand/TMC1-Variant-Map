# Mapping hearing-loss variants onto TMC1 with Claude Science

> **Status:** draft. Sections marked TODO get filled in after the Claude Science sessions.

A worked example of an end-to-end [Claude Science](https://claude.com/product/claude-science) workflow,
using only public data: pull clinical variants for **TMC1**, the protein thought to form the pore of the hair-cell
mechanotransduction channel, map them onto a predicted structure, and test whether pathogenic missense
variants cluster in the putative pore region more than benign ones do.

I spent my PhD recording from cochlear hair cells, so I could check every step Claude took against
what I already know about the biology.

## Question
Do pathogenic/likely-pathogenic TMC1 missense variants sit closer to the putative pore-lining helices than
benign/likely-benign variants?

## Workflow
| Step | What Claude Science did | Connectors / skills | Output |
|---|---|---|---|
| 1. Plan | TODO | – | plan |
| 2. Data pull | TODO | UniProt, ClinVar, gnomAD | `data/tmc1_variants.csv` |
| 3. Structure | TODO | AlphaFold, PDB | 3D view, pLDDT plot |
| 4. Analysis | TODO | – | distance-to-pore figure + permutation test |
| 5. Review | TODO | reviewer | findings + fixes |

Exact prompts: [`session-plan.md`](session-plan.md).

## Results
TODO: headline figure + 2–3 sentences.

## What the reviewer caught
TODO: what the background reviewer flagged and what I changed.

## Where I had to step in
TODO: wrong assumptions, domain corrections, dead ends. This section is the most useful part for readers.

- *(draft, run 1)* The first pass said no benign missense variant sits in a TM helix (25/0 vs 23/6, Fisher p = 0.025).
  Two of the 6 "benign missense" variants are not missense on the MANE transcript: `p.A755A` is synonymous and
  `p.*761*` is stop-retained. Their `missense_variant` label likely came from another transcript.
  With the 4 real benign missense variants, the test gives 25/0 vs 23/4, p = 0.11, so the "real signal" does not hold.
  After I pointed this out, Claude Science reclassified consequences on MANE and reran the test, getting p = 0.112 (fixed).
- *(draft, run 2)* The 3D pore-distance figure reports "35.6 Å closer, permutation p = 0.008". Three problems:
  - Only 3 benign missense variants pass pLDDT ≥ 70.
  - 35.6 Å is a difference of **means**, driven by p.K65E at 98.7 Å. K65E is in the N-terminal domain, where pLDDT is high
    locally but AlphaFold's placement relative to the TM domain is uncertain (that needs PAE, not pLDDT). The plotted bars are medians (~17 Å apart).
  - The pore was defined from TM4 + TM6 (+1 TM7 residue), not TM4–TM7 as asked.
- *(draft)* What it got right: TM4 is a broken helix at residues 418–419 in both the AlphaFold model and the C. elegans cryo-EM structure (7USW).
  M418 is the Beethoven-equivalent residue. UniProt's TM1 and TM6 run ~12 aa past the helices in the structures.

## Limitations
- ClinVar has ascertainment bias, and benign variants are under-sampled in some domains.
- Predicted structures are not experimental structures; check pLDDT before reading anything into a region.
- Research demo only. Not for clinical or diagnostic use.

## Reproduce
Each notebook in [`notebooks/`](notebooks/) was exported from Claude Science
(Files → artifact → Provenance → Code → Download notebook), with its execution log and environment.

## Data sources & licenses
UniProt (CC BY 4.0), AlphaFold DB (CC BY 4.0), ClinVar (public domain), gnomAD (ODbL), RCSB PDB (CC0).

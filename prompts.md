# Prompts used in Claude Science

All prompts were sent in one Claude Science session. The original plan comes first, then the follow-up prompts
sent after I reviewed each run.

# Original plan

## 1. Plan first
> I want to test whether pathogenic missense variants in human TMC1 cluster near the putative ion-conducting pore
> more than benign variants do. Use UniProt, ClinVar and gnomAD for variants, and the AlphaFold model and any relevant
> experimental TMC structures from PDB. Propose an analysis plan with the specific data sources, how you'll define
> "pore-lining" residues, and the statistical test. Don't run anything yet.

## 2. Data pull
> Run the data steps of the plan. Save one tidy table, `tmc1_variants.csv`, with protein position, ref/alt amino acid,
> ClinVar classification and review status, gnomAD allele frequency, and source accession. Report counts per class
> and how you handled conflicting classifications.

## 3. Structure + mapping
> Load the AlphaFold model for TMC1, show it in 3D with variants colored by class, and save a per-residue pLDDT plot
> with variant positions marked. Compute each variant's minimum distance to the pore-lining residues you defined.

## 4. Test
> Compare distance-to-pore between pathogenic/likely-pathogenic and benign/likely-benign variants with a permutation
> test, restricted to residues with pLDDT ≥ 70. Save a publication-quality figure and a one-paragraph result, with the
> caveats stated.

## 5. Review
> Run the reviewer on this session and summarize its findings. Fix anything it flags and re-save the affected artifacts.

---

# Follow-up prompts (sent after reviewing each run)

## After run 1: wrong-transcript consequences
> Two of the 6 B/LB "missense" variants are p.A755A and p.*761*, which aren't missense on ENST00000297784.10. Audit every row where `consequence` disagrees with the p. change you derived, reclassify consequences on the MANE transcript, and re-run the TM vs non-TM test. Also, regions under 10 aa make the per-100-aa density unstable. Show raw counts next to the densities and drop the "real signal" wording if the result changes.

> Now that ClinVar access is on, add per-variant conditions and split the P/LP missense into DFNA36 (dominant) and DFNB7/B11 (recessive) assertions. Does the TM enrichment hold within the recessive set alone?

> Load the AlphaFold model for Q8TDI8 and the C. elegans TMC-1 cryo-EM structure from PDB. Check that UniProt's 10-helix topology agrees with the helices in the structure, and say where they differ. Define pore-lining residues by homology to TMEM16/OSCA (TM4–TM7). Then compute each variant's minimum distance to the pore, keeping only residues with pLDDT ≥ 70. Compare P/LP vs B/LB missense with a permutation test, show it in 3D, and save a figure.

## After run 2: the pore-distance figure
> Fix the pore-distance analysis:
> (a) The figure's "35.6 Å closer" is a difference of means, but the bars show medians, and the gap is driven by p.K65E at 98.7 Å. Use medians throughout.
> (b) For residues outside the TM domain, filter on PAE to the pore residues, not just pLDDT.
> (c) With 3 benign variants, make the main comparison P/LP vs all non-P/LP missense (VUS + conflicting + benign), and show P/LP vs benign as secondary.
> (d) Define pore-lining residues across TM4–TM7 as I originally asked, and justify which TM5/TM7 residues you include.
> (e) Rerun the step 2 TM enrichment using the cryo-EM helix boundaries instead of UniProt's.
> Re-save the figure and give the results in a table.

## Export
> Save a single Jupyter notebook artifact, `tmc1_variant_map_analysis.ipynb`, that reproduces `tmc1_clinvar_variant_map.csv`, `tmc1_tm_enrichment.csv`, `tmc1_pore_distance_sensitivity.csv` and `tmc1_pore_distance.png` end to end from the public sources. Include markdown cells explaining each step, and note the environment and package versions at the top.

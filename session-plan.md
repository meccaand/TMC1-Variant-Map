# Claude Science session plan

Run these in one Claude Science project named **TMC1 variant map**. Paste each prompt, approve the permission
cards, and save the outputs. Notes go in the "Log" lines and feed the README.

## 0. Setup (once)
- Install from https://claude.com/product/claude-science (Mac, Apple silicon), then sign in.
- Setup wizard: keep the Featured connectors on, memory on.
- Optional warm-up: open the **Example project** to see how permission cards and artifacts work.

## 1. Plan first
> I want to test whether pathogenic missense variants in human TMC1 cluster near the putative ion-conducting pore
> more than benign variants do. Use UniProt, ClinVar and gnomAD for variants, and the AlphaFold model and any relevant
> experimental TMC structures from PDB. Propose an analysis plan with the specific data sources, how you'll define
> "pore-lining" residues, and the statistical test. Don't run anything yet.

Log: did the plan define the pore sensibly (TM4–TM7 region by homology to TMEM16/OSCA)? What did I change?

## 2. Data pull
> Run the data steps of the plan. Save one tidy table, `tmc1_variants.csv`, with protein position, ref/alt amino acid,
> ClinVar classification and review status, gnomAD allele frequency, and source accession. Report counts per class
> and how you handled conflicting classifications.

Log: counts and odd cases.

## 3. Structure + mapping
> Load the AlphaFold model for TMC1, show it in 3D with variants colored by class, and save a per-residue pLDDT plot
> with variant positions marked. Compute each variant's minimum distance to the pore-lining residues you defined.

Log: screenshot of the 3D view → `figures/`.

## 4. Test
> Compare distance-to-pore between pathogenic/likely-pathogenic and benign/likely-benign variants with a permutation
> test, restricted to residues with pLDDT ≥ 70. Save a publication-quality figure and a one-paragraph result, with the
> caveats stated.

## 5. Review
> Run the reviewer on this session and summarize its findings. Fix anything it flags and re-save the affected artifacts.

Log: what it caught → README "What the reviewer caught".

## 6. Export
For the final table and each figure: **Files → artifact menu → Provenance → Code → Download notebook**.
Unzip into `notebooks/`, copy figures into `figures/` and the table into `data/`. Also save **Export Metadata** for
the headline figure.

# Intended Use Statement
## Four-plex real-time PCR panel, gastrointestinal bacterial pathogens

**Status:** DESIGN ONLY — in silico, not wet-lab verified
**Author:** Rajendra Songara
**Date:** 13 October 2026
**Version:** 1.0

---

## 1. Intended purpose

A single-tube, qualitative, four-plex real-time PCR assay for the detection and
differentiation of *Salmonella* spp., *Campylobacter jejuni* and *Shigella* spp. in human
stool specimens from patients presenting with acute gastroenteritis. Intended to support
diagnosis, infection-control decisions and antimicrobial prescribing. An internal
amplification control (IAC) is included in every reaction.

## 2. Target population and setting

Stool specimens from patients of any age presenting with acute diarrhoeal illness.
Processed in a hospital clinical microbiology laboratory.

**Operator model:** a single operator performs the complete workflow — extraction, reaction
setup, loading, and interpretation. There is no second-person check at any stage.

**Batch size:** up to 30 specimens per run, plus a no-template control and a positive
control, in a 96-well format. The limit is set by what one operator can pipette before
master mix performance degrades, not by plate capacity. A 96-well plate could in principle
hold 94 specimens; a single operator setting up manually cannot do so reliably, and
designing for the plate rather than the person is how an assay that validates cleanly fails
in routine use.

Consequences for the design:

- The protocol must be difficult to perform incorrectly; steps that depend on operator
  judgement are a liability.
- Result interpretation is unaided, so the reporting logic in section 6 must be
  unambiguous and require no calculation.
- Reagents must tolerate the full setup time for a 30-specimen batch at bench temperature.

Single-operator use today does not remove the need for a multi-operator reproducibility
study in validation. Operators change.

## 3. Specimen type and preparation

Stool collected into a sterile container, avoiding contamination with urine or toilet
water, followed by commercial nucleic acid extraction.

A sterile container prevents the *addition* of organisms. It does nothing about the
organisms already present, and stool carries a large and variable background flora. That
flora is the basis of the exclusivity panel in section 9 (DI-08).

Extraction is outside the scope of this design and is assumed to deliver eluate of adequate
quality and volume.

**Expected inhibitors:** bile salts, complex polysaccharides, haem, urate. Stool is among
the most PCR-inhibitory specimen types, which is why the IAC is not optional.

**Dilution is a conditional remediation step, not routine preparation.** Diluting the
extract reduces inhibitor concentration and target concentration by the same factor, so a
specimen near the limit of detection may dilute from detected to not detected. Dilution is
therefore applied only to samples returning an invalid result (section 6), never to every
sample. The IAC is what identifies which samples need it.

All three targets are bacterial, so the template is DNA and no reverse-transcription step is
required.

## 4. Platform

**Applied Biosystems QuantStudio 5 Real-Time PCR System, 96-well (catalogue A28574).**

Six decoupled excitation and emission filter channels, 21 filter combinations, white LED
excitation source.

Source: Thermo Fisher Scientific product page for catalogue A28574, read 13 October 2026.

Three pathogen targets plus one IAC require four reporter channels. One further channel is
reserved for a passive reference dye (section 5), leaving one spare for a future fifth
target.

## 5. Assay format

Single-tube, four-plex hydrolysis-probe real-time PCR. Qualitative. Three pathogen targets
and one non-competitive internal amplification control, each reporting into a separate
optical channel.

| Target | Reporter | Rationale |
|---|---|---|
| *Shigella* spp. | FAM | Most sensitive reporter, assigned to the target expected at lowest copy number (see below) |
| *Salmonella* spp. | VIC / HEX | Second channel; typically higher organism burden in acute disease |
| *Campylobacter jejuni* | Cy5 | Third channel, well separated spectrally from FAM |
| Internal amplification control | Texas Red | Control signal; deliberately not given a high-sensitivity channel |
| *(reserved)* | ROX | Passive reference for well-to-well normalisation; not available as a reporter |

**Channel assignment rationale.** The strongest reporter goes to the target expected at the
lowest copy number, not to the most clinically important one. *Shigella* has a notably low
infectious dose, so a genuine infection may present with relatively few organisms in the
specimen, and that assay has the least signal to work with. *Salmonella* in symptomatic
acute disease is generally shed at higher burden and can tolerate a weaker channel.

This rationale rests on infectious dose rather than on measured shedding during acute
illness, and those are not the same thing. The assignment should be revisited if the Session
2 literature reading contradicts it, and the decision recorded either way.

**Dye set caveat.** The reporter-to-channel mapping above must be confirmed against the
QuantStudio 5 calibrated dye list before any oligo is ordered. A probe labelled with a dye
the instrument is not calibrated for is invisible, and this is the single easiest way to
waste a modified-oligo order.

## 6. Result interpretation

| *Shigella* (FAM) | *Salmonella* (VIC) | *C. jejuni* (Cy5) | IAC | Report |
|---|---|---|---|---|
| + | − | − | any | **Detected:** *Shigella* spp. / EIEC |
| − | + | − | any | **Detected:** *Salmonella* spp. |
| − | − | + | any | **Detected:** *Campylobacter jejuni* |
| Any two or three positive | | | any | **Detected:** report every organism identified |
| − | − | − | + | **Not detected** — valid negative |
| − | − | − | − | **INVALID** — repeat, or re-extract and dilute |

**Why a positive target validates the reaction regardless of the IAC.** If any pathogen
channel amplified, the reaction demonstrably worked: enzyme active, no total inhibition,
cycling correct. The IAC has nothing left to prove. Treating this as invalid means repeating
good samples indefinitely.

A failed IAC alongside a strongly positive target is in fact the expected behaviour, not a
fault. The IAC template is present at low, fixed concentration; an abundant pathogen target
outcompetes it for polymerase and nucleotides. An IAC that never fails in high-positive
samples is a sign it has been added at too high a concentration, which would mask a
low-positive specimen.

**Co-detection is reported in full.** More than one enteric pathogen can be present in a
single specimen and this is not rare. Every organism detected is named. Suppressing a second
positive because it was unexpected would discard a real finding.

**The row that justifies the whole design** is the last one. Without the IAC, an inhibited
reaction and a true negative are indistinguishable — both show three flat curves. One of
them is a missed infection.

## 7. Regulatory status

**Research use only. Not for use in diagnostic procedures.**

To become an in vitro diagnostic device, this assay would require full analytical validation
(the studies specified in `05_validation_protocol.md`), clinical performance evaluation
against a reference method, a quality management system to ISO 13485, technical
documentation, and conformity assessment appropriate to its risk class.

## 8. Explicit exclusions

- Does not detect viral or parasitic causes of gastroenteritis. A negative result does not
  exclude infectious diarrhoea.
- **Does not distinguish *Shigella* spp. from enteroinvasive *Escherichia coli*.** The
  standard molecular target used for *Shigella* detection is also carried by EIEC, so a
  positive result is reported as *Shigella*/EIEC. This is a known property of the target
  shared by commercial panels, not a defect introduced by this design. The specific gene and
  a supporting citation are recorded in `02_target_regions.md`.
- **Does not detect *Campylobacter* species other than *C. jejuni*.** This design is
  species-level by choice. *C. coli* causes a clinically meaningful share of human
  campylobacteriosis and will be reported as not detected. A genus-level design was
  considered and rejected to keep the exclusivity requirement tractable within the scope of
  this project; the trade-off is recorded in `02_target_regions.md`.
- Does not distinguish viable from non-viable organisms. Nucleic acid persists after the
  organism is dead, so a positive result does not prove active infection.
- Does not provide antimicrobial susceptibility information.
- Does not quantify organism burden. Quantification is outside the intended purpose and no
  limit of quantification is established.
- Does not detect toxin genes or virulence determinants, and therefore cannot distinguish
  pathogenic from non-pathogenic carriage where that distinction depends on them.
- Performance on specimen types other than stool has not been assessed.
- Performance in patients already receiving antimicrobial therapy has not been assessed.
- No performance characteristic in this document has been established experimentally. All
  statements describe design intent, not measured performance.

## 9. Design inputs

| ID | Requirement |
|---|---|
| DI-01 | All four assays must function in a single tube under one cycling protocol. |
| DI-02 | Primer Tm 58–62 °C; all primers in the panel within 2 °C of each other. |
| DI-03 | Each probe Tm 5–10 °C above its own primer pair. |
| DI-04 | Amplicon length 70–150 bp for all targets. |
| DI-05 | No oligo pair in the panel may form a stable cross-dimer at the annealing temperature. |
| DI-06 | Each reporter dye must be within the QuantStudio 5 calibrated dye set, with minimal spectral overlap, and must not use the channel reserved for the passive reference. |
| DI-07 | Each pathogen assay must detect all sequences in the inclusivity set examined. |
| DI-08 | No pathogen assay may amplify any organism in the exclusivity panel in silico, including the other two panel targets and common stool flora. |
| DI-09 | The IAC must amplify in every reaction containing a valid extract and no pathogen target, within a defined Cq window. |

Each design input is verified in Session 4 and the result recorded in
`04_compatibility_report.csv`.

---

*No gene targets are named in this document. Target selection follows in
`02_target_regions.md`, after this statement is complete.*

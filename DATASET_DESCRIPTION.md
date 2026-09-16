# Reference Standard Attribution Dataset: Calibration Records With The Traceability Links Removed

## Overview

This is an original, fully synthetic dataset of 1,500 calibration laboratories. Each laboratory keeps 4 to 6 reference standards and 8 to 18 working instruments, observed over 730 days. Every instrument is periodically re-calibrated against exactly one standard, which pins its readings to that standard's value on the day of the calibration; between calibrations it drifts on its own. The dataset contains no real operational records, no personal data and no third-party material; every row was produced by the generation procedure described below.

The labels are the traceability links: which standard each instrument is calibrated against. They are the one thing the released records do not contain.

## Release At A Glance

- Raw files: 12
- Laboratories (cases): 1,500, each with its own standards and instruments, so every laboratory is an independent unit
- Reference standards: 7,444, 4 to 6 per laboratory
- Instruments: 18,513, 8 to 18 per laboratory, 12.3 on average
- Capacity: each standard serves exactly 2 or 3 instruments, the same number throughout a laboratory, so every laboratory is fully subscribed
- Certificates: 33,601, 3 to 6 per standard (4.5 on average), each the standard's value on one day with a small recording error
- Calibration events: 64,619, 2 to 5 per instrument (3.4 on average)
- Check readings: 961,908, about 52 per instrument, one every 7 to 21 days
- Observation window: days 0 to 729
- Prepared split: 1,200 training laboratories, 300 test laboratories (every fifth laboratory in hashed-id order)
- Data origin: creator-generated synthetic data

## Raw File Structure

The uploaded ZIP is flat and contains exactly these twelve files at its root:

- `labs.csv`: one record per laboratory: `case_id`, `n_instruments`, `n_standards`, `capacity`.
- `standards.csv`: one record per reference standard: `case_id`, `standard_id`.
- `certificates.csv`: one record per published certificate: `case_id`, `standard_id`, `day`, `value`.
- `instruments.csv`: one record per instrument: `case_id`, `instrument_id`.
- `readings.csv`: one record per check reading: `case_id`, `instrument_id`, `day`, `value`.
- `calibrations.csv`: one record per calibration event: `case_id`, `instrument_id`, `day`.
- `assignment.csv`: one creator-side record per instrument: `case_id`, `instrument_id`, `standard_id`; used by `prepare.py` and never copied into public prepared data in full.
- `source_metadata.json`: provenance, scale, ranges and licence metadata.
- `LICENSE`: CC BY 4.0 notice and licence URL.
- `ATTRIBUTION.txt`: attribution text.
- `DATASET_CARD.md`: short scope and safety summary.
- `DATASET_DESCRIPTION.md`: this document.
- `PACKAGE_MANIFEST.sha256`: SHA-256 checksum of every other file in the package.

## How The Data Was Generated

Every draw and every identifier comes from HMAC-SHA256 keyed to a withheld 256-bit secret; the generator code and the secret are not released. Each laboratory is drawn independently.

1. **Setup.** Draw 4 to 6 standards and a capacity of 2 or 3, giving the laboratory exactly capacity times the number of standards instruments. Assign instruments to standards so that every standard is filled to capacity.
2. **Standards.** Each standard drifts as a random walk over 730 days with a step of 0.004, 0.008 or 0.015. It publishes a certificate on 3 to 6 randomly chosen days: its value on that day plus a recording error of about 0.004.
3. **Instruments.** Each instrument has its own drift walk with a step of 0.003, 0.006 or 0.012, its own measurement noise of 0.01, 0.02 or 0.04, and 2 to 5 calibration days.
4. **Calibration.** At each calibration day the instrument's offset is reset so that its level equals its standard's value on that day. The offset then holds until the next calibration while the instrument's own drift continues.
5. **Observation.** Publish the check readings, every 7 to 21 days, each the instrument's own drift plus its current offset plus measurement noise. The standards' daily paths, the instruments' own drift walks, the offsets and the assignment all stay hidden.

## Intended Use And Limitations

- Intended use: research and benchmarking of constrained assignment recovery and structure recovery from sparse, noisy measurement records.
- Out of scope: any claim about real laboratories, instruments or measurement standards. The laboratories, standards and instruments do not exist.
- No personal data, no real laboratory data and no third-party material is included.

## Licence

Creative Commons Attribution 4.0 International (CC BY 4.0), https://creativecommons.org/licenses/by/4.0/

# Kepler-51 b — Real TESS Transit Report

<p align="center">
  <img src="figures/kepler51b_tess_transit.png" alt="Phase-folded real TESS transit light curve of Kepler-51 b" width="760">
</p>

One real, public TESS SPOC light curve; one saved NASA Exoplanet Archive
ephemeris; one reproducible flat-versus-box statistical comparison.

**[Open the full report](https://biswajit1999.github.io/kepler-51b-exoplanet-report/)** — the live GitHub Pages version.

## Data sources

- **System parameters** — the saved `pscomppars` row from the [NASA Exoplanet Archive TAP service](https://exoplanetarchive.ipac.caltech.edu/TAP/sync?query=select+pl_name%2Chostname%2Cra%2Cdec%2Cpl_orbper%2Cpl_tranmid%2Cpl_trandur%2Cpl_rade%2Cpl_bmasse%2Cpl_eqt%2Cpl_orbsmax%2Csy_dist%2Csy_tmag%2Cst_teff%2Cst_rad%2Cst_mass%2Cdisc_year%2Cdiscoverymethod%2Cdisc_refname%2Cdisc_pubdate%2Cdisc_facility+from+pscomppars+where+pl_name%3D%27Kepler-51+b%27&format=csv).
- **Observed photometry** — unmodified MAST file `tess2019198215352-s0014-0000000027846348-0150-s_lc.fits`, TESS Sector 14, DOI [10.17909/t9-nmc8-f686](https://doi.org/10.17909/t9-nmc8-f686). This is a real SPOC reduced light curve, not simulated data.
- Exact URLs, IDs, retrieval date, and SHA-256 checksum are in [`data/SOURCE.md`](data/SOURCE.md).

## Reproduce the analysis

```bash
pip install -r requirements.txt
python scripts/analyze_transit.py
python scripts/analyze_multisector.py
pytest tests/ -v
```

The script keeps finite `QUALITY == 0` cadences, normalizes `PDCSAP_FLUX`,
applies one symmetric robust outlier rule, and examines ±2.5 published transit
durations around the fixed NASA ephemeris. It compares a weighted constant with
a two-level box whose depth is fitted. Timing and duration are not searched.

## What the numbers show

| Quantity | Result |
|---|---:|
| TESS sector | 14 |
| Cadences in comparison | 865 |
| Fitted box depth | 1667.3 ± 2417.9 ppm |
| Flat χ² / dof / p | 789.04 / 864 / 0.9673 |
| Box χ² / dof / p | 788.57 / 863 / 0.9663 |
| Improvement Δχ² / Δdof / p | 0.48 / 1 / 0.4905 |

This sector is statistically consistent with no box-shaped dip at the fixed archive ephemeris. This establishes only how these archived fluxes compare with this
pre-specified box model. It does not independently confirm the planet or identify
an atmosphere.

<!-- MULTISECTOR-UPGRADE-START -->
## Multi-sector robustness and correlated noise

The fixed archive ephemeris was fitted independently in 1 usable sector(s) (S14) from 2 committed files; 1 lacked adequate fixed-window sampling. Formal depth errors were inflated by sqrt(max(reduced chi-square, 1)) times the residual time-averaging beta factor (observed range 1.38-1.38). The robust inverse-variance depth is 1667.3 +/- 3333.8 ppm; a sector-to-sector Q test requires at least two usable sectors. These scaled errors address underestimated scatter and short-timescale correlation, but they are not a full Gaussian-process or physical limb-darkened transit fit.

<p align="center"><img src="figures/kepler51b_multisector_transits.png" alt="Independent sector transit fits for Kepler-51 b" width="760"></p>

<p align="center"><img src="figures/kepler51b_depth_consistency.png" alt="Sector depth consistency for Kepler-51 b" width="760"></p>

<p align="center"><img src="figures/kepler51b_noise_diagnostics.png" alt="Residual RMS time-averaging diagnostic for Kepler-51 b" width="760"></p>

The per-sector table is in [`figures/multisector_statistics.csv`](figures/multisector_statistics.csv). Regenerate all three figures with `python scripts/analyze_multisector.py`.
<!-- MULTISECTOR-UPGRADE-END -->

## System context

- Radius: 7.10 Earth radii
- Mass: 2.10 Earth masses
- Orbital period: 45.154000 days
- Transit duration: 5.797 hours
- Semi-major axis: 0.2514 AU
- Equilibrium temperature: 543 K
- Host: Kepler-51 · distance 783.83 pc
- Discovery: 2012 by Transit (Kepler)

## Limitations

- A box is not a limb-darkened physical transit model and does not retrieve radius ratio, impact parameter, or stellar density.
- Period, mid-transit epoch, and duration are fixed to one NASA composite row; their uncertainties and transit-timing variations are not propagated.
- SPOC PDCSAP processing, crowding corrections, stellar variability, time-correlated noise, and underestimated point uncertainties can make absolute χ² p-values poor even when the relative comparison is informative.
- The χ² improvement uses one additional fitted depth parameter and no timing search. It is not a blind detection false-alarm probability, and nearby-star contamination is not ruled out.
- Published global fits combine sectors, instruments, detrending choices, limb darkening, and astrophysical priors. This deliberately smaller test does not replace them.

## Repository structure

```text
README.md
index.html
requirements.txt
data/                       unmodified TESS FITS + NASA row + SOURCE.md
scripts/analyze_transit.py  real-data analysis and figure generation
figures/                    generated plot + summary_statistics.csv
tests/                      real-data regression tests
.github/workflows/tests.yml CI on every push and pull request
LICENSE                     MIT
```

## References

1. [Steffen et al. 2013](https://ui.adsabs.harvard.edu/abs/2013MNRAS.428.1077S/abstract) — discovery reference as listed by the NASA Exoplanet Archive.
2. Ricker, G. R. et al. (2015), *Transiting Exoplanet Survey Satellite (TESS)*, JATIS 1, 014003, [doi:10.1117/1.JATIS.1.1.014003](https://doi.org/10.1117/1.JATIS.1.1.014003).
3. TESS Team, *TESS Light Curves — All Sectors*, MAST, [doi:10.17909/t9-nmc8-f686](https://doi.org/10.17909/t9-nmc8-f686); Sector 14 used here.
4. [NASA Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/), `pscomppars` TAP row retrieved 2026-08-15.

## Author

Biswajit Jana — [Portfolio](https://biswajit1999.github.io/Biswajit_Jana.github.io/) · [GitHub](https://github.com/Biswajit1999) · [LinkedIn](https://www.linkedin.com/in/biswajit-jana-27011a151/) · [ORCID](https://orcid.org/0009-0002-2411-1891)

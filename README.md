# Radar vs. Drone Ground Truth Analysis

## Overview

This project uses Python to evaluate highway traffic radar measurements against drone-derived ground truth from the [DRaT dataset](https://zenodo.org/records/20171110).

The goal was to match vehicle detections between the two systems, calculate position and speed errors, and investigate factors affecting radar performance.

## Tools

Python, pandas, NumPy, SciPy, Matplotlib, Jupyter Notebook

## Methodology

- Explored and cleaned radar and drone trajectory data.
- Matched vehicle detections using the Hungarian assignment algorithm and a 40-foot distance threshold.
- Calculated position and speed errors for 266,928 accepted matches.
- Analyzed errors by sensor range, track quality, and traffic lane.

## Key Results

| Metric | Result |
|---|---:|
| Mean position error | 2.204 m |
| Median position error | 1.360 m |
| Position RMSE | 3.166 m |
| Mean absolute speed error | 1.214 m/s |
| Speed RMSE | 2.845 m/s |

Position error varied by sensor range, radar track quality, and traffic lane. The analysis also identified inconsistencies in vehicle matching across timestamps.

## Limitations

Matching was performed independently at each timestamp using a provisional 40-foot threshold. Some vehicle associations may be incorrect, so the reported errors apply to accepted matches rather than representing independently validated radar accuracy.

## Running the Project

Install the dependencies:

Download the DRaT dataset, place `radar.csv` and `drone_GT.csv` in the `data/` folder, and run `notebooks/radar_analysis.ipynb`.
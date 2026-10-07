# DRaT-Trajectory-Dataset

**DRaT** (**D**rone and **Ra**dar **T**rajectories) is a dual-modality dataset of naturalistic vehicle trajectories designed to systematically evaluate infrastructure-based radar sensing against drone-derived ground truth. The data were collected at a highway merging segment along the Ronald Reagan Memorial Highway in Fort Worth, Texas, covering a roughly 140 m (460 ft) eastbound section that transitions from four lanes to three.

## Overview

- **Duration:** ~80 minutes of synchronized collection, covering a 43-minute free-flow period (density ≈ 40.8 veh/km, space-mean speed ≈ 95.3 km/h) and a 37-minute congested period (density ≈ 122.9 veh/km, space-mean speed ≈ 28.7 km/h).
- **Drone (ground truth):** Hovered at 122 m (400 ft) AGL, capturing 4K video at 30 fps from a bird's-eye view. Trajectories were extracted using the open-source [geotrax](https://github.com/rfonod/geo-trax) framework, which combines a YOLO detector with the BoT-SORT multi-object tracker; frames were stabilized to a master reference and georeferenced to an orthophoto via ground control points.
- **Radar:** A commercial FMCW K-band radar (24.25–24.50 GHz) with a 110° horizontal field-of-view, mounted at ~5.5 m height with a 15° downward tilt. Outputs object-level positions, speeds, dimensions, and headings at 8 Hz, georeferenced using an RTK-corrected GPS reference and a refined azimuth angle.
- **Temporal alignment:** Drone trajectories (30 Hz) were linearly interpolated onto the radar's 8 Hz timestamps for one-to-one correspondence.

## Contents

| File | Description |
| --- | --- |
| `drone_GT.csv` | 5,278 drone-derived ground-truth trajectories — 392,707 detections aligned to radar timestamps. |
| `radar.csv` | 5,430 radar-derived trajectories — 282,712 detections at 8 Hz. |
| `LICENSE` | CC BY 4.0 license. |

Both files share a common core schema (vehicle ID, timestamp, global and local coordinates, speeds, and vehicle dimensions) plus a few file-specific columns. All timestamps are Unix epoch seconds at the radar's 8 Hz cadence; UTM coordinates are zone 14N; `local_x_ft` / `local_y_ft` are in the radar's sensor-centered frame, with `local_y_ft` approximately aligned to the road direction.

### `drone_GT.csv` columns

| Column | Unit | Description |
| --- | --- | --- |
| `track_id` | — | Unique drone-derived vehicle track ID |
| `timestamp` | s (Unix epoch) | Detection time, aligned to the radar's 8 Hz timestamps |
| `lat`, `lon` | deg (WGS84) | Vehicle position in geographic coordinates |
| `utm_x`, `utm_y` | m | Vehicle position in UTM zone 14N |
| `local_x_ft`, `local_y_ft` | ft | Vehicle position in the radar's sensor-centered local frame |
| `speed_mps` | m/s | Vehicle speed |
| `accel_mps2` | m/s² | Vehicle acceleration |
| `length_m`, `width_m` | m | Vehicle dimensions |
| `vehicle_class` | — | Vehicle class label from the drone detector |

### `radar.csv` columns

| Column | Unit | Description |
| --- | --- | --- |
| `track_id` | — | Radar object track ID |
| `timestamp` | s (Unix epoch) | Detection time at the radar's native 8 Hz cadence |
| `lat`, `lon` | deg (WGS84) | Vehicle position in geographic coordinates |
| `utm_x`, `utm_y` | m | Vehicle position in UTM zone 14N |
| `local_x_ft`, `local_y_ft` | ft | Vehicle position in the radar's sensor-centered local frame |
| `speed_mps` | m/s | Vehicle speed (magnitude of the radar's velocity vector) |
| `length_m`, `width_m` | m | Vehicle dimensions reported by the radar |
| `object_type` | — | Radar internal object-type code (not the same taxonomy as `vehicle_class`) |
| `track_quality` | — | Radar's per-detection track quality score |
| `lane_id` | — | Smoothed lane assignment |

## Intended Use

DRaT is intended to support reproducible research on infrastructure-based traffic monitoring, including:

- Multi-level evaluation of radar detection and tracking (individual vehicle, trajectory, macroscopic flow).
- Trajectory-level studies based on infrastructure sensing systems.

## Citation

If you use DRaT in your research, please cite:

```bibtex
@inproceedings{zhu2026drat,
  title     = {A Systematic Evaluation of Infrastructure-Based Radar System for Highway Traffic Monitoring},
  author    = {Zhu, Tianheng and Chang, Woei-chyi and Riaz, Alamss and Hasanzadeh, Sogand and Feng, Yiheng},
  booktitle = {IEEE International Conference on Intelligent Transportation Systems (ITSC)},
  year      = {2026}
}
```

## License

This dataset is released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). See [LICENSE](LICENSE) for details.

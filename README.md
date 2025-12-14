# Apomorphine Test Analysis - Cylindrical Arena

[![MATLAB](https://img.shields.io/badge/MATLAB-R2025b+-blue.svg)](https://www.mathworks.com/products/matlab.html)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Overview

This MATLAB script performs automated behavioral analysis of mouse or rat movement in a **cylindrical open-field arena** following **apomorphine injection**. It is specifically designed to quantify dopamine agonist-induced behaviors, including:

- Hyperlocomotion
- Stereotypical movements (high-speed repetitive behavior)
- Freezing
- Central zone occupancy (as a measure of anxiety-like behavior or thigmotaxis avoidance)
- Rotational behavior (circling/turning preference)

The script analyzes **post-injection tracking data only** (recording starts at injection time, t=0) over a user-defined duration (default: 30 minutes).

**Arena specifications**:  
- Diameter: 20 cm  
- Height: 40 cm  
- No peripheral zone defined (focus on central zone only)

**Author**: A. Babaei

## Features

- Automatic loading of tracking data from common formats (`Out_Post.mat`, `tracking_data.mat`, or user-selected `.mat`/`.csv`)
- Self-calibration of pixel-to-cm conversion based on detected arena bounds
- Zone analysis: central zone (default 10 cm diameter)
- Speed-based classification: freezing, slow movement, locomotion, stereotypy
- Time-binned analysis (5-minute bins)
- Rotational behavior quantification (total rotations, rotations/min, directional bias)
- Comprehensive visualization (trajectory, speed profile, zone occupancy, cumulative distance, histograms, pie chart)
- Export of results:
  - Summary statistics (CSV)
  - Detailed frame-by-frame data (CSV)
  - Time-binned metrics (CSV)
  - Figures (`.fig` and `.png`)
  - Full results (`.mat`)

## Requirements

- **MATLAB** (tested on R2020b and later)
- No external toolboxes required (uses base MATLAB only)
- Tracking data containing a `track` structure with position information

### Supported Tracking Data Formats

The script expects a variable named `track` (struct) with one of the following fields:
- `track.Centroid` – [N×2] or [2×N] matrix of x,y positions
- `track.Head`
- `track.Body_Centroid`

Additionally, optional fields:
- `track.FrameRate` – for accurate FPS
- `track.Time` – time vector for FPS calculation

Common sources:
- DeepLabCut
- EthoVision exports
- Custom tracking pipelines (saved as `.mat`)

## Usage

1. Place your post-injection tracking data file in the same folder as the script, preferably named:
   - `Out_Post.mat`
   - `tracking_data.mat`
   - Or any `.mat` file containing the `track` variable

2. Open the script in MATLAB and modify the **User Parameters** section if needed:

```matlab
%% ===== USER PARAMETERS =====
arena_diameter_cm = 20;           % Arena diameter in cm
central_zone_diameter_cm = 10;    % Central zone diameter (cm)
analysis_duration_min = 30;       % Analysis duration (minutes)
apomorphine_dose = 0.5;            % Dose in mg/kg
locomotion_threshold = 2.0;       % cm/s
stereotypy_threshold = 5.0;       % cm/s
freezing_threshold = 0.5;         % cm/s
```

3. Run the script.

4. The script will:
   - Load data automatically or prompt for file selection
   - Perform analysis
   - Display results in the command window
   - Generate and save a comprehensive figure
   - Export CSV and MAT files with timestamped filenames

## Output Files

Files are saved with prefix: `Apomorphine_PostInj_30min_Dose0.50_YYYY-MM-DD_HH-mm-ss`

- `_overview.png` and `_overview.fig` – Main results figure
- `_summary.csv` – Key behavioral metrics
- `_detailed_data.csv` – Frame-by-frame data (time, position, speed, zones, etc.)
- `_timebins.csv` – 5-minute bin averages
- `_results.mat` – Full workspace variables for further analysis

## Interpretation of Key Metrics

- **% Moving**: Time spent with speed > 2 cm/s (general locomotion)
- **% Stereotypy**: Time spent with speed > 5 cm/s (repetitive high-speed movements)
- **% Freezing**: Time spent with speed < 0.5 cm/s
- **% Central**: Time spent in inner 10 cm diameter zone (higher = reduced anxiety/thigmotaxis)
- **Rotations per minute**: Indicator of dopaminergic asymmetry or intense stereotypy

## Notes & Limitations

- Assumes recording starts at the moment of apomorphine injection (t=0).
- Arena must be approximately circular and fully visible in tracking.
- Calibration relies on animal exploring the entire arena for accurate scaling.
- Rotational analysis assumes movement around the arena center.

## Key Differences Between the Two Scripts

<img width="977" height="600" alt="image" src="https://github.com/user-attachments/assets/980e9c2b-930b-4887-9aa6-2ae0e972f79b" />

## License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

## Citation

If you use this script in your research, please cite it as:

> Babaei, A. (2025). Apomorphine Test Analysis - Cylindrical Arena. GitHub repository. (https://github.com/A-Babaei/Apomorphine-test-_-TrAQ-Analyzed-result)

Feel free to fork, modify, and contribute improvements!

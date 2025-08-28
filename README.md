Music Popularity Analysis: Spotify × YouTube
An exploratory data analysis of cross‑platform music popularity using Spotify stream counts and YouTube engagement metrics.

Table of Contents
Overview

Dataset

Project Structure

Environment and Setup

Quick Start

Analyses

Visualizations

Key Findings

Engagement Metric: LV_ratio

Troubleshooting

Roadmap

Contributing

License

Acknowledgments

Overview
This project examines how artists, tracks, and channels perform across Spotify and YouTube, focusing on total reach (Views/Streams) and engagement (Likes, Comments), and introduces a simple like‑rate metric, LV_ratio. The analysis is implemented in a Jupyter notebook with pandas, seaborn, and matplotlib.

Dataset
File: “Spotify Youtube Dataset (1).csv” containing track‑level records with Spotify and YouTube fields.

Size: ~20.7k rows; 24–28 columns depending on selected subsets.

Key columns used:

Identity: Artist, Track, Album, Album_type, Channel

YouTube: Views, Likes, Comments, Url_youtube, official_video, Licensed

Spotify: Stream, Url_spotify

Note: The notebook currently loads from an absolute path. Replace with a relative path (e.g., data/Spotify_Youtube_Dataset.csv) for portability.

Project Structure
text
.
├─ Analysis.ipynb        # Main analysis notebook
├─ data/                 # Place the CSV here (not checked in by default)
├─ figures/              # Optional: saved charts and exports
└─ README.md             # This file
Environment and Setup
Python 3.9+ recommended

Required packages: pandas, seaborn, matplotlib, jupyter

Install dependencies:

bash
pip install pandas seaborn matplotlib jupyter
Launch the notebook:

bash
jupyter notebook Analysis.ipynb
Quick Start
Place the dataset in data/ and update the load cell in the notebook:

python
import pandas as pd
data = pd.read_csv("data/Spotify_Youtube_Dataset.csv")
Run cells top to bottom:

Preview data and columns

Aggregations by Artist, Channel, Album_type

Compute LV_ratio

Create bar charts and summary tables

Analyses
Top Artists by YouTube Views

Group by Artist, sum Views, sort descending, display top 10.

Top Channels by YouTube Views

Group by Channel, sum Views, and rank.

Most‑Streamed Tracks on Spotify

Sort by Stream to surface global hits.

Album_type Summaries

Mean Likes, Views, Comments by Album_type to compare release strategies.

Example aggregation:

python
top_artists = (data.groupby('Artist', as_index=False)['Views']
                  .sum()
                  .sort_values('Views', ascending=False)
                  .head(10))
top_artists
Visualizations
Bar chart: Artists vs Total YouTube Views (Views on y‑axis, sorted descending)

Bar chart: Channels vs Total YouTube Views

Tables: Album_type means for Likes, Views, Comments

Example artist bar chart with readable y‑axis:

python
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker

ax = top_artists.plot.bar(x='Artist', y='Views', figsize=(10,6), legend=False)
ax.set_xlabel('Artist')
ax.set_ylabel('Views')
ax.yaxis.set_major_formatter(ticker.FuncFormatter(lambda v, pos: f'{v/1e9:.1f}B'))
plt.xticks(rotation=30, ha='right')
plt.tight_layout()
plt.show()
Key Findings
A small set of artists accounts for a large share of YouTube views (e.g., Ed Sheeran, CoComelon, Katy Perry, Luis Fonsi, etc.).

High‑capacity channels (e.g., VEVO or mega‑publishers) dominate total view counts.

Spotify’s top‑streamed tracks include “Blinding Lights” and “Shape of You,” showing overlap but not perfect alignment with YouTube leaders.

Engagement measured with LV_ratio highlights tracks with stronger like‑rate relative to views.

Engagement Metric: LV_ratio
Definition: LV_ratio = Likes / Views × 100 (percentage like‑rate per view).

Construction:

python
track_lv = data[['Likes','Views','Track']].dropna().copy()
track_lv['LV_ratio'] = (track_lv['Likes'] / track_lv['Views']) * 100
track_lv.sort_values('LV_ratio', ascending=False).head(10)
Interpretation: Higher LV_ratio suggests comparatively stronger audience appreciation per view; near‑zero values often reflect tiny denominators or low engagement.

Troubleshooting
SettingWithCopyWarning

Cause: Assigning to a slice can be ambiguous (view vs copy).

Fix: Create an explicit copy before assignment or use .loc on the original DataFrame.

Example:

python
track_lv = data[['Likes','Views','Track']].dropna().copy()
track_lv['LV_ratio'] = (track_lv['Likes'] / track_lv['Views']) * 100
Large axis numbers

Use a formatter to display billions on the y‑axis (see example in Visualizations).

Roadmap
Normalize metrics by release year to control for exposure time.

Segment results by language/region and by official_video.

Add interactive exploration (Plotly or Altair).

Replace absolute file paths with environment‑based configuration.

Contributing
Open an issue describing the change.

Fork the repo, create a feature branch, and submit a PR with a clear description and screenshots of updated figures.

License
Include a code license (e.g., MIT). Confirm dataset usage rights before redistributing the CSV.

Acknowledgments
Thanks to the dataset providers and the open‑source tools that power this analysis: pandas, seaborn, matplotlib, and Jupyter.
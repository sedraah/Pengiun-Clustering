# Palmer Penguins Clustering (DBSCAN & Mean Shift)

## Overview

This project applies two density-based clustering algorithms: DBSCAN and Mean Shift to a cleaned subset of the Palmer Archipelago penguin dataset. Using only two numeric features (`bill_depth_mm` and `flipper_length_mm`), both methods are evaluated using silhouette scores and compared in a summary table, with a reflection on their practical differences.

---

## Dataset

**Source:** [Palmer Archipelago (Antarctica) Penguin Data — Kaggle](https://www.kaggle.com/datasets/parulpandey/palmer-archipelago-antarctica-penguin-data)

The original dataset contains 344 rows. The version used here has been pre-cleaned: no missing-value handling, duplicate removal, or other preprocessing is required.

**Features used for clustering:**

| Column | Description |
|---|---|
| `bill_depth_mm` | Depth of the penguin's bill (mm) |
| `flipper_length_mm` | Length of the penguin's flipper (mm) |

---

### DBSCAN Workflow
- Fit `NearestNeighbors(n_neighbors=6)` and plot the sorted 6th-nearest-neighbor distances (k-distance plot) to identify the `eps` bend
- Fit `DBSCAN(eps=0.28, min_samples=6)` on `X_scaled`
- Print: clusters found (excluding noise), noise point count, silhouette score (non-noise points only)


- **Clusters found:** 3
- **Noise points:** 24
- **Silhouette score (non-noise only):** ≈ 0.4206
- **Settings:** `eps=0.28`, `min_samples=6`


### Mean Shift Workflow
- Fit `MeanShift(bandwidth=0.55, bin_seeding=True)` on `X_scaled`
- Print: clusters found, noise point count, silhouette score
- Convert cluster centers back to original units using the inverse scaler transform
- Plot `bill_depth_mm` vs `flipper_length_mm` coloured by cluster, with centers marked by a large X


- **Clusters found:** 3
- **Noise points:** 0
- **Silhouette score:** ≈ 0.5551
- **Settings:** `bandwidth=0.55`, `bin_seeding=True`

### Mean Shift Cluster Centers (Original Units)

| Cluster | bill_depth_mm | flipper_length_mm |
|---|---|---|
| 0 | 18.43 | 192.12 |
| 1 | 14.35 | 213.24 |
| 2 | 15.33 | 219.06 |



### Method Comparison

| Method | Main Settings | Clusters Found | Noise Points | Silhouette Score |
|---|---|---|---|---|
| DBSCAN | eps=0.28, min_samples=6 | 3 | 24 | 0.4206 |
| Mean Shift | bandwidth=0.55 | 3 | 0 | 0.5551 |

### Key Insight

DBSCAN is capable of identifying and isolating noise/outlier points rather than forcing every observation into a cluster, which makes it useful when the data is expected to contain anomalies. Mean Shift, by contrast, assigns every point to a cluster by shifting toward regions of highest density, producing a cleaner partition with no noise, and in this case a higher silhouette score. Although both methods found three clusters, the Mean Shift clusters appeared more naturally separated, while one of the DBSCAN clusters largely overlapped with another in the feature space.

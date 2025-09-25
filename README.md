# SV + CN Visualization Script

This script provides helper functions to visualize **structural variants (SVs)** and **copy number (CN)** information across chromosomes.  
It is designed to produce two aligned panels:  
- **CN panel (ax_cn)**: scatterplot of coverage bins + CNV segments.  
- **SV panel (ax_sv)**: arcs and markers for SV breakpoints, with optional cluster shading and labels.  

---

## Features

### Copy Number (CN) plotting
- **`plot_copy_number(ax_cn, chr_cov, chr_seg)`**  
  - Plots per-bin copy number estimates as a scatter plot.  
  - Adds segmentation tracks as red horizontal lines with vertical boundaries.  
  - Returns the maximum genomic x-position encountered for axis alignment.  

- **`add_legends(ax_cn, ax_sv)`**  
  - Adds two legends:  
    - CN legend (bins vs. segments) on the CN panel.  
    - SV legend (marker shapes for each SV category) on the SV panel.  

---

### Structural Variant (SV) plotting
- **`clusters_on_chr(chr_bedpe, chr_name)`**  
  - Groups SVs by cluster ID for a given chromosome.  
  - Collects positions and SV counts per cluster.  

- **`cluster_colors(cluster_ids)`**  
  - Assigns stable colors per cluster using matplotlib’s *tab20* palette.  

- **`plot_cluster_spans(ax_sv, ax_cn, clusters_dict, min_svs_per_cluster=2)`**  
  - Shades cluster spans on both SV and CN panels.  
  - Prepares label information for later placement.  
  - Returns `(label_recs, color_map)`.  

- **`place_cluster_labels(ax_sv, label_recs)`**  
  - Places cluster labels above the SV panel, using multiple “lanes” to avoid overlap.  

- **`plot_svs(ax_sv, ax_cn, chr_bed, chr_name, cluster_color_all)`**  
  - Draws intra-chromosomal SVs as arcs with markers.  
  - Draws inter-chromosomal SVs as local stubs + vertical guides.  
  - Places partner chromosome labels outside the SV panel, stacked per breakpoint.  

---

## Dependencies

- Python **3.8+**
- [pandas](https://pandas.pydata.org/)  
- [matplotlib](https://matplotlib.org/)

## Example Output

When run with a chromosome slice of coverage (`chr_cov`), segments (`chr_seg`), and structural variants (`chr_bed`), the script produces two aligned panels:

- **CN panel (top):**
  - Light-blue scatter points showing per-bin coverage (copy number estimates).
  - Red horizontal lines showing copy number segments.
  - Vertical dashed red lines marking segment boundaries.

- **SV panel (bottom):**
  - **Intra-chromosomal SVs:** shown as semicircular arcs with markers at both breakpoints.
  - **Inter-chromosomal SVs:** shown as local markers with vertical guide lines extending across both panels.
  - **Cluster spans:** shaded background regions (optional) with labels above the panel.
  - **Partner chromosome labels:** placed outside, above the SV panel, stacked vertically if multiple labels share the same x-position.

Both panels include legends:
- **CN legend** (upper right): “Coverage (bins)” and “Segments”.
- **SV legend** (upper left): marker shapes mapped to SV types (DEL, DUP/TD, INV, INTER, Complex).

The result is a compact visualization where copy number patterns, structural variants, and their clusters can be inspected together.

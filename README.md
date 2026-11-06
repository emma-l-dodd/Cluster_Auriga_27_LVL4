# Auriga 27 — Level 4 Clustering Catalogue

Contains **342,051 accreted stars** from Auriga Halo 27 Level 4.

## Clustering pipeline

Clustering is performed in integrals-of-motion (IOM) space \((E_n, L_z, L_\perp)\) and the method has three stages:

1. **KC clusters (`cluster_labels`)** — Initial single linkage clustering in IOM space using a significance to extract clusters (>3σ relative to smooth artificial haloes), producing **428 candidate clusters**.

2. **DM complexes (`dm_group_labels`)** — Nearby clusters from step 1 are merged using the Mahalanobis distances between cluster centroids to join clusters that are close in IOM space and reducing fragmentation of individual structures. See the dendrogram. 

3. **KS groups (`ks_group_labels`)** — DM complexes are tested for internal coherence using KS tests of the metallicity distributions between different combinations of clusters within each group/complex.

### Mahalanobis distance

The Mahalanobis distance:

$$
D_M(\mathbf{x},\boldsymbol{\mu}) =
\sqrt{(\mathbf{x}-\boldsymbol{\mu})^T
\boldsymbol{\Sigma}^{-1}
(\mathbf{x}-\boldsymbol{\mu})}
$$

where:

- $\mathbf{x}=(E_n,L_z,L_\perp)$ is the IOM coordinate vector.
- $\boldsymbol{\mu}$ is the mean of the cluster/group in IOM space.
- $\boldsymbol{\Sigma}$ is the covariance matrix of the cluster/group.
- 

## Added members to the clusters / groups

Using the cluster stars a 3D Mahalanobis ellipsoid is fitted to cluster (or group) in IOM space. The mahalanobis distance of each star to each cluster (or group) is then calcualted and they assigned to the closest one (minimum DM) as long as they are within a threshold of 2.13 (being apprxoimately 80% of the distribution in 3D if it was a Gaussian). This means we have different levels of memebers, the original memebers from the clustering algorithm and members we add with these ellipsoids. We did the same for the data and so I do it here as a check. Typically the purity remains similar and in some cases the completeness increases but I didnt play with this too much. 

## Catalogue labels

Each stage has an original and an extended (`_added`) label column:

| Original labels | Extended labels |
|---|---|
| `cluster_labels` | `cluster_labels_added` |
| `dm_group_labels` | `dm_group_labels_added` |
| `ks_group_labels` | `ks_group_labels_added` |

In all cases **`-1`:** Stars not assigned to any group or cluster.

For a group with ID `gid`:
- `ks_group_labels == gid`: Original members.
- `ks_group_labels_added == gid`: Original and extended members.

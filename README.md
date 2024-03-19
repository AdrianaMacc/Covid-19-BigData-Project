# Covid-19-BigData-Project
 SARS-COV-2 genome analysis using Big Data algorithms in order to find clusters of similar mutations that belongs to different clades which mutate together and generate the correspondent clade.

## Main Task: SARS-COV-2 genome analysis.
- Building a mutation profile (matrix rows: patients, columns: mutations).

- Generation of a network of similarities between mutations for each clade present in at least 1000 records.

- Generation of networks (for each clade) representing sets of frequently similar mutations (a triple of mutations which are always connected to others four).

- For each pair of clades, I calculate the similarity between clusters of frequently similar mutations, in order to study the tendency with which these clusters appear from variant to variant.

## Dataset Description:
*(the dataset is private)*
Each dataset file contains a matrix where each row corresponds to a patient affected by COVID-19 and each column contains various parameters relating to the sample collected, like:

- **Name** (String): sample name
- **Accession** (String): missing info
- **Geo_Location** (String): geographical position where the sample was collected.
- **Country** (String): country where the sample was collected.
- **Collection_Date** (Date): sample collection date.
- **Host** (String): species of the host on which the sample was collected (man...etc)
- **Isolation_Source** (String): pickup method.
- **clade** (String): sample virus covid variant (Omicron, 20A, etc...)
- [E.__76...- S.Y904H] (Int): series of binary columns with binary values 0/1 indicating the absence/presence of the particular mutation named by the name of the column.

## Algorithm steps:
- **Step one**: Data Acquisition and Description (Dask and Pandas libraries).
- **Step two**:  Grouping records by the "clade" attribute (covid variant).
- **Step three**: Filtering the clades which are present in a least 1000 records.
- **Step four**: Dictionary Creation {Clade: Patient-mutations}.
- **Step five**: **Min-Hashing, Locality Sensitive Hashing (LSH) and Jaccard similarity** in order to detect similar covid mutations.
- **Step six**: Creation of the multilayer network. For each covid clade I build a graph where the nodes are the mutations present in that particular variant and an edge is present between two mutations if their Jaccard similarity is greater than or equal to a threshold equal to 0.3
- **Step seven**: **Arena3D** graphs visualization.
- **Step eight**: **A-Priori Association Rules** algorithm in order to search for clusters of three mutations frequently associated with others four.
- **Step nine**: Find the similar clusters of mutations using a Jaccard threshold of 0.75.

<p align="left">
  <img src="https://github.com/AdrianaMacc/Covid-19-BigData-Project/blob/main/similarmutationgraph.png" width="500" title="Graph Example">
</p>
In the image above a similar mutations graph for a specific clade. 

The resulting dataframe has the following format:

[**Clade 1**, **Clade 2**, **C1_itemset**, **C2_itemset**, **jaccard_index**]

where:
- Clade1 and Clade 2 are the clades to which the similar clusters belong. 
- C1_itemset and C2_itemset are the two clusters of seven mutations that pass the threshold. 
- jaccard_index is the value of the Jaccard similarity calculated on the two clusters.


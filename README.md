# Amazon Review Network Analysis: Community Detection and Review Fraud Identification

A comprehensive data-driven analysis of Amazon product reviews focused on detecting fake review patterns through network analysis, community detection algorithms, and temporal behavioral analysis.

## 📋 Project Overview

This project analyzes the Amazon ratings dataset to identify suspicious review patterns and potential fraudulent behavior through a multi-layered approach combining:
- **Bipartite Network Analysis**: User-Product review networks
- **Community Detection**: Multiple algorithms (Louvain, Leiden, Infomap)
- **Temporal Analysis**: Burstiness metrics to detect coordinated review campaigns
- **Statistical Validation**: Correlation analysis and behavioral pattern recognition

## 🎯 Objectives

1. **Network Construction**: Build and analyze bipartite user-product networks from Amazon review data
2. **Community Detection**: Identify user communities with similar review behaviors
3. **Fraud Detection**: Detect suspicious review patterns through temporal and statistical analysis
4. **Algorithm Comparison**: Evaluate different community detection methods for fraud identification

## 📊 Dataset

- **Source**: Amazon product reviews dataset (`rec-amazon-ratings.edges`)
- **Focus Period**: Year 2006
- **Data Fields**: 
  - User ID
  - Product ID  
  - Review rating (1-5 stars)
  - Timestamp

## 🔧 Technologies & Libraries

```python
# Network Analysis
networkx, igraph

# Community Detection
python-louvain, leidenalg, infomap, cdlib

# Data Processing & Analysis
pandas, numpy, scipy

# Visualization
matplotlib

# Statistical Analysis
scikit-learn
```

## 🚀 Methodology

### 1. Data Preprocessing & Exploratory Analysis

- **Data Loading**: Conversion from edge list format to structured DataFrame
- **Temporal Filtering**: Focus on 2006 reviews for consistency
- **User & Product Profiling**:
  - Distribution of reviews per user (activity levels)
  - Distribution of reviews per product (popularity levels)
  - Rating variance analysis across products
  - User behavior comparison (active vs. casual users)

### 2. Bipartite Network Construction

- **Graph Structure**: Users and products as two distinct node sets
- **Edge Weights**: Number of common products reviewed (user-user projection)
- **Statistical Backbone Extraction**: 
  - Implementation of significance filtering
  - Hypergeometric test for edge validation
  - Threshold: α < 0.05 for statistically significant connections

### 3. Network Characterization

**Key Metrics Computed**:
- Number of nodes and edges
- Average degree and strength
- Standard deviation of degree distribution
- Network density
- Average clustering coefficient
- Modularity score
- Power-law exponent estimation (Γ ≈ expected value)

**Comparison with Random Graphs**:
- Double edge swap randomization
- Validation of non-random structure
- Significant clustering above random expectation

### 4. Pearson Correlation Analysis

- **Correlation Calculation**: Between user pairs based on common product ratings
- **Threshold**: Minimum 5 common products for reliable correlation
- **Distribution Analysis**: Identification of highly correlated user pairs
- **Interpretation**: High positive correlations (> 0.90) indicate similar rating behaviors

### 5. Community Detection Algorithms

Three state-of-the-art algorithms applied and compared:

#### **A. Louvain Method**
- Modularity optimization approach
- Fast and efficient for large networks
- Hierarchical community structure

#### **B. Leiden Algorithm**  
- Improvement over Louvain
- Better connected communities
- More stable partitions

#### **C. Infomap**
- Information-theoretic approach
- Random walk-based method
- Flow-based community detection

### 6. Community Filtering & Validation

**Two-stage filtering process**:

1. **Pearson Correlation Filter**: Communities with average internal Pearson > threshold (e.g., 0.05)
2. **Size Filter**: Communities with ≥ 5 members

**Results**: Identification of cohesive, statistically significant communities

### 7. Consensus Community Detection

- **Co-association Matrix Method**: Fusion of multiple partitions
- **Union Approach**: Handling nodes present in different algorithms
- **Agglomerative Clustering**: Hierarchical merging based on co-membership
- **Output**: Robust consensus communities across algorithms

### 8. Suspicious Product Detection

#### **Burstiness Analysis**

Mathematical framework for detecting abnormal review timing:

**Burstiness Coefficient (B)**:
```
B = (σ_τ - μ_τ) / (σ_τ + μ_τ)
```
Where:
- `μ_τ`: Mean time interval between reviews
- `σ_τ`: Standard deviation of time intervals
- `B ∈ [-1, 1]`: Values close to 1 indicate bursty behavior

**Suspicious Product Criteria**:
- Burstiness coefficient > 0.1
- High temporal concentration of reviews
- Deviation from natural review patterns

#### **Community-Level Analysis**

For each filtered community:
1. Extract all reviewed products
2. Calculate local burstiness metrics
3. Compare with global product statistics
4. Identify products with suspicious patterns **only within specific communities**

**Output**: Table of suspicious products with:
- Local vs. global burstiness
- Local vs. global review counts
- Local vs. global mean ratings
- Mean inter-arrival time (μ_τ)

### 9. Advanced Fraud Detection: Baseline Deviation Analysis

Enhanced analysis considering historical context:

**Key Innovation**: 
- Calculate **prior mean rating** using all historical data before each review
- Detect coordinated review campaigns that **drastically shift product ratings**

**Suspicious Coalition Detection**:
A time window is flagged as suspicious if it meets ALL three conditions:

1. **Burst Metric**: ≥ 3 reviews AND ≥ 10% of product's maximum single-period review count
2. **Polarization**: Low std deviation (< 0.8) AND extreme mean rating (≤ 1.5 or ≥ 4.5)
3. **Baseline Deviation**: |mean_rating - baseline_prior_mean| ≥ 1.0

**Time Window Analysis**: 
- Configurable resolution (e.g., 1 day, 7 days)
- Sliding window approach for temporal pattern detection

### 10. Case Study: Perfect Correlation Clique

**Discovery**: Identification of a 4-user clique with:
- Perfect Pearson correlation (ρ = 1.0) between all pairs
- High volume of shared products (> 50 common reviews)
- 79 products reviewed by ALL 4 users

**Detailed Investigation**:
1. **Intersection Analysis**: Products reviewed by entire suspect group
2. **Dual-Method Validation**:
   - Burstiness coefficient analysis
   - Historical deviation detection
3. **High Confidence Results**: 6 products flagged by BOTH methods

**Key Finding**: 
- These products show coordinated review timing AND significant rating manipulation
- Demonstrates limitations of pure community detection
- Highlights need for **multi-method validation** (Graph + Temporal + Behavioral)

### 11. Algorithm Evaluation & Limitations

**Community Detection Challenges**:
- Louvain and Infomap merged small suspicious groups into large components
- Low internal Pearson correlation in giant communities
- Filtering by correlation and size removes these large, weakly-connected groups

**Why Traditional Methods Miss Fraud**:
- Small fraudulent cliques get absorbed into large communities
- Modularity optimization favors larger communities
- Need for **hybrid approaches** combining multiple signals

## 📈 Key Results

### Network Properties
- **Supercritical Regime**: Average degree > log(N), indicating giant component presence
- **Scale-Free Characteristics**: Degree distribution follows approximate power-law
- **High Clustering**: Significantly higher than random graphs
- **Modular Structure**: Clear community organization

### Community Detection
- **Algorithm Comparison**: Different algorithms detect varying numbers of communities
- **Filtered Communities**: Significant reduction after Pearson and size filters
- **Consensus Communities**: Robust community structure across methods

### Fraud Detection
- **Suspicious Products**: Multiple products identified with abnormal review patterns
- **Community-Specific Fraud**: Some products suspicious only within certain communities
- **Temporal Patterns**: Clear burstiness signatures in fraudulent review campaigns
- **Coordinated Behavior**: Detection of user cliques with perfect correlation

### Case Study Insights
- **4-User Clique**: Perfect correlation, 79 common products
- **6 High-Confidence Manipulated Products**: Validated by multiple methods
- **Rating Shifts**: Up to 2.5-point deviations from historical baselines
- **Timing Patterns**: Concentrated review bursts within 24-hour windows

## 💡 Conclusions

1. **Multi-Method Necessity**: No single approach is sufficient for fraud detection
2. **Temporal Signals**: Burstiness analysis is crucial for identifying coordinated campaigns
3. **Baseline Context**: Historical rating trends essential for detecting manipulation
4. **Graph Structure**: Network analysis reveals user relationships but misses small fraud groups
5. **Hybrid Framework**: Combination of graph, temporal, and statistical methods required

## 🔮 Future Work

- **Real-time Detection**: Streaming algorithms for live fraud detection
- **Text Analysis**: Incorporate review content (NLP) for sentiment manipulation detection
- **Product Categories**: Category-specific fraud patterns
- **Temporal Evolution**: Longitudinal study of fraud tactics over multiple years
- **Machine Learning**: Supervised learning models trained on validated fraud cases
- **Scalability**: Optimization for datasets with millions of reviews

## 📁 Repository Structure

```
├── main.ipynb          # Main analysis notebook
├── rec-amazon-ratings.edges     # Dataset 
└── README.md                    # This file
```

## 🚦 How to Run

1. **Install dependencies**:
```bash
pip install networkx igraph python-louvain leidenalg infomap cdlib pandas numpy scipy matplotlib scikit-learn pyreadr
```

2. **Prepare dataset**: Place `rec-amazon-ratings.edges` in working directory

3. **Execute notebook**: Run `main.ipynb` sequentially

4. **Review outputs**: Analyze plots, metrics, and suspicious product tables

## 📚 References

- **Bipartite Network Projection**: Newman, M. E. J. (2001). Scientific collaboration networks
- **Statistical Backbone**: Serrano et al. (2009). Extracting the multiscale backbone of complex weighted networks
- **Burstiness**: Goh & Barabási (2008). Burstiness and memory in complex systems
- **Community Detection**:
  - Louvain: Blondel et al. (2008)
  - Leiden: Traag et al. (2019)
  - Infomap: Rosvall & Bergstrom (2008)

## 👥 Author

Querqui A. 
Sorrentini M.
Trionfetti F.

## 📄 License

This project is for academic purposes. Dataset rights belong to original providers.

---

**Note**: This analysis demonstrates methodologies for fraud detection in review networks. Results should be validated before making real-world accusations of fraudulent behavior.

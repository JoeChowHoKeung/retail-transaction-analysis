# RFM Clustering

## Objective

- To indicate the profitability of customers
- To group and focus studying on loyal and profitable customers

## Methodology

To achieve this objective, we will use a combination of **RFM (Recency, Frequency, Monetary) analysis** and **clustering techniques**. **RFM analysis** will be used to create a baseline segmentation of customers based on their **recent purchasing behavior, frequency of purchases, and total monetary value**. Then, **clustering algorithms** such as **k-means or hierarchical clustering** will be applied to further refine the segmentation and identify distinct customer groups based on additional variables such as demographics, product preferences, or website behavior. The resulting customer segments will be evaluated based on their size, revenue potential, and distinct characteristics, and **recommendations** will be made for how to target and engage each segment effectively. The final output will be a set of **customer profiles** and recommended marketing strategies for each segment.

## Findings

## Data Processing

### RFM matrix

```python
# produce RFM matrix 
rfm_raw = dataset.get(['customerid', 'revenue', 'invoiceno', 'invoicedate'])
rfm_matrix = (rfm_raw
    .groupby(['customerid'])
    .agg(monetary = ('revenue', 'sum'),
        frequency = ('invoiceno', 'nunique'),
        recency = ('invoicedate', 'max'))
)       
rfm_matrix['recency'] = (rfm_matrix.recency.max() - rfm_matrix.recency).dt.days
rfm_matrix = rfm_matrix.loc[
	(rfm_matrix.get(['monetary', 'frequency']) > rfm_matrix.get(['monetary', 'frequency']).quantile(0.05)).all(axis=1)
 & (rfm_matrix < rfm_matrix.quantile(0.95)).all(axis=1),
 :]
```

| customerid | monetary | frequency | recency |
| --- | --- | --- | --- |
| 12747 | 4196.01 | 11 | 1 |
| 12749 | 4090.88 | 5 | 3 |
| 12820 | 942.34 | 4 | 2 |
| 12822 | 948.88 | 2 | 70 |
| 12823 | 1759.5 | 5 | 74 |
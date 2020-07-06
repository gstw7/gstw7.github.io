---
title: Análise COVID-19 em SP - Dados do Hospital Albert Einstein
date: 2020-05-05
tags: [python, data manipulation, data visualization, importing & cleaning data, ordinal encoder, machine learning]
excerpt: "Data Science"
mathjax: "true"
---

## [Dados](https://bit.ly/3b5K2Nw)


```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
pd.set_option('display.max_columns', 200)
```


```python
df = pd.read_excel('dataset.xlsx')
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Patient ID</th>
      <th>Patient age quantile</th>
      <th>SARS-Cov-2 exam result</th>
      <th>Patient addmited to regular ward (1=yes, 0=no)</th>
      <th>Patient addmited to semi-intensive unit (1=yes, 0=no)</th>
      <th>Patient addmited to intensive care unit (1=yes, 0=no)</th>
      <th>Hematocrit</th>
      <th>Hemoglobin</th>
      <th>Platelets</th>
      <th>Mean platelet volume</th>
      <th>Red blood Cells</th>
      <th>Lymphocytes</th>
      <th>Mean corpuscular hemoglobin concentration (MCHC)</th>
      <th>Leukocytes</th>
      <th>Basophils</th>
      <th>Mean corpuscular hemoglobin (MCH)</th>
      <th>Eosinophils</th>
      <th>Mean corpuscular volume (MCV)</th>
      <th>Monocytes</th>
      <th>Red blood cell distribution width (RDW)</th>
      <th>Serum Glucose</th>
      <th>Respiratory Syncytial Virus</th>
      <th>Influenza A</th>
      <th>Influenza B</th>
      <th>Parainfluenza 1</th>
      <th>CoronavirusNL63</th>
      <th>Rhinovirus/Enterovirus</th>
      <th>Mycoplasma pneumoniae</th>
      <th>Coronavirus HKU1</th>
      <th>Parainfluenza 3</th>
      <th>Chlamydophila pneumoniae</th>
      <th>Adenovirus</th>
      <th>Parainfluenza 4</th>
      <th>Coronavirus229E</th>
      <th>CoronavirusOC43</th>
      <th>Inf A H1N1 2009</th>
      <th>Bordetella pertussis</th>
      <th>Metapneumovirus</th>
      <th>Parainfluenza 2</th>
      <th>Neutrophils</th>
      <th>Urea</th>
      <th>Proteina C reativa mg/dL</th>
      <th>Creatinine</th>
      <th>Potassium</th>
      <th>Sodium</th>
      <th>Influenza B, rapid test</th>
      <th>Influenza A, rapid test</th>
      <th>Alanine transaminase</th>
      <th>Aspartate transaminase</th>
      <th>Gamma-glutamyltransferase</th>
      <th>Total Bilirubin</th>
      <th>Direct Bilirubin</th>
      <th>Indirect Bilirubin</th>
      <th>Alkaline phosphatase</th>
      <th>Ionized calcium</th>
      <th>Strepto A</th>
      <th>Magnesium</th>
      <th>pCO2 (venous blood gas analysis)</th>
      <th>Hb saturation (venous blood gas analysis)</th>
      <th>Base excess (venous blood gas analysis)</th>
      <th>pO2 (venous blood gas analysis)</th>
      <th>Fio2 (venous blood gas analysis)</th>
      <th>Total CO2 (venous blood gas analysis)</th>
      <th>pH (venous blood gas analysis)</th>
      <th>HCO3 (venous blood gas analysis)</th>
      <th>Rods #</th>
      <th>Segmented</th>
      <th>Promyelocytes</th>
      <th>Metamyelocytes</th>
      <th>Myelocytes</th>
      <th>Myeloblasts</th>
      <th>Urine - Esterase</th>
      <th>Urine - Aspect</th>
      <th>Urine - pH</th>
      <th>Urine - Hemoglobin</th>
      <th>Urine - Bile pigments</th>
      <th>Urine - Ketone Bodies</th>
      <th>Urine - Nitrite</th>
      <th>Urine - Density</th>
      <th>Urine - Urobilinogen</th>
      <th>Urine - Protein</th>
      <th>Urine - Sugar</th>
      <th>Urine - Leukocytes</th>
      <th>Urine - Crystals</th>
      <th>Urine - Red blood cells</th>
      <th>Urine - Hyaline cylinders</th>
      <th>Urine - Granular cylinders</th>
      <th>Urine - Yeasts</th>
      <th>Urine - Color</th>
      <th>Partial thromboplastin time (PTT)</th>
      <th>Relationship (Patient/Normal)</th>
      <th>International normalized ratio (INR)</th>
      <th>Lactic Dehydrogenase</th>
      <th>Prothrombin time (PT), Activity</th>
      <th>Vitamin B12</th>
      <th>Creatine phosphokinase (CPK)</th>
      <th>Ferritin</th>
      <th>Arterial Lactic Acid</th>
      <th>Lipase dosage</th>
      <th>D-Dimer</th>
      <th>Albumin</th>
      <th>Hb saturation (arterial blood gases)</th>
      <th>pCO2 (arterial blood gas analysis)</th>
      <th>Base excess (arterial blood gas analysis)</th>
      <th>pH (arterial blood gas analysis)</th>
      <th>Total CO2 (arterial blood gas analysis)</th>
      <th>HCO3 (arterial blood gas analysis)</th>
      <th>pO2 (arterial blood gas analysis)</th>
      <th>Arteiral Fio2</th>
      <th>Phosphor</th>
      <th>ctO2 (arterial blood gas analysis)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>44477f75e8169d2</td>
      <td>13</td>
      <td>negative</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>1</td>
      <td>126e9dd13932f68</td>
      <td>17</td>
      <td>negative</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.236515</td>
      <td>-0.02234</td>
      <td>-0.517413</td>
      <td>0.010677</td>
      <td>0.102004</td>
      <td>0.318366</td>
      <td>-0.95079</td>
      <td>-0.09461</td>
      <td>-0.223767</td>
      <td>-0.292269</td>
      <td>1.482158</td>
      <td>0.166192</td>
      <td>0.357547</td>
      <td>-0.625073</td>
      <td>-0.140648</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>detected</td>
      <td>NaN</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>-0.619086</td>
      <td>1.198059</td>
      <td>-0.147895</td>
      <td>2.089928</td>
      <td>-0.305787</td>
      <td>0.862512</td>
      <td>negative</td>
      <td>negative</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2</td>
      <td>a46b4402a0e5696</td>
      <td>8</td>
      <td>negative</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>3</td>
      <td>f7d619a94f97c45</td>
      <td>5</td>
      <td>negative</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>4</td>
      <td>d9e41465789c2b5</td>
      <td>15</td>
      <td>negative</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>detected</td>
      <td>NaN</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.shape
```




    (5644, 111)




```python
a_trocar = {
    'negative' : 0,
    'positive' : 1
}
df['SARS-Cov-2 exam result'] = df['SARS-Cov-2 exam result'].map(a_trocar)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Patient ID</th>
      <th>Patient age quantile</th>
      <th>SARS-Cov-2 exam result</th>
      <th>Patient addmited to regular ward (1=yes, 0=no)</th>
      <th>Patient addmited to semi-intensive unit (1=yes, 0=no)</th>
      <th>Patient addmited to intensive care unit (1=yes, 0=no)</th>
      <th>Hematocrit</th>
      <th>Hemoglobin</th>
      <th>Platelets</th>
      <th>Mean platelet volume</th>
      <th>Red blood Cells</th>
      <th>Lymphocytes</th>
      <th>Mean corpuscular hemoglobin concentration (MCHC)</th>
      <th>Leukocytes</th>
      <th>Basophils</th>
      <th>Mean corpuscular hemoglobin (MCH)</th>
      <th>Eosinophils</th>
      <th>Mean corpuscular volume (MCV)</th>
      <th>Monocytes</th>
      <th>Red blood cell distribution width (RDW)</th>
      <th>Serum Glucose</th>
      <th>Respiratory Syncytial Virus</th>
      <th>Influenza A</th>
      <th>Influenza B</th>
      <th>Parainfluenza 1</th>
      <th>CoronavirusNL63</th>
      <th>Rhinovirus/Enterovirus</th>
      <th>Mycoplasma pneumoniae</th>
      <th>Coronavirus HKU1</th>
      <th>Parainfluenza 3</th>
      <th>Chlamydophila pneumoniae</th>
      <th>Adenovirus</th>
      <th>Parainfluenza 4</th>
      <th>Coronavirus229E</th>
      <th>CoronavirusOC43</th>
      <th>Inf A H1N1 2009</th>
      <th>Bordetella pertussis</th>
      <th>Metapneumovirus</th>
      <th>Parainfluenza 2</th>
      <th>Neutrophils</th>
      <th>Urea</th>
      <th>Proteina C reativa mg/dL</th>
      <th>Creatinine</th>
      <th>Potassium</th>
      <th>Sodium</th>
      <th>Influenza B, rapid test</th>
      <th>Influenza A, rapid test</th>
      <th>Alanine transaminase</th>
      <th>Aspartate transaminase</th>
      <th>Gamma-glutamyltransferase</th>
      <th>Total Bilirubin</th>
      <th>Direct Bilirubin</th>
      <th>Indirect Bilirubin</th>
      <th>Alkaline phosphatase</th>
      <th>Ionized calcium</th>
      <th>Strepto A</th>
      <th>Magnesium</th>
      <th>pCO2 (venous blood gas analysis)</th>
      <th>Hb saturation (venous blood gas analysis)</th>
      <th>Base excess (venous blood gas analysis)</th>
      <th>pO2 (venous blood gas analysis)</th>
      <th>Fio2 (venous blood gas analysis)</th>
      <th>Total CO2 (venous blood gas analysis)</th>
      <th>pH (venous blood gas analysis)</th>
      <th>HCO3 (venous blood gas analysis)</th>
      <th>Rods #</th>
      <th>Segmented</th>
      <th>Promyelocytes</th>
      <th>Metamyelocytes</th>
      <th>Myelocytes</th>
      <th>Myeloblasts</th>
      <th>Urine - Esterase</th>
      <th>Urine - Aspect</th>
      <th>Urine - pH</th>
      <th>Urine - Hemoglobin</th>
      <th>Urine - Bile pigments</th>
      <th>Urine - Ketone Bodies</th>
      <th>Urine - Nitrite</th>
      <th>Urine - Density</th>
      <th>Urine - Urobilinogen</th>
      <th>Urine - Protein</th>
      <th>Urine - Sugar</th>
      <th>Urine - Leukocytes</th>
      <th>Urine - Crystals</th>
      <th>Urine - Red blood cells</th>
      <th>Urine - Hyaline cylinders</th>
      <th>Urine - Granular cylinders</th>
      <th>Urine - Yeasts</th>
      <th>Urine - Color</th>
      <th>Partial thromboplastin time (PTT)</th>
      <th>Relationship (Patient/Normal)</th>
      <th>International normalized ratio (INR)</th>
      <th>Lactic Dehydrogenase</th>
      <th>Prothrombin time (PT), Activity</th>
      <th>Vitamin B12</th>
      <th>Creatine phosphokinase (CPK)</th>
      <th>Ferritin</th>
      <th>Arterial Lactic Acid</th>
      <th>Lipase dosage</th>
      <th>D-Dimer</th>
      <th>Albumin</th>
      <th>Hb saturation (arterial blood gases)</th>
      <th>pCO2 (arterial blood gas analysis)</th>
      <th>Base excess (arterial blood gas analysis)</th>
      <th>pH (arterial blood gas analysis)</th>
      <th>Total CO2 (arterial blood gas analysis)</th>
      <th>HCO3 (arterial blood gas analysis)</th>
      <th>pO2 (arterial blood gas analysis)</th>
      <th>Arteiral Fio2</th>
      <th>Phosphor</th>
      <th>ctO2 (arterial blood gas analysis)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>44477f75e8169d2</td>
      <td>13</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>1</td>
      <td>126e9dd13932f68</td>
      <td>17</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.236515</td>
      <td>-0.02234</td>
      <td>-0.517413</td>
      <td>0.010677</td>
      <td>0.102004</td>
      <td>0.318366</td>
      <td>-0.95079</td>
      <td>-0.09461</td>
      <td>-0.223767</td>
      <td>-0.292269</td>
      <td>1.482158</td>
      <td>0.166192</td>
      <td>0.357547</td>
      <td>-0.625073</td>
      <td>-0.140648</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>detected</td>
      <td>NaN</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>-0.619086</td>
      <td>1.198059</td>
      <td>-0.147895</td>
      <td>2.089928</td>
      <td>-0.305787</td>
      <td>0.862512</td>
      <td>negative</td>
      <td>negative</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2</td>
      <td>a46b4402a0e5696</td>
      <td>8</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>3</td>
      <td>f7d619a94f97c45</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>4</td>
      <td>d9e41465789c2b5</td>
      <td>15</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>detected</td>
      <td>NaN</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>not_detected</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['SARS-Cov-2 exam result'].value_counts()
```




    0    5086
    1     558
    Name: SARS-Cov-2 exam result, dtype: int64




```python
df.replace('not_detected', 0, inplace=True)
df.replace('detected', 1, inplace = True)
df.replace('negative', 0, inplace = True)
df.replace('positive', 1, inplace = True)
df.replace('not_done', 0, inplace=True)
df = df.fillna(-1)
df.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Patient ID</th>
      <th>Patient age quantile</th>
      <th>SARS-Cov-2 exam result</th>
      <th>Patient addmited to regular ward (1=yes, 0=no)</th>
      <th>Patient addmited to semi-intensive unit (1=yes, 0=no)</th>
      <th>Patient addmited to intensive care unit (1=yes, 0=no)</th>
      <th>Hematocrit</th>
      <th>Hemoglobin</th>
      <th>Platelets</th>
      <th>Mean platelet volume</th>
      <th>Red blood Cells</th>
      <th>Lymphocytes</th>
      <th>Mean corpuscular hemoglobin concentration (MCHC)</th>
      <th>Leukocytes</th>
      <th>Basophils</th>
      <th>Mean corpuscular hemoglobin (MCH)</th>
      <th>Eosinophils</th>
      <th>Mean corpuscular volume (MCV)</th>
      <th>Monocytes</th>
      <th>Red blood cell distribution width (RDW)</th>
      <th>Serum Glucose</th>
      <th>Respiratory Syncytial Virus</th>
      <th>Influenza A</th>
      <th>Influenza B</th>
      <th>Parainfluenza 1</th>
      <th>CoronavirusNL63</th>
      <th>Rhinovirus/Enterovirus</th>
      <th>Mycoplasma pneumoniae</th>
      <th>Coronavirus HKU1</th>
      <th>Parainfluenza 3</th>
      <th>Chlamydophila pneumoniae</th>
      <th>Adenovirus</th>
      <th>Parainfluenza 4</th>
      <th>Coronavirus229E</th>
      <th>CoronavirusOC43</th>
      <th>Inf A H1N1 2009</th>
      <th>Bordetella pertussis</th>
      <th>Metapneumovirus</th>
      <th>Parainfluenza 2</th>
      <th>Neutrophils</th>
      <th>Urea</th>
      <th>Proteina C reativa mg/dL</th>
      <th>Creatinine</th>
      <th>Potassium</th>
      <th>Sodium</th>
      <th>Influenza B, rapid test</th>
      <th>Influenza A, rapid test</th>
      <th>Alanine transaminase</th>
      <th>Aspartate transaminase</th>
      <th>Gamma-glutamyltransferase</th>
      <th>Total Bilirubin</th>
      <th>Direct Bilirubin</th>
      <th>Indirect Bilirubin</th>
      <th>Alkaline phosphatase</th>
      <th>Ionized calcium</th>
      <th>Strepto A</th>
      <th>Magnesium</th>
      <th>pCO2 (venous blood gas analysis)</th>
      <th>Hb saturation (venous blood gas analysis)</th>
      <th>Base excess (venous blood gas analysis)</th>
      <th>pO2 (venous blood gas analysis)</th>
      <th>Fio2 (venous blood gas analysis)</th>
      <th>Total CO2 (venous blood gas analysis)</th>
      <th>pH (venous blood gas analysis)</th>
      <th>HCO3 (venous blood gas analysis)</th>
      <th>Rods #</th>
      <th>Segmented</th>
      <th>Promyelocytes</th>
      <th>Metamyelocytes</th>
      <th>Myelocytes</th>
      <th>Myeloblasts</th>
      <th>Urine - Esterase</th>
      <th>Urine - Aspect</th>
      <th>Urine - pH</th>
      <th>Urine - Hemoglobin</th>
      <th>Urine - Bile pigments</th>
      <th>Urine - Ketone Bodies</th>
      <th>Urine - Nitrite</th>
      <th>Urine - Density</th>
      <th>Urine - Urobilinogen</th>
      <th>Urine - Protein</th>
      <th>Urine - Sugar</th>
      <th>Urine - Leukocytes</th>
      <th>Urine - Crystals</th>
      <th>Urine - Red blood cells</th>
      <th>Urine - Hyaline cylinders</th>
      <th>Urine - Granular cylinders</th>
      <th>Urine - Yeasts</th>
      <th>Urine - Color</th>
      <th>Partial thromboplastin time (PTT)</th>
      <th>Relationship (Patient/Normal)</th>
      <th>International normalized ratio (INR)</th>
      <th>Lactic Dehydrogenase</th>
      <th>Prothrombin time (PT), Activity</th>
      <th>Vitamin B12</th>
      <th>Creatine phosphokinase (CPK)</th>
      <th>Ferritin</th>
      <th>Arterial Lactic Acid</th>
      <th>Lipase dosage</th>
      <th>D-Dimer</th>
      <th>Albumin</th>
      <th>Hb saturation (arterial blood gases)</th>
      <th>pCO2 (arterial blood gas analysis)</th>
      <th>Base excess (arterial blood gas analysis)</th>
      <th>pH (arterial blood gas analysis)</th>
      <th>Total CO2 (arterial blood gas analysis)</th>
      <th>HCO3 (arterial blood gas analysis)</th>
      <th>pO2 (arterial blood gas analysis)</th>
      <th>Arteiral Fio2</th>
      <th>Phosphor</th>
      <th>ctO2 (arterial blood gas analysis)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>5639</td>
      <td>ae66feb9e4dc3a0</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.00000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.00000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.00000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.000000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
    </tr>
    <tr>
      <td>5640</td>
      <td>517c2834024f3ea</td>
      <td>17</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.00000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.00000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.00000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.000000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
    </tr>
    <tr>
      <td>5641</td>
      <td>5c57d6037fe266d</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.00000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.00000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.00000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.000000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
    </tr>
    <tr>
      <td>5642</td>
      <td>c20c44766f28291</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.00000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.00000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.00000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>clear</td>
      <td>5</td>
      <td>absent</td>
      <td>absent</td>
      <td>absent</td>
      <td>-1.0</td>
      <td>-0.338525</td>
      <td>normal</td>
      <td>absent</td>
      <td>-1.0</td>
      <td>29000</td>
      <td>Ausentes</td>
      <td>-0.177169</td>
      <td>absent</td>
      <td>absent</td>
      <td>absent</td>
      <td>yellow</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
    </tr>
    <tr>
      <td>5643</td>
      <td>2697fdccbfeb7f7</td>
      <td>19</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.694287</td>
      <td>0.541564</td>
      <td>-0.906829</td>
      <td>-0.325903</td>
      <td>0.578024</td>
      <td>-0.295726</td>
      <td>-0.353319</td>
      <td>-1.288428</td>
      <td>-1.140144</td>
      <td>-0.135455</td>
      <td>-0.835508</td>
      <td>0.025985</td>
      <td>0.567652</td>
      <td>-0.18279</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>0.380685</td>
      <td>0.453725</td>
      <td>-0.50357</td>
      <td>-0.735872</td>
      <td>-0.552949</td>
      <td>-0.934388</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-0.28361</td>
      <td>0.108761</td>
      <td>-0.420454</td>
      <td>-0.480996</td>
      <td>-0.586463</td>
      <td>-0.278654</td>
      <td>-0.243405</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1.000000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.000000</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>0.420204</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-0.343291</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
      <td>-1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
from category_encoders.ordinal import OrdinalEncoder

enc = OrdinalEncoder(cols=['Urine - Esterase', 'Urine - Aspect', 'Urine - pH', 'Urine - Hemoglobin', 'Urine - Bile pigments', 'Urine - Ketone Bodies',
                           'Urine - Nitrite', 'Urine - Urobilinogen', 'Urine - Leukocytes' ,'Urine - Protein', 'Urine - Sugar', 'Urine - Crystals', 'Urine - Red blood cells',
                           'Urine - Hyaline cylinders', 'Urine - Granular cylinders', 'Urine - Yeasts', 'Urine - Color'])
enc.fit(df)
df_ord = enc.transform(df)
```


```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import recall_score
from sklearn.metrics import precision_score


x = df_ord.drop(['SARS-Cov-2 exam result','Patient ID'], axis = 1)
y = df_ord['SARS-Cov-2 exam result']

SEED = 20

treino_x, teste_x, treino_y, teste_y = train_test_split(x, y, random_state = SEED)

print('Treino com %d elementos e teste com %d elementos' % (len(treino_x), len(teste_x)))
```

    Treino com 4233 elementos e teste com 1411 elementos



```python
treino_y.value_counts()
```




    0    3816
    1     417
    Name: SARS-Cov-2 exam result, dtype: int64




```python
from imblearn.over_sampling import SMOTE

smt = SMOTE()
treino_x, treino_y = smt.fit_sample(treino_x, treino_y)
```


```python
np.bincount(treino_y)
```




    array([3816, 3816], dtype=int64)




```python
modelo = RandomForestClassifier(n_estimators=200, n_jobs=-1)
modelo.fit(treino_x, treino_y)
previsoes = modelo.predict(teste_x)

recall = recall_score(teste_y, previsoes) *100
print("O recall foi %.2f%%" % recall)

precision = precision_score(teste_y, previsoes) *100
print("A precision foi de %.2f%%" % precision)


```

    O recall foi 58.16%
    A precision foi de 13.51%



```python
previsoes = modelo.predict_proba(teste_x)[:,1]

def avaliar_threshold(teste_y, previsoes):
    lista_recall = []
    lista_precision = []

    for i in np.linspace(0,1,1000):
        novas_previsoes = previsoes >= i
        lista_recall.append(recall_score(teste_y, novas_previsoes))
        lista_precision.append(precision_score(teste_y, novas_previsoes))

    plt.scatter(np.linspace(0,1,1000), lista_recall)
    plt.scatter(np.linspace(0,1,1000), lista_precision)
    plt.show()

avaliar_threshold(teste_y, previsoes)
```

    C:\Users\gust4\Anaconda3\lib\site-packages\sklearn\metrics\_classification.py:1272: UndefinedMetricWarning: Precision is ill-defined and being set to 0.0 due to no predicted samples. Use `zero_division` parameter to control this behavior.
      _warn_prf(average, modifier, msg_start, len(result))



![png]({{ "/assets\images\Análise COVID-19 SP Albert Einstein/output_12_1.png" }})



```python

```


# Data merge
### 透過merge了解各個網頁的產品屬性 （分為4類）


```python
import pandas as pd
import numpy as np

```


```python
behavior = pd.read_csv('behavior_reduced.csv')
wm = pd.read_csv('TBN_WM_TXN.csv')
cc = pd.read_csv('TBN_CC_APPLY.csv')
ln = pd.read_csv('TBN_LN_APPLY.csv')
fx = pd.read_csv('TBN_FX_TXN.csv')
```

### 該商品購買兩次以上者remove。只留下一次購買


```python
wm2 = wm.drop_duplicates('CUST_NO')
cc2 = cc.drop_duplicates('CUST_NO')
ln2 = ln.drop_duplicates('CUST_NO')
fx2 = fx.drop_duplicates('CUST_NO')
```

### Behavior left join on 4個商品


```python
b = wm2
c = cc2
d = ln2
e = fx2

#Merge

ab = pd.merge(behavior, b, how='left', on='CUST_NO')
abc = pd.merge(ab, c, how='left', on='CUST_NO')
abcd = pd.merge(abc, d, how='left', on='CUST_NO')
abcde = pd.merge(abcd, e, how='left', on='CUST_NO')
```


```python
abcde.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>4th</th>
      <th>1234PAGE</th>
      <th>TXN_DT_x</th>
      <th>CUST_RISK_CODE</th>
      <th>INVEST_TYPE_CODE</th>
      <th>WM_TXN_AMT</th>
      <th>TXN_DT_y</th>
      <th>TXN_DT_x</th>
      <th>LN_AMT</th>
      <th>LN_USE</th>
      <th>TXN_DT_y</th>
      <th>FX_TXN_AMT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AZTHNWQ_LXMGIMYG</td>
      <td>9462</td>
      <td>gygrt/e2c/iougkjr/</td>
      <td>gygrt</td>
      <td>e2c</td>
      <td>iougkjr</td>
      <td>NaN</td>
      <td>gygrt/e2c/iougkjr</td>
      <td>NaN</td>
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
      <th>1</th>
      <td>AZTHNWQ_LXMGIMYG</td>
      <td>9528</td>
      <td>gygrt/wgdqth/gsxrioof/</td>
      <td>gygrt</td>
      <td>wgdqth</td>
      <td>gsxrioof</td>
      <td>NaN</td>
      <td>gygrt/wgdqth/gsxrioof</td>
      <td>NaN</td>
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
      <th>2</th>
      <td>3PY428CHUQBULFIG</td>
      <td>9458</td>
      <td>edrn/deoxt/rgws-cgrtgu</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>rgws-cgrtgu</td>
      <td>NaN</td>
      <td>edrn/deoxt/rgws-cgrtgu</td>
      <td>NaN</td>
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
      <th>3</th>
      <td>JVPD1QUJWVLMZU8S</td>
      <td>9457</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9546.0</td>
      <td>53728.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>JVPD1QUJWVLMZU8S</td>
      <td>9485</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9546.0</td>
      <td>53728.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
abcde['4th'] = np.where(pd.isnull(abcde['4th']), abcde['3rd'], abcde['4th'])
```


```python
abcde.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>4th</th>
      <th>1234PAGE</th>
      <th>TXN_DT_x</th>
      <th>CUST_RISK_CODE</th>
      <th>INVEST_TYPE_CODE</th>
      <th>WM_TXN_AMT</th>
      <th>TXN_DT_y</th>
      <th>TXN_DT_x</th>
      <th>LN_AMT</th>
      <th>LN_USE</th>
      <th>TXN_DT_y</th>
      <th>FX_TXN_AMT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AZTHNWQ_LXMGIMYG</td>
      <td>9462</td>
      <td>gygrt/e2c/iougkjr/</td>
      <td>gygrt</td>
      <td>e2c</td>
      <td>iougkjr</td>
      <td>iougkjr</td>
      <td>gygrt/e2c/iougkjr</td>
      <td>NaN</td>
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
      <th>1</th>
      <td>AZTHNWQ_LXMGIMYG</td>
      <td>9528</td>
      <td>gygrt/wgdqth/gsxrioof/</td>
      <td>gygrt</td>
      <td>wgdqth</td>
      <td>gsxrioof</td>
      <td>gsxrioof</td>
      <td>gygrt/wgdqth/gsxrioof</td>
      <td>NaN</td>
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
      <th>2</th>
      <td>3PY428CHUQBULFIG</td>
      <td>9458</td>
      <td>edrn/deoxt/rgws-cgrtgu</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>rgws-cgrtgu</td>
      <td>rgws-cgrtgu</td>
      <td>edrn/deoxt/rgws-cgrtgu</td>
      <td>NaN</td>
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
      <th>3</th>
      <td>JVPD1QUJWVLMZU8S</td>
      <td>9457</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9546.0</td>
      <td>53728.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>JVPD1QUJWVLMZU8S</td>
      <td>9485</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9546.0</td>
      <td>53728.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
abcde = abcde.sort_values(by = 'CUST_NO', ascending = True, axis = 0)
```


```python
abcde.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>4th</th>
      <th>1234PAGE</th>
      <th>TXN_DT_x</th>
      <th>CUST_RISK_CODE</th>
      <th>INVEST_TYPE_CODE</th>
      <th>WM_TXN_AMT</th>
      <th>TXN_DT_y</th>
      <th>TXN_DT_x</th>
      <th>LN_AMT</th>
      <th>LN_USE</th>
      <th>TXN_DT_y</th>
      <th>FX_TXN_AMT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1138726</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>edrn/deoxt/qocdtkors/eudrch</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>qocdtkors</td>
      <td>eudrch</td>
      <td>edrn/deoxt/qocdtkors</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138729</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>edrn/deoxt/sguykcgs/cxstomgu</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>sguykcgs</td>
      <td>cxstomgu</td>
      <td>edrn/deoxt/sguykcgs</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138725</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>edrn/pgusordq</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138730</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>edrn/deoxt/sguykcgs/cxstomgu</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>sguykcgs</td>
      <td>cxstomgu</td>
      <td>edrn/deoxt/sguykcgs</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138724</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>edrn/pgusordq</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
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
abcde.shape
```




    (2209864, 18)




```python
behavior.shape
```




    (2209864, 8)




```python
df = abcde.iloc[:,np.r_[0,1,6,8,12,13,16]]
```

### 移除沒有3個階層以上的瀏覽行為


```python
df = df.loc[pd.notnull(abcde['4th'])]
```


```python
df.shape
```




    (1846122, 7)




```python
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>4th</th>
      <th>TXN_DT_x</th>
      <th>TXN_DT_y</th>
      <th>TXN_DT_x</th>
      <th>TXN_DT_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1138726</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>eudrch</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138729</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138730</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138731</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9532</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138727</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9532</td>
      <td>eudrch</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.shape
```




    (1846122, 7)




```python
df.columns = ['CUST_NO','VISITDATE','PAGE', 'WM_DT','CC_DT','LN_DT','FX_DT']
```


```python
df['WM'] = pd.notnull(df['WM_DT'])
```


```python
sum(df['WM'])
```




    182059




```python
df['CC'] = pd.notnull(df['CC_DT'])
```


```python
df['LN'] = pd.notnull(df['LN_DT'])
```


```python
df['FX'] = pd.notnull(df['FX_DT'])
```


```python
df.head()

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>FX</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1138726</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>eudrch</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1138729</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1138730</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1138731</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9532</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1138727</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9532</td>
      <td>eudrch</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['WM1'] = pd.notnull(df['WM_DT']) & pd.isnull(df['CC_DT']) & pd.isnull(df['LN_DT']) & pd.isnull(df['FX_DT']) 
df['CC1'] = pd.notnull(df['CC_DT']) & pd.isnull(df['WM_DT']) & pd.isnull(df['LN_DT']) & pd.isnull(df['FX_DT']) 
df['LN1'] = pd.notnull(df['LN_DT']) & pd.isnull(df['WM_DT']) & pd.isnull(df['CC_DT']) & pd.isnull(df['FX_DT']) 
df['FX1'] = pd.notnull(df['FX_DT']) & pd.isnull(df['WM_DT']) & pd.isnull(df['CC_DT']) & pd.isnull(df['LN_DT']) 
```


```python
sum(df['WM1'])
```




    35820




```python
abcde.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>4th</th>
      <th>1234PAGE</th>
      <th>TXN_DT_x</th>
      <th>CUST_RISK_CODE</th>
      <th>INVEST_TYPE_CODE</th>
      <th>WM_TXN_AMT</th>
      <th>TXN_DT_y</th>
      <th>TXN_DT_x</th>
      <th>LN_AMT</th>
      <th>LN_USE</th>
      <th>TXN_DT_y</th>
      <th>FX_TXN_AMT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1138726</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>edrn/deoxt/qocdtkors/eudrch</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>qocdtkors</td>
      <td>eudrch</td>
      <td>edrn/deoxt/qocdtkors</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138729</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>edrn/deoxt/sguykcgs/cxstomgu</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>sguykcgs</td>
      <td>cxstomgu</td>
      <td>edrn/deoxt/sguykcgs</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138725</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>edrn/pgusordq</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138730</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>edrn/deoxt/sguykcgs/cxstomgu</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>sguykcgs</td>
      <td>cxstomgu</td>
      <td>edrn/deoxt/sguykcgs</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1138724</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>edrn/pgusordq</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9458.0</td>
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
df['tWM2'] = np.where(df['WM1']==True & (df['WM_DT'] >= df['VISITDATE']),df['WM_DT']-df['VISITDATE'],'NaN')
df['tCC2'] = np.where(df['CC1']==True & (df['CC_DT'] >= df['VISITDATE']),df['CC_DT']-df['VISITDATE'],'NaN')
df['tLN2'] = np.where(df['LN1']==True & (df['LN_DT'] >= df['VISITDATE']),df['LN_DT']-df['VISITDATE'],'NaN')
df['tFX2'] = np.where(df['FX1']==True & (df['FX_DT'] >= df['VISITDATE']),df['FX_DT']-df['VISITDATE'],'NaN')
```


```python
df['WM2'] = np.where((df['WM1']==True) & (df['WM_DT'] >= df['VISITDATE']),True,False)
df['CC2'] = np.where((df['CC1']==True) & (df['CC_DT'] >= df['VISITDATE']),True,False)
df['LN2'] = np.where((df['LN1']==True) & (df['LN_DT'] >= df['VISITDATE']),True,False)
df['FX2'] = np.where((df['FX1']==True) & (df['FX_DT'] >= df['VISITDATE']),True,False)
```


```python
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>...</th>
      <th>LN1</th>
      <th>FX1</th>
      <th>tWM2</th>
      <th>tCC2</th>
      <th>tLN2</th>
      <th>tFX2</th>
      <th>WM2</th>
      <th>CC2</th>
      <th>LN2</th>
      <th>FX2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1138726</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9499</td>
      <td>eudrch</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>NaN</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1138729</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>NaN</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1138730</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9533</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>NaN</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1138731</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9532</td>
      <td>cxstomgu</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>NaN</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1138727</th>
      <td>---CHVW7DUN8SZLO</td>
      <td>9532</td>
      <td>eudrch</td>
      <td>NaN</td>
      <td>9458.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>NaN</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>




```python
df.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VISITDATE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1.846122e+06</td>
      <td>182059.000000</td>
      <td>192951.000000</td>
      <td>24839.000000</td>
      <td>987633.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>9.496579e+03</td>
      <td>9496.889497</td>
      <td>9503.761276</td>
      <td>9508.465437</td>
      <td>9489.209191</td>
    </tr>
    <tr>
      <th>std</th>
      <td>3.159564e+01</td>
      <td>35.662465</td>
      <td>34.831101</td>
      <td>34.824596</td>
      <td>35.430006</td>
    </tr>
    <tr>
      <th>min</th>
      <td>9.448000e+03</td>
      <td>9449.000000</td>
      <td>9448.000000</td>
      <td>9449.000000</td>
      <td>9448.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>9.470000e+03</td>
      <td>9466.000000</td>
      <td>9471.000000</td>
      <td>9473.000000</td>
      <td>9455.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>9.492000e+03</td>
      <td>9486.000000</td>
      <td>9505.000000</td>
      <td>9512.000000</td>
      <td>9480.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>9.522000e+03</td>
      <td>9526.000000</td>
      <td>9532.000000</td>
      <td>9537.000000</td>
      <td>9511.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>9.567000e+03</td>
      <td>9567.000000</td>
      <td>9567.000000</td>
      <td>9567.000000</td>
      <td>9567.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
wms = df.loc[df['WM1']==True,:]
ccs = df.loc[df['CC1']==True,:]
lns = df.loc[df['LN1']==True,:]
fxs = df.loc[df['FX1']==True,:]
```


```python
wms = df.loc[df['WM2']>0,:]
ccs = df.loc[df['CC2']>0,:]
lns = df.loc[df['LN2']>0,:]
fxs = df.loc[df['FX2']>0,:]
```


```python
wms.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>...</th>
      <th>LN1</th>
      <th>FX1</th>
      <th>tWM2</th>
      <th>tCC2</th>
      <th>tLN2</th>
      <th>tFX2</th>
      <th>WM2</th>
      <th>CC2</th>
      <th>LN2</th>
      <th>FX2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1375769</th>
      <td>-BVX1VPSBE5MUALM</td>
      <td>9490</td>
      <td>ixrfgdsa</td>
      <td>9514.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>24.0</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1375770</th>
      <td>-BVX1VPSBE5MUALM</td>
      <td>9514</td>
      <td>sxggrsfda</td>
      <td>9514.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>0.0</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>381923</th>
      <td>-CNIZ3EXFHYOEKVC</td>
      <td>9473</td>
      <td>fj</td>
      <td>9525.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>52.0</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1363958</th>
      <td>-DCWVKMZFOZFUV3U</td>
      <td>9478</td>
      <td>ixrf</td>
      <td>9532.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>54.0</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1926366</th>
      <td>-DCWVKMZFOZFUV3U</td>
      <td>9527</td>
      <td>iougkjr-sguykcg</td>
      <td>9532.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>5.0</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>




```python
df2 = df
```


```python
df2 = df2.sort_values('VISITDATE').drop_duplicates(subset=['CUST_NO', 'PAGE'], keep='last')
```


```python
df2.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>...</th>
      <th>LN1</th>
      <th>FX1</th>
      <th>tWM2</th>
      <th>tCC2</th>
      <th>tLN2</th>
      <th>tFX2</th>
      <th>WM2</th>
      <th>CC2</th>
      <th>LN2</th>
      <th>FX2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>279019</th>
      <td>3FWSWLVHAPPKITQW</td>
      <td>9448</td>
      <td>tudygq</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>18557</th>
      <td>YMGDL8VOR0TMRQZM</td>
      <td>9448</td>
      <td>fkscoxrt</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9507.0</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>True</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>59.0</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>144708</th>
      <td>6WOMO7-JWC94UG0G</td>
      <td>9448</td>
      <td>udtg</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>735601</th>
      <td>-WOAR7ZY1KJ7OTLG</td>
      <td>9448</td>
      <td>ugwduf</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>640439</th>
      <td>WXCWRMTGWBIBE7FY</td>
      <td>9448</td>
      <td>cugfkt-cduf</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>nan</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 23 columns</p>
</div>




```python
two = pd.crosstab(index=df["PAGE"], 
                           columns=(df["WM2"],df['CC2'],df['LN2'],df['FX2']),margins = True)
two
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>WM2</th>
      <th colspan="4" halign="left">False</th>
      <th>True</th>
      <th>All</th>
    </tr>
    <tr>
      <th>CC2</th>
      <th colspan="3" halign="left">False</th>
      <th>True</th>
      <th>False</th>
      <th></th>
    </tr>
    <tr>
      <th>LN2</th>
      <th colspan="2" halign="left">False</th>
      <th>True</th>
      <th>False</th>
      <th>False</th>
      <th></th>
    </tr>
    <tr>
      <th>FX2</th>
      <th>False</th>
      <th>True</th>
      <th>False</th>
      <th>False</th>
      <th>False</th>
      <th></th>
    </tr>
    <tr>
      <th>PAGE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10401</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1040325gcduf</th>
      <td>897</td>
      <td>36</td>
      <td>11</td>
      <td>422</td>
      <td>0</td>
      <td>1366</td>
    </tr>
    <tr>
      <th>1070101oygusgds</th>
      <td>156</td>
      <td>14</td>
      <td>1</td>
      <td>26</td>
      <td>1</td>
      <td>198</td>
    </tr>
    <tr>
      <th>1070101txktkor</th>
      <td>884</td>
      <td>43</td>
      <td>1</td>
      <td>23</td>
      <td>5</td>
      <td>956</td>
    </tr>
    <tr>
      <th>1070101txktkor_t</th>
      <td>52</td>
      <td>4</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>63</td>
    </tr>
    <tr>
      <th>1070101wdqqgt</th>
      <td>197</td>
      <td>37</td>
      <td>0</td>
      <td>23</td>
      <td>0</td>
      <td>257</td>
    </tr>
    <tr>
      <th>1070116chkrgsg</th>
      <td>350</td>
      <td>42</td>
      <td>5</td>
      <td>36</td>
      <td>3</td>
      <td>436</td>
    </tr>
    <tr>
      <th>1070120lce_1</th>
      <td>401</td>
      <td>25</td>
      <td>0</td>
      <td>42</td>
      <td>2</td>
      <td>470</td>
    </tr>
    <tr>
      <th>1070123schooq</th>
      <td>245</td>
      <td>21</td>
      <td>1</td>
      <td>14</td>
      <td>2</td>
      <td>283</td>
    </tr>
    <tr>
      <th>1070125moykg</th>
      <td>584</td>
      <td>22</td>
      <td>0</td>
      <td>36</td>
      <td>1</td>
      <td>643</td>
    </tr>
    <tr>
      <th>1070201pda_dppqg_i</th>
      <td>197</td>
      <td>27</td>
      <td>1</td>
      <td>8</td>
      <td>1</td>
      <td>234</td>
    </tr>
    <tr>
      <th>1070206g_dq</th>
      <td>132</td>
      <td>1</td>
      <td>0</td>
      <td>5</td>
      <td>0</td>
      <td>138</td>
    </tr>
    <tr>
      <th>1070208iugg</th>
      <td>29</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>32</td>
    </tr>
    <tr>
      <th>1070210pda_dppqg</th>
      <td>2405</td>
      <td>81</td>
      <td>6</td>
      <td>92</td>
      <td>3</td>
      <td>2587</td>
    </tr>
    <tr>
      <th>1070301pda_sdmsxrj_i</th>
      <td>72</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>74</td>
    </tr>
    <tr>
      <th>1070305fg_m</th>
      <td>28</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>28</td>
    </tr>
    <tr>
      <th>1070308ch</th>
      <td>37</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
      <td>0</td>
      <td>45</td>
    </tr>
    <tr>
      <th>1070309pda_joojqg</th>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2011pwf</th>
      <td>179</td>
      <td>19</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>202</td>
    </tr>
    <tr>
      <th>2011ssq</th>
      <td>467</td>
      <td>46</td>
      <td>2</td>
      <td>17</td>
      <td>0</td>
      <td>532</td>
    </tr>
    <tr>
      <th>201310gikrdrcg</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2013_sc</th>
      <td>7</td>
      <td>17</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>24</td>
    </tr>
    <tr>
      <th>201401shdug</th>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>20140401oex</th>
      <td>42</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>42</td>
    </tr>
    <tr>
      <th>2014_euic</th>
      <td>25</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>30</td>
    </tr>
    <tr>
      <th>2014_iougkjr</th>
      <td>71</td>
      <td>17</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>88</td>
    </tr>
    <tr>
      <th>2014cgutkikcdtg</th>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2014dcct-chkqf</th>
      <td>88</td>
      <td>6</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>98</td>
    </tr>
    <tr>
      <th>201511</th>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>201512</th>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>tkp4</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>tooqs</th>
      <td>6203</td>
      <td>323</td>
      <td>687</td>
      <td>298</td>
      <td>16</td>
      <td>7527</td>
    </tr>
    <tr>
      <th>totdq</th>
      <td>588</td>
      <td>64</td>
      <td>59</td>
      <td>30</td>
      <td>3</td>
      <td>744</td>
    </tr>
    <tr>
      <th>tudydq.htmq</th>
      <td>7</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>10</td>
    </tr>
    <tr>
      <th>tudygq</th>
      <td>5141</td>
      <td>3186</td>
      <td>15</td>
      <td>129</td>
      <td>81</td>
      <td>8552</td>
    </tr>
    <tr>
      <th>tudygq2.htmq</th>
      <td>18</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>28</td>
    </tr>
    <tr>
      <th>tudygqcduf</th>
      <td>2977</td>
      <td>134</td>
      <td>5</td>
      <td>61</td>
      <td>18</td>
      <td>3195</td>
    </tr>
    <tr>
      <th>tudygqgucduf</th>
      <td>378</td>
      <td>3</td>
      <td>0</td>
      <td>21</td>
      <td>0</td>
      <td>402</td>
    </tr>
    <tr>
      <th>tudygqss</th>
      <td>18</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>19</td>
    </tr>
    <tr>
      <th>twf sguykcg</th>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>twf-sguykcg</th>
      <td>260</td>
      <td>19</td>
      <td>0</td>
      <td>13</td>
      <td>2</td>
      <td>294</td>
    </tr>
    <tr>
      <th>twtudygq</th>
      <td>18</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>txuroygu</th>
      <td>187</td>
      <td>9</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>207</td>
    </tr>
    <tr>
      <th>ty_wge_shop.htmq</th>
      <td>52</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>56</td>
    </tr>
    <tr>
      <th>tzrsguykcg</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>udtg</th>
      <td>645564</td>
      <td>249693</td>
      <td>601</td>
      <td>6878</td>
      <td>8168</td>
      <td>910904</td>
    </tr>
    <tr>
      <th>ugcommgrf</th>
      <td>722</td>
      <td>46</td>
      <td>12</td>
      <td>105</td>
      <td>0</td>
      <td>885</td>
    </tr>
    <tr>
      <th>ugjkstudtkor</th>
      <td>12147</td>
      <td>576</td>
      <td>357</td>
      <td>449</td>
      <td>60</td>
      <td>13589</td>
    </tr>
    <tr>
      <th>ugtkugmgrt</th>
      <td>19</td>
      <td>4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>ugwduf</th>
      <td>108237</td>
      <td>6620</td>
      <td>647</td>
      <td>3389</td>
      <td>1290</td>
      <td>120183</td>
    </tr>
    <tr>
      <th>wdqqgt</th>
      <td>144</td>
      <td>6</td>
      <td>1</td>
      <td>7</td>
      <td>0</td>
      <td>158</td>
    </tr>
    <tr>
      <th>wgdqth</th>
      <td>2300</td>
      <td>39</td>
      <td>0</td>
      <td>13</td>
      <td>517</td>
      <td>2869</td>
    </tr>
    <tr>
      <th>wge</th>
      <td>95</td>
      <td>19</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>118</td>
    </tr>
    <tr>
      <th>wgffkrj</th>
      <td>294</td>
      <td>22</td>
      <td>25</td>
      <td>18</td>
      <td>0</td>
      <td>359</td>
    </tr>
    <tr>
      <th>wouqfcduf</th>
      <td>5178</td>
      <td>715</td>
      <td>13</td>
      <td>319</td>
      <td>121</td>
      <td>6346</td>
    </tr>
    <tr>
      <th>xs-stocn</th>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>11</td>
    </tr>
    <tr>
      <th>ykfgo</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>yykp</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>zmds</th>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>All</th>
      <td>1428761</td>
      <td>343335</td>
      <td>12326</td>
      <td>44714</td>
      <td>16986</td>
      <td>1846122</td>
    </tr>
  </tbody>
</table>
<p>307 rows × 6 columns</p>
</div>




```python
final = two
final.columns = ['None', 'FX_DT','LN_DT','CC_DT','WM_DT','All']

```


```python
final['Page4'] =final.index
final.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>None</th>
      <th>FX_DT</th>
      <th>LN_DT</th>
      <th>CC_DT</th>
      <th>WM_DT</th>
      <th>All</th>
      <th>Page4</th>
    </tr>
    <tr>
      <th>PAGE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10401</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>10401</td>
    </tr>
    <tr>
      <th>1040325gcduf</th>
      <td>897</td>
      <td>36</td>
      <td>11</td>
      <td>422</td>
      <td>0</td>
      <td>1366</td>
      <td>1040325gcduf</td>
    </tr>
    <tr>
      <th>1070101oygusgds</th>
      <td>156</td>
      <td>14</td>
      <td>1</td>
      <td>26</td>
      <td>1</td>
      <td>198</td>
      <td>1070101oygusgds</td>
    </tr>
    <tr>
      <th>1070101txktkor</th>
      <td>884</td>
      <td>43</td>
      <td>1</td>
      <td>23</td>
      <td>5</td>
      <td>956</td>
      <td>1070101txktkor</td>
    </tr>
    <tr>
      <th>1070101txktkor_t</th>
      <td>52</td>
      <td>4</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>63</td>
      <td>1070101txktkor_t</td>
    </tr>
  </tbody>
</table>
</div>




```python
final['WM_P'] = final['WM_DT']/final['All']
final['CC_P'] = final['CC_DT']/final['All']
final['LN_P'] = final['LN_DT']/final['All']
final['FX_P'] = final['FX_DT']/final['All']
final['conversion_P'] = 1-final['None']/final['All']
final.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>None</th>
      <th>FX_DT</th>
      <th>LN_DT</th>
      <th>CC_DT</th>
      <th>WM_DT</th>
      <th>All</th>
      <th>Page4</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
    </tr>
    <tr>
      <th>PAGE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10401</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>10401</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>1040325gcduf</th>
      <td>897</td>
      <td>36</td>
      <td>11</td>
      <td>422</td>
      <td>0</td>
      <td>1366</td>
      <td>1040325gcduf</td>
      <td>0.000000</td>
      <td>0.308931</td>
      <td>0.008053</td>
      <td>0.026354</td>
      <td>0.343338</td>
    </tr>
    <tr>
      <th>1070101oygusgds</th>
      <td>156</td>
      <td>14</td>
      <td>1</td>
      <td>26</td>
      <td>1</td>
      <td>198</td>
      <td>1070101oygusgds</td>
      <td>0.005051</td>
      <td>0.131313</td>
      <td>0.005051</td>
      <td>0.070707</td>
      <td>0.212121</td>
    </tr>
    <tr>
      <th>1070101txktkor</th>
      <td>884</td>
      <td>43</td>
      <td>1</td>
      <td>23</td>
      <td>5</td>
      <td>956</td>
      <td>1070101txktkor</td>
      <td>0.005230</td>
      <td>0.024059</td>
      <td>0.001046</td>
      <td>0.044979</td>
      <td>0.075314</td>
    </tr>
    <tr>
      <th>1070101txktkor_t</th>
      <td>52</td>
      <td>4</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
      <td>63</td>
      <td>1070101txktkor_t</td>
      <td>0.015873</td>
      <td>0.095238</td>
      <td>0.000000</td>
      <td>0.063492</td>
      <td>0.174603</td>
    </tr>
  </tbody>
</table>
</div>




```python
PAGE_prob = final[['Page4','WM_P','CC_P','LN_P','FX_P','conversion_P']]
```


```python
y = pd.read_csv('TBN_Y_ZERO.csv')
```


```python
behavior1 = behavior
behavior1['Page4'] = np.where(pd.isnull(behavior1['4th']), behavior1['3rd'], behavior1['4th'])
y_behavior = pd.merge(behavior1, y, how='right', on='CUST_NO')
```


```python
y_behavior.head(20)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>4th</th>
      <th>1234PAGE</th>
      <th>Page4</th>
      <th>CC_IND</th>
      <th>FX_IND</th>
      <th>LN_IND</th>
      <th>WM_IND</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9480</td>
      <td>gygrt/qodr/fj/</td>
      <td>gygrt</td>
      <td>qodr</td>
      <td>fj</td>
      <td>NaN</td>
      <td>gygrt/qodr/fj</td>
      <td>fj</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shopkrio</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shopkrio</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shopkrio</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shopkrio</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shops</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shops</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shops</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shops</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shopkrio</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shopkrio</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shops/tudygq</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shops/tudygq</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9535</td>
      <td>edrn/deoxt/drroxrcgmgrt/drroxrcgmgrt</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>drroxrcgmgrt</td>
      <td>drroxrcgmgrt</td>
      <td>edrn/deoxt/drroxrcgmgrt</td>
      <td>drroxrcgmgrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9537</td>
      <td>edrn/deoxt/drroxrcgmgrt/drroxrcgmgrt</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>drroxrcgmgrt</td>
      <td>drroxrcgmgrt</td>
      <td>edrn/deoxt/drroxrcgmgrt</td>
      <td>drroxrcgmgrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9535</td>
      <td>gygrt/mgmegutgdm/sxggr/</td>
      <td>gygrt</td>
      <td>mgmegutgdm</td>
      <td>sxggr</td>
      <td>NaN</td>
      <td>gygrt/mgmegutgdm/sxggr</td>
      <td>sxggr</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9483</td>
      <td>gygrt/e2c/iougkjr/</td>
      <td>gygrt</td>
      <td>e2c</td>
      <td>iougkjr</td>
      <td>NaN</td>
      <td>gygrt/e2c/iougkjr</td>
      <td>iougkjr</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9483</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>udtg</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9469</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>udtg</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9452</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>udtg</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
y_behavior['CUST_NO'].nunique()
```




    30000




```python
y_score = pd.merge(y_behavior,PAGE_prob,how = 'left',on = 'Page4')
```


```python
y_score2 = y_score.drop_duplicates(subset=['CUST_NO', 'Page4'])
```


```python
y_score2.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>4th</th>
      <th>1234PAGE</th>
      <th>Page4</th>
      <th>CC_IND</th>
      <th>FX_IND</th>
      <th>LN_IND</th>
      <th>WM_IND</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9480</td>
      <td>gygrt/qodr/fj/</td>
      <td>gygrt</td>
      <td>qodr</td>
      <td>fj</td>
      <td>NaN</td>
      <td>gygrt/qodr/fj</td>
      <td>fj</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.003556</td>
      <td>0.122826</td>
      <td>0.070842</td>
      <td>0.035691</td>
      <td>0.232914</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>9559</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt/shopkrio</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>cugfkt-cduf</td>
      <td>fkscoxrt</td>
      <td>edrn/pgusordq/cugfkt-cduf/fkscoxrt</td>
      <td>fkscoxrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.004323</td>
      <td>0.036252</td>
      <td>0.001631</td>
      <td>0.043691</td>
      <td>0.085897</td>
    </tr>
    <tr>
      <th>13</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9535</td>
      <td>edrn/deoxt/drroxrcgmgrt/drroxrcgmgrt</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>drroxrcgmgrt</td>
      <td>drroxrcgmgrt</td>
      <td>edrn/deoxt/drroxrcgmgrt</td>
      <td>drroxrcgmgrt</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.009329</td>
      <td>0.027723</td>
      <td>0.002427</td>
      <td>0.163386</td>
      <td>0.202866</td>
    </tr>
    <tr>
      <th>15</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9535</td>
      <td>gygrt/mgmegutgdm/sxggr/</td>
      <td>gygrt</td>
      <td>mgmegutgdm</td>
      <td>sxggr</td>
      <td>NaN</td>
      <td>gygrt/mgmegutgdm/sxggr</td>
      <td>sxggr</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.008086</td>
      <td>0.008985</td>
      <td>0.002995</td>
      <td>0.052111</td>
      <td>0.072177</td>
    </tr>
    <tr>
      <th>16</th>
      <td>JDVF4U8JUANEID68</td>
      <td>9483</td>
      <td>gygrt/e2c/iougkjr/</td>
      <td>gygrt</td>
      <td>e2c</td>
      <td>iougkjr</td>
      <td>NaN</td>
      <td>gygrt/e2c/iougkjr</td>
      <td>iougkjr</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.010176</td>
      <td>0.007717</td>
      <td>0.000538</td>
      <td>0.263209</td>
      <td>0.281640</td>
    </tr>
  </tbody>
</table>
</div>




```python
y_score2 = y_score2.groupby(by = 'CUST_NO')
```


```python
f_y = y_score2.sum()
```


```python
two = pd.crosstab(index=df["PAGE"], 
                           columns=(df["WM1"],df['CC1'],df['LN1'],df['FX1']))
two
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>WM1</th>
      <th colspan="4" halign="left">False</th>
      <th>True</th>
    </tr>
    <tr>
      <th>CC1</th>
      <th colspan="3" halign="left">False</th>
      <th>True</th>
      <th>False</th>
    </tr>
    <tr>
      <th>LN1</th>
      <th colspan="2" halign="left">False</th>
      <th>True</th>
      <th>False</th>
      <th>False</th>
    </tr>
    <tr>
      <th>FX1</th>
      <th>False</th>
      <th>True</th>
      <th>False</th>
      <th>False</th>
      <th>False</th>
    </tr>
    <tr>
      <th>PAGE</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10401</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1040325gcduf</th>
      <td>706</td>
      <td>112</td>
      <td>14</td>
      <td>534</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1070101oygusgds</th>
      <td>122</td>
      <td>35</td>
      <td>1</td>
      <td>38</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1070101txktkor</th>
      <td>759</td>
      <td>110</td>
      <td>3</td>
      <td>72</td>
      <td>12</td>
    </tr>
    <tr>
      <th>1070101txktkor_t</th>
      <td>49</td>
      <td>5</td>
      <td>0</td>
      <td>8</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1070101wdqqgt</th>
      <td>150</td>
      <td>64</td>
      <td>0</td>
      <td>43</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1070116chkrgsg</th>
      <td>275</td>
      <td>94</td>
      <td>7</td>
      <td>57</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1070120lce_1</th>
      <td>278</td>
      <td>85</td>
      <td>2</td>
      <td>103</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1070123schooq</th>
      <td>189</td>
      <td>56</td>
      <td>2</td>
      <td>31</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1070125moykg</th>
      <td>465</td>
      <td>80</td>
      <td>2</td>
      <td>91</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1070201pda_dppqg_i</th>
      <td>127</td>
      <td>75</td>
      <td>2</td>
      <td>27</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1070206g_dq</th>
      <td>108</td>
      <td>12</td>
      <td>0</td>
      <td>18</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1070208iugg</th>
      <td>24</td>
      <td>6</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1070210pda_dppqg</th>
      <td>1743</td>
      <td>498</td>
      <td>11</td>
      <td>318</td>
      <td>17</td>
    </tr>
    <tr>
      <th>1070301pda_sdmsxrj_i</th>
      <td>56</td>
      <td>9</td>
      <td>0</td>
      <td>9</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1070305fg_m</th>
      <td>19</td>
      <td>7</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1070308ch</th>
      <td>24</td>
      <td>8</td>
      <td>0</td>
      <td>13</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1070309pda_joojqg</th>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2011pwf</th>
      <td>161</td>
      <td>23</td>
      <td>2</td>
      <td>16</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2011ssq</th>
      <td>414</td>
      <td>77</td>
      <td>6</td>
      <td>32</td>
      <td>3</td>
    </tr>
    <tr>
      <th>201310gikrdrcg</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2013_sc</th>
      <td>7</td>
      <td>17</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>201401shdug</th>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20140401oex</th>
      <td>42</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2014_euic</th>
      <td>17</td>
      <td>13</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2014_iougkjr</th>
      <td>46</td>
      <td>40</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2014cgutkikcdtg</th>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2014dcct-chkqf</th>
      <td>56</td>
      <td>27</td>
      <td>3</td>
      <td>11</td>
      <td>1</td>
    </tr>
    <tr>
      <th>201511</th>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>201512</th>
      <td>4</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>tkp2</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tkp4</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tooqs</th>
      <td>5293</td>
      <td>799</td>
      <td>812</td>
      <td>586</td>
      <td>37</td>
    </tr>
    <tr>
      <th>totdq</th>
      <td>521</td>
      <td>113</td>
      <td>62</td>
      <td>44</td>
      <td>4</td>
    </tr>
    <tr>
      <th>tudydq.htmq</th>
      <td>5</td>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tudygq</th>
      <td>4111</td>
      <td>4125</td>
      <td>18</td>
      <td>195</td>
      <td>103</td>
    </tr>
    <tr>
      <th>tudygq2.htmq</th>
      <td>12</td>
      <td>16</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tudygqcduf</th>
      <td>2454</td>
      <td>544</td>
      <td>8</td>
      <td>164</td>
      <td>25</td>
    </tr>
    <tr>
      <th>tudygqgucduf</th>
      <td>295</td>
      <td>28</td>
      <td>7</td>
      <td>71</td>
      <td>1</td>
    </tr>
    <tr>
      <th>tudygqss</th>
      <td>13</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>twf sguykcg</th>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>twf-sguykcg</th>
      <td>225</td>
      <td>40</td>
      <td>0</td>
      <td>22</td>
      <td>7</td>
    </tr>
    <tr>
      <th>twtudygq</th>
      <td>15</td>
      <td>5</td>
      <td>0</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>txuroygu</th>
      <td>162</td>
      <td>27</td>
      <td>4</td>
      <td>11</td>
      <td>3</td>
    </tr>
    <tr>
      <th>ty_wge_shop.htmq</th>
      <td>48</td>
      <td>6</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>tzrsguykcg</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>udtg</th>
      <td>315827</td>
      <td>562981</td>
      <td>743</td>
      <td>11125</td>
      <td>20228</td>
    </tr>
    <tr>
      <th>ugcommgrf</th>
      <td>534</td>
      <td>101</td>
      <td>15</td>
      <td>233</td>
      <td>2</td>
    </tr>
    <tr>
      <th>ugjkstudtkor</th>
      <td>10763</td>
      <td>1482</td>
      <td>449</td>
      <td>757</td>
      <td>138</td>
    </tr>
    <tr>
      <th>ugtkugmgrt</th>
      <td>8</td>
      <td>15</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>ugwduf</th>
      <td>97478</td>
      <td>13753</td>
      <td>1009</td>
      <td>5354</td>
      <td>2589</td>
    </tr>
    <tr>
      <th>wdqqgt</th>
      <td>110</td>
      <td>16</td>
      <td>1</td>
      <td>31</td>
      <td>0</td>
    </tr>
    <tr>
      <th>wgdqth</th>
      <td>1661</td>
      <td>216</td>
      <td>1</td>
      <td>38</td>
      <td>953</td>
    </tr>
    <tr>
      <th>wge</th>
      <td>72</td>
      <td>41</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>wgffkrj</th>
      <td>268</td>
      <td>34</td>
      <td>30</td>
      <td>25</td>
      <td>2</td>
    </tr>
    <tr>
      <th>wouqfcduf</th>
      <td>3971</td>
      <td>1558</td>
      <td>21</td>
      <td>578</td>
      <td>218</td>
    </tr>
    <tr>
      <th>xs-stocn</th>
      <td>8</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>ykfgo</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>yykp</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>zmds</th>
      <td>6</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>306 rows × 5 columns</p>
</div>




```python
y_behavior.loc[pd.notnull(y_behavior['Page4'])]['CUST_NO'].nunique()
```




    28347




```python
wm_behavior = pd.merge(behavior1, b, how='right', on='CUST_NO')
```


```python
behavior1.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>1st</th>
      <th>2nd</th>
      <th>3rd</th>
      <th>4th</th>
      <th>1234PAGE</th>
      <th>Page4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AZTHNWQ_LXMGIMYG</td>
      <td>9462</td>
      <td>gygrt/e2c/iougkjr/</td>
      <td>gygrt</td>
      <td>e2c</td>
      <td>iougkjr</td>
      <td>NaN</td>
      <td>gygrt/e2c/iougkjr</td>
      <td>iougkjr</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AZTHNWQ_LXMGIMYG</td>
      <td>9528</td>
      <td>gygrt/wgdqth/gsxrioof/</td>
      <td>gygrt</td>
      <td>wgdqth</td>
      <td>gsxrioof</td>
      <td>NaN</td>
      <td>gygrt/wgdqth/gsxrioof</td>
      <td>gsxrioof</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3PY428CHUQBULFIG</td>
      <td>9458</td>
      <td>edrn/deoxt/rgws-cgrtgu</td>
      <td>edrn</td>
      <td>deoxt</td>
      <td>rgws-cgrtgu</td>
      <td>NaN</td>
      <td>edrn/deoxt/rgws-cgrtgu</td>
      <td>rgws-cgrtgu</td>
    </tr>
    <tr>
      <th>3</th>
      <td>JVPD1QUJWVLMZU8S</td>
      <td>9457</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>udtg</td>
    </tr>
    <tr>
      <th>4</th>
      <td>JVPD1QUJWVLMZU8S</td>
      <td>9485</td>
      <td>edrn/pgusordq/fgposkt/udtg/iougz/gzchdrjg-udtg...</td>
      <td>edrn</td>
      <td>pgusordq</td>
      <td>fgposkt</td>
      <td>udtg</td>
      <td>edrn/pgusordq/fgposkt/udtg</td>
      <td>udtg</td>
    </tr>
  </tbody>
</table>
</div>




```python
wm_score = pd.merge(wms,PAGE_prob,how = 'left',left_on='PAGE', right_on = 'Page4')
```


```python
wm_score.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>VISITDATE</th>
      <th>PAGE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>...</th>
      <th>WM2</th>
      <th>CC2</th>
      <th>LN2</th>
      <th>FX2</th>
      <th>Page4</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-BVX1VPSBE5MUALM</td>
      <td>9490</td>
      <td>ixrfgdsa</td>
      <td>9514.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>ixrfgdsa</td>
      <td>0.093185</td>
      <td>0.018081</td>
      <td>0.001391</td>
      <td>0.116829</td>
      <td>0.229485</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-BVX1VPSBE5MUALM</td>
      <td>9514</td>
      <td>sxggrsfda</td>
      <td>9514.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>sxggrsfda</td>
      <td>0.006974</td>
      <td>0.021727</td>
      <td>0.002146</td>
      <td>0.046942</td>
      <td>0.077790</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-CNIZ3EXFHYOEKVC</td>
      <td>9473</td>
      <td>fj</td>
      <td>9525.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>fj</td>
      <td>0.003556</td>
      <td>0.122826</td>
      <td>0.070842</td>
      <td>0.035691</td>
      <td>0.232914</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-DCWVKMZFOZFUV3U</td>
      <td>9478</td>
      <td>ixrf</td>
      <td>9532.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>ixrf</td>
      <td>0.101665</td>
      <td>0.005841</td>
      <td>0.000553</td>
      <td>0.084458</td>
      <td>0.192517</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-DCWVKMZFOZFUV3U</td>
      <td>9527</td>
      <td>iougkjr-sguykcg</td>
      <td>9532.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>iougkjr-sguykcg</td>
      <td>0.008882</td>
      <td>0.007627</td>
      <td>0.000418</td>
      <td>0.324097</td>
      <td>0.341025</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 29 columns</p>
</div>




```python
wm_score2 = wm_score.groupby(by = 'CUST_NO')
```


```python
wm_score3 = wm_score2.sum()
```


```python
wm_score3.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VISITDATE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>FX</th>
      <th>WM1</th>
      <th>...</th>
      <th>FX1</th>
      <th>WM2</th>
      <th>CC2</th>
      <th>LN2</th>
      <th>FX2</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1.678000e+03</td>
      <td>1.678000e+03</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1678.000000</td>
      <td>1678.0</td>
      <td>1678.0</td>
      <td>1678.0</td>
      <td>1678.000000</td>
      <td>...</td>
      <td>1678.0</td>
      <td>1678.000000</td>
      <td>1678.0</td>
      <td>1678.0</td>
      <td>1678.0</td>
      <td>1678.000000</td>
      <td>1678.000000</td>
      <td>1678.000000</td>
      <td>1678.000000</td>
      <td>1678.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>9.599788e+04</td>
      <td>9.632808e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10.122765</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>10.122765</td>
      <td>...</td>
      <td>0.0</td>
      <td>10.122765</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.248150</td>
      <td>0.169506</td>
      <td>0.033834</td>
      <td>1.915792</td>
      <td>2.367282</td>
    </tr>
    <tr>
      <th>std</th>
      <td>2.773868e+05</td>
      <td>2.784577e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29.253266</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>29.253266</td>
      <td>...</td>
      <td>0.0</td>
      <td>29.253266</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.660657</td>
      <td>0.338232</td>
      <td>0.078777</td>
      <td>7.503870</td>
      <td>8.117361</td>
    </tr>
    <tr>
      <th>min</th>
      <td>9.448000e+03</td>
      <td>9.449000e+03</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>...</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.001754</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.013594</td>
      <td>0.034072</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>9.517000e+03</td>
      <td>9.553000e+03</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>...</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.015446</td>
      <td>0.023035</td>
      <td>0.001979</td>
      <td>0.112669</td>
      <td>0.246191</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2.842000e+04</td>
      <td>2.851350e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>3.000000</td>
      <td>...</td>
      <td>0.0</td>
      <td>3.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.072559</td>
      <td>0.060754</td>
      <td>0.007959</td>
      <td>0.300421</td>
      <td>0.566156</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>6.643450e+04</td>
      <td>6.662600e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>7.000000</td>
      <td>...</td>
      <td>0.0</td>
      <td>7.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.235912</td>
      <td>0.162056</td>
      <td>0.028037</td>
      <td>0.908208</td>
      <td>1.395249</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5.011701e+06</td>
      <td>5.040288e+06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>528.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>528.000000</td>
      <td>...</td>
      <td>0.0</td>
      <td>528.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>15.281735</td>
      <td>4.486846</td>
      <td>1.125480</td>
      <td>141.528750</td>
      <td>151.433477</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 22 columns</p>
</div>




```python
cc_score = pd.merge(ccs,PAGE_prob,how = 'left',left_on='PAGE', right_on = 'Page4')
cc_score2 = cc_score.groupby(by = 'CUST_NO')
cc_score3 = cc_score2.sum()
cc_score3.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VISITDATE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>FX</th>
      <th>WM1</th>
      <th>...</th>
      <th>FX1</th>
      <th>WM2</th>
      <th>CC2</th>
      <th>LN2</th>
      <th>FX2</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>8.947000e+03</td>
      <td>0.0</td>
      <td>8.947000e+03</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>8947.0</td>
      <td>8947.000000</td>
      <td>8947.0</td>
      <td>8947.0</td>
      <td>8947.0</td>
      <td>...</td>
      <td>8947.0</td>
      <td>8947.0</td>
      <td>8947.000000</td>
      <td>8947.0</td>
      <td>8947.0</td>
      <td>8947.000000</td>
      <td>8947.000000</td>
      <td>8947.000000</td>
      <td>8947.000000</td>
      <td>8947.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4.745228e+04</td>
      <td>NaN</td>
      <td>4.757346e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>4.997653</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.997653</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.031791</td>
      <td>0.300759</td>
      <td>0.082642</td>
      <td>0.478090</td>
      <td>0.893281</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.774237e+05</td>
      <td>NaN</td>
      <td>1.781459e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>18.685511</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>18.685511</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.169150</td>
      <td>0.482698</td>
      <td>0.165179</td>
      <td>4.589241</td>
      <td>4.975517</td>
    </tr>
    <tr>
      <th>min</th>
      <td>9.448000e+03</td>
      <td>NaN</td>
      <td>9.448000e+03</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.003406</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.021818</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>9.506000e+03</td>
      <td>NaN</td>
      <td>9.520000e+03</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.003556</td>
      <td>0.109999</td>
      <td>0.009688</td>
      <td>0.035691</td>
      <td>0.232914</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.900400e+04</td>
      <td>NaN</td>
      <td>1.904200e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>2.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.009329</td>
      <td>0.144553</td>
      <td>0.070842</td>
      <td>0.109799</td>
      <td>0.363024</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>4.727500e+04</td>
      <td>NaN</td>
      <td>4.731500e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>5.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>5.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.023389</td>
      <td>0.321116</td>
      <td>0.079705</td>
      <td>0.287026</td>
      <td>0.748398</td>
    </tr>
    <tr>
      <th>max</th>
      <td>8.450581e+06</td>
      <td>NaN</td>
      <td>8.460864e+06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>888.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>888.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>7.739887</td>
      <td>9.164566</td>
      <td>6.095959</td>
      <td>243.637779</td>
      <td>259.658167</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 22 columns</p>
</div>




```python
ln_score = pd.merge(lns,PAGE_prob,how = 'left',left_on='PAGE', right_on = 'Page4')
ln_score2 = ln_score.groupby(by = 'CUST_NO')
ln_score3 = ln_score2.sum()
ln_score3.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VISITDATE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>FX</th>
      <th>WM1</th>
      <th>...</th>
      <th>FX1</th>
      <th>WM2</th>
      <th>CC2</th>
      <th>LN2</th>
      <th>FX2</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1.801000e+03</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.801000e+03</td>
      <td>0.0</td>
      <td>1801.0</td>
      <td>1801.0</td>
      <td>1801.000000</td>
      <td>1801.0</td>
      <td>1801.0</td>
      <td>...</td>
      <td>1801.0</td>
      <td>1801.0</td>
      <td>1801.0</td>
      <td>1801.000000</td>
      <td>1801.0</td>
      <td>1801.000000</td>
      <td>1801.000000</td>
      <td>1801.000000</td>
      <td>1801.000000</td>
      <td>1801.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>6.497970e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>6.510301e+04</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>6.843976</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>6.843976</td>
      <td>0.0</td>
      <td>0.031523</td>
      <td>0.410549</td>
      <td>0.384582</td>
      <td>0.400626</td>
      <td>1.227280</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1.157024e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.159581e+05</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.185491</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>12.185491</td>
      <td>0.0</td>
      <td>0.101560</td>
      <td>0.518636</td>
      <td>0.487607</td>
      <td>2.074371</td>
      <td>2.659701</td>
    </tr>
    <tr>
      <th>min</th>
      <td>9.448000e+03</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.449000e+03</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>0.007551</td>
      <td>0.000418</td>
      <td>0.011338</td>
      <td>0.068145</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.893800e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.896600e+04</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.000000</td>
      <td>0.0</td>
      <td>0.006265</td>
      <td>0.122826</td>
      <td>0.082653</td>
      <td>0.071382</td>
      <td>0.332180</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.785200e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.788000e+04</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.000000</td>
      <td>0.0</td>
      <td>0.013142</td>
      <td>0.245651</td>
      <td>0.221371</td>
      <td>0.152141</td>
      <td>0.685044</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>7.586000e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.608800e+04</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>8.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>8.000000</td>
      <td>0.0</td>
      <td>0.029996</td>
      <td>0.492207</td>
      <td>0.487694</td>
      <td>0.354523</td>
      <td>1.425022</td>
    </tr>
    <tr>
      <th>max</th>
      <td>3.266806e+06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.272128e+06</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>344.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>344.000000</td>
      <td>0.0</td>
      <td>3.331791</td>
      <td>4.784771</td>
      <td>4.528406</td>
      <td>79.709687</td>
      <td>91.100375</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 22 columns</p>
</div>




```python
fx_score = pd.merge(fxs,PAGE_prob,how = 'left',left_on='PAGE', right_on = 'Page4')
fx_score2 = fx_score.groupby(by = 'CUST_NO')
fx_score3 = fx_score2.sum()
fx_score3.describe()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VISITDATE</th>
      <th>WM_DT</th>
      <th>CC_DT</th>
      <th>LN_DT</th>
      <th>FX_DT</th>
      <th>WM</th>
      <th>CC</th>
      <th>LN</th>
      <th>FX</th>
      <th>WM1</th>
      <th>...</th>
      <th>FX1</th>
      <th>WM2</th>
      <th>CC2</th>
      <th>LN2</th>
      <th>FX2</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1.850200e+04</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.850200e+04</td>
      <td>18502.0</td>
      <td>18502.0</td>
      <td>18502.0</td>
      <td>18502.000000</td>
      <td>18502.0</td>
      <td>...</td>
      <td>18502.000000</td>
      <td>18502.0</td>
      <td>18502.0</td>
      <td>18502.0</td>
      <td>18502.000000</td>
      <td>18502.000000</td>
      <td>18502.000000</td>
      <td>18502.000000</td>
      <td>18502.000000</td>
      <td>18502.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.759338e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.764695e+05</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>18.556643</td>
      <td>0.0</td>
      <td>...</td>
      <td>18.556643</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>18.556643</td>
      <td>0.173749</td>
      <td>0.231189</td>
      <td>0.038997</td>
      <td>4.620808</td>
      <td>5.064743</td>
    </tr>
    <tr>
      <th>std</th>
      <td>8.904658e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.933025e+05</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>93.928011</td>
      <td>0.0</td>
      <td>...</td>
      <td>93.928011</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>93.928011</td>
      <td>0.869949</td>
      <td>0.775170</td>
      <td>0.139410</td>
      <td>25.589147</td>
      <td>27.237972</td>
    </tr>
    <tr>
      <th>min</th>
      <td>9.448000e+03</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>9.449000e+03</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>...</td>
      <td>1.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.011338</td>
      <td>0.034072</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.889800e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.890200e+04</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.000000</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.000000</td>
      <td>0.011314</td>
      <td>0.030203</td>
      <td>0.002495</td>
      <td>0.274116</td>
      <td>0.341025</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.785900e+04</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>3.791600e+04</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.000000</td>
      <td>0.0</td>
      <td>...</td>
      <td>4.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4.000000</td>
      <td>0.033356</td>
      <td>0.075490</td>
      <td>0.009014</td>
      <td>0.799502</td>
      <td>0.923611</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>1.041588e+05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.043460e+05</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>11.000000</td>
      <td>0.0</td>
      <td>...</td>
      <td>11.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>11.000000</td>
      <td>0.099664</td>
      <td>0.197708</td>
      <td>0.030530</td>
      <td>2.356311</td>
      <td>2.695600</td>
    </tr>
    <tr>
      <th>max</th>
      <td>4.374563e+07</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.377473e+07</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4621.000000</td>
      <td>0.0</td>
      <td>...</td>
      <td>4621.000000</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>4621.000000</td>
      <td>41.440177</td>
      <td>34.952109</td>
      <td>11.717077</td>
      <td>1266.614866</td>
      <td>1346.061937</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 22 columns</p>
</div>




```python
fx_thres = fx_score3.describe().FX_P[5]
wm_thres = wm_score3.describe().WM_P[5]
ln_thres = ln_score3.describe().LN_P[5]
cc_thres = cc_score3.describe().CC_P[5]
```


```python
f_y.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VISITDATE</th>
      <th>CC_IND</th>
      <th>FX_IND</th>
      <th>LN_IND</th>
      <th>WM_IND</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
    </tr>
    <tr>
      <th>CUST_NO</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>--JQX2SAA6ICQYR8</th>
      <td>19085</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.002897</td>
      <td>0.109999</td>
      <td>0.004844</td>
      <td>0.027455</td>
      <td>0.145195</td>
    </tr>
    <tr>
      <th>--JVR0JBCXHAROP8</th>
      <td>19122</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.001754</td>
      <td>0.019391</td>
      <td>0.001378</td>
      <td>0.016510</td>
      <td>0.039033</td>
    </tr>
    <tr>
      <th>--NQCBUYH8AY5GJW</th>
      <td>9566</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.007758</td>
      <td>0.037030</td>
      <td>0.011812</td>
      <td>0.073510</td>
      <td>0.130110</td>
    </tr>
    <tr>
      <th>--OR0BOBUBUTKZFC</th>
      <td>66511</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.027841</td>
      <td>0.244374</td>
      <td>0.087476</td>
      <td>0.247294</td>
      <td>0.606985</td>
    </tr>
    <tr>
      <th>--VNUI5BC0EIGK9A</th>
      <td>47592</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.027070</td>
      <td>0.068670</td>
      <td>0.013073</td>
      <td>0.923049</td>
      <td>1.031863</td>
    </tr>
  </tbody>
</table>
</div>




```python
wm_thres
```




    0.072559068237501523




```python
cc_thres
```




    0.14455304109468378




```python
f_y['f_WM'] = np.where(f_y['WM_P']>wm_thres,1,0)
f_y['f_CC'] = np.where(f_y['CC_P']>cc_thres,1,0)
f_y['f_LN'] = np.where(f_y['LN_P']>ln_thres,1,0)
f_y['f_FX'] = np.where(f_y['FX_P']>fx_thres,1,0)

```


```python
f_y.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>VISITDATE</th>
      <th>CC_IND</th>
      <th>FX_IND</th>
      <th>LN_IND</th>
      <th>WM_IND</th>
      <th>WM_P</th>
      <th>CC_P</th>
      <th>LN_P</th>
      <th>FX_P</th>
      <th>conversion_P</th>
      <th>f_WM</th>
      <th>f_CC</th>
      <th>f_LN</th>
      <th>f_FX</th>
    </tr>
    <tr>
      <th>CUST_NO</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>--JQX2SAA6ICQYR8</th>
      <td>19085</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.002897</td>
      <td>0.109999</td>
      <td>0.004844</td>
      <td>0.027455</td>
      <td>0.145195</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>--JVR0JBCXHAROP8</th>
      <td>19122</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.001754</td>
      <td>0.019391</td>
      <td>0.001378</td>
      <td>0.016510</td>
      <td>0.039033</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>--NQCBUYH8AY5GJW</th>
      <td>9566</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.007758</td>
      <td>0.037030</td>
      <td>0.011812</td>
      <td>0.073510</td>
      <td>0.130110</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>--OR0BOBUBUTKZFC</th>
      <td>66511</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.027841</td>
      <td>0.244374</td>
      <td>0.087476</td>
      <td>0.247294</td>
      <td>0.606985</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>--VNUI5BC0EIGK9A</th>
      <td>47592</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.027070</td>
      <td>0.068670</td>
      <td>0.013073</td>
      <td>0.923049</td>
      <td>1.031863</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
f_y['CUST_NO'] = f_y.index
```


```python
y_pred = f_y[['CUST_NO','f_CC','f_FX','f_LN','f_WM']]
```


```python
y_pred2 = pd.merge(y,y_pred,how = 'left',on= 'CUST_NO')
```


```python
y_pred2.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>CC_IND</th>
      <th>FX_IND</th>
      <th>LN_IND</th>
      <th>WM_IND</th>
      <th>f_CC</th>
      <th>f_FX</th>
      <th>f_LN</th>
      <th>f_WM</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>_PT5HFBEZJKOZ934</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>JDVF4U8JUANEID68</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8I6SQDGP9OQYUN1M</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>R-TRDUV3GHTID31I</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
y_pred3 = y_pred2[['CUST_NO','f_CC','f_FX','f_LN','f_WM']]
```


```python
y_pred3.columns = ['CUST_NO','CC_IND','FX_IND','LN_IND','WM_IND']
```


```python
y_pred3
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CUST_NO</th>
      <th>CC_IND</th>
      <th>FX_IND</th>
      <th>LN_IND</th>
      <th>WM_IND</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>_PT5HFBEZJKOZ934</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6STXUMWZRDCGSDDU</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>JDVF4U8JUANEID68</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8I6SQDGP9OQYUN1M</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>R-TRDUV3GHTID31I</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>J0DDOZLDFF03QBKW</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>HWAZJ_IO2-GACG_C</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>TREOAHKDWDKDT3UQ</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>SA1L7XU6FBENQNT8</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>K0RCWGLXIKGPZNCI</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>TSRGOP9C_GNWJWU4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>PVLZ97GRPBFRXUA4</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>I7B3N-KZJPDS1SNO</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>_ETDAAZBGUC3TZPU</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>FKVT-HRJIGSRNAU4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>ME_P7MRLRHJNK-7Q</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>FGXT7RNWGNE4U874</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>I2UIZFF1XZVEJ-UI</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>W9YD0FJGPZPYH06E</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>O8YTDNCJ9JUJNJ-G</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>BKL8CYAPEX6OO9CG</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>YBHJWLILFOOCOJJ4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>_RZLU2DEKJJRVI_0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>PQN2J1PZQASPI458</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>SF5PVUK9GCJEL6YO</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>DHX2R4NZ9SOCV020</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>QSC4EGORNZ2TZEAY</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>BHFG8MRPY-_JQTC4</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>MTTHSGUQVFZBC5UQ</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>N4MDYYOUWV3XIFVS</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>29970</th>
      <td>Y1XFI1ZZNHUUB514</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29971</th>
      <td>MSDFDR8YGJK871TY</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29972</th>
      <td>R8SWKAS9YR51KYYK</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29973</th>
      <td>ZHOYH9UCB5AQQ89S</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29974</th>
      <td>K03W6VZUW0VOKVZ4</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29975</th>
      <td>EEQX1GZPCC3FSN4W</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29976</th>
      <td>MGGYNOZFM7NVBR9C</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29977</th>
      <td>ZTB9GLRBDBERVRFG</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29978</th>
      <td>PSVK0FTRVFP5CTAE</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29979</th>
      <td>5TMOVDK2ZU1R0WWU</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29980</th>
      <td>YO1EHTTS7NWPGFSE</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29981</th>
      <td>_ARTN2AITSSCHETA</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29982</th>
      <td>LEF08MVHIXQERI8U</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29983</th>
      <td>HFGSGS8YHVOOMRU8</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29984</th>
      <td>2K7G5GB7CVLMUONS</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29985</th>
      <td>SW3S27KW4EYCHS84</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29986</th>
      <td>U2FZHFIGUSGQT7XS</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29987</th>
      <td>XAFN91QL87CTCFVC</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29988</th>
      <td>ZGKRJTW1XNSA6CLY</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29989</th>
      <td>IGJGFEQUOY31-UZ0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29990</th>
      <td>AGSNZ-GQTFXBL478</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29991</th>
      <td>2VOKN13ABMN46E-8</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29992</th>
      <td>95L-B4WYOVYXBBPQ</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29993</th>
      <td>P6RVELSOTJJUD6CO</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29994</th>
      <td>KK0XOHNFLE-KOFO8</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29995</th>
      <td>ZIKUBRDJR7XPKZGQ</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29996</th>
      <td>HHSKYKG3KFYV-_YA</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29997</th>
      <td>GBGSO4CV37Q9HUGM</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29998</th>
      <td>VCIYFOUZLAFRO400</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29999</th>
      <td>FN2DBQ7_0OCJOSES</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>30000 rows × 5 columns</p>
</div>




```python
y_pred3['CUST_NO'] = '"'+y_pred3['CUST_NO']+'"'
```

    /anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """Entry point for launching an IPython kernel.



```python
y_pred3.to_csv('answer1.csv')
```

# NPA-NXX City Data

A dataset mapping North American telephone prefixes (NPA-NXX) to cities and states/provinces.

## Dataset

**File:** `npa_nxx_city.txt`  
**Format:** CSV (no header row)  
**Records:** 213,364  
**Area codes (NPA):** 437  
**Regions:** 88 state/province codes  

### Columns

| Column | Description | Example |
|--------|-------------|---------|
| NPA | Area code (Numbering Plan Area) | `201` |
| NXX | Exchange prefix | `200` |
| City | City name | `Jersey City` |
| State | State or province code | `NJ` |

### Sample

```
201,200,Jersey City,NJ
201,202,Hackensack,NJ
212,220,New York,NY
310,200,Los Angeles,CA
```

## Coverage

Covers the [North American Numbering Plan (NANP)](https://www.nanpa.com/), including:
- United States (all states and territories)
- Canada (provinces and territories)
- Caribbean and other NANP regions

## Usage

### Python

```python
import csv

with open('npa_nxx_city.txt', newline='') as f:
    reader = csv.reader(f)
    for npa, nxx, city, state in reader:
        print(f"{npa}-{nxx} → {city}, {state}")
```

### Pandas

```python
import pandas as pd

df = pd.read_csv('npa_nxx_city.txt', header=None,
                 names=['npa', 'nxx', 'city', 'state'])
print(df.head())
```

### SQL

```sql
CREATE TABLE npa_nxx (
    npa   CHAR(3),
    nxx   CHAR(3),
    city  VARCHAR(100),
    state CHAR(2)
);

-- Then import the CSV file into the table
```

## License

This dataset is provided as-is for reference purposes.

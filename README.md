# NPA-NXX City Data

A dataset mapping North American telephone prefixes (NPA-NXX) to cities and states/provinces.  
Covers the full [North American Numbering Plan (NANP)](https://www.nanpa.com/) — USA, Canada, Caribbean and territories.

---

## Files

| File | Format | Records |
|------|--------|---------|
| [`npa_nxx_city.txt`](./npa_nxx_city.txt) | CSV (no header row) | 213,365 |
| [`npa_nxx_city.json`](./npa_nxx_city.json) | JSON array of objects | 213,365 |

Both files contain identical data — use whichever format suits your stack.

---

## Dataset

**Records:** 213,365  
**Area codes (NPA):** 437  
**Regions:** 88 state/province/territory codes  

### Columns

| Column | Description | Example |
|--------|-------------|---------|
| NPA | Area code (Numbering Plan Area) | `201` |
| NXX | Exchange prefix | `200` |
| City | City name | `Jersey City` |
| State | State, province or territory code | `NJ` |

---

## Coverage & Statistics

### By Region

| Region | Codes | Records | Share |
|--------|-------|---------|-------|
| **USA** (50 states + DC) | 51 | 196,511 | 92.1% |
| **Canada** | 13 | 13,066 | 6.1% |
| **Caribbean & Other NANP** | 19 | 2,446 | 1.1% |
| **US Territories** | 5 | 1,342 | 0.6% |

### Top 15 Regions by Record Count

| # | Code | Region | Records |
|---|------|--------|---------|
| 1 | CA | California | 21,931 |
| 2 | TX | Texas | 16,018 |
| 3 | NY | New York | 12,013 |
| 4 | FL | Florida | 11,130 |
| 5 | IL | Illinois | 8,941 |
| 6 | PA | Pennsylvania | 8,216 |
| 7 | OH | Ohio | 7,304 |
| 8 | MI | Michigan | 6,906 |
| 9 | GA | Georgia | 6,240 |
| 10 | NJ | New Jersey | 5,582 |
| 11 | NC | North Carolina | 5,517 |
| 12 | VA | Virginia | 4,857 |
| 13 | MA | Massachusetts | 4,757 |
| 14 | ON | Ontario (Canada) | 4,290 |
| 15 | MO | Missouri | 4,141 |

### US Territories (non-state NANP regions)

| Code | Territory | Records |
|------|-----------|---------|
| PR | Puerto Rico | 1,142 |
| GU | Guam | 96 |
| VI | U.S. Virgin Islands | 60 |
| AS | American Samoa | 22 |
| MP | Northern Mariana Islands | 22 |

### Canadian Provinces & Territories

| Code | Province/Territory | Records |
|------|-------------------|---------|
| ON | Ontario | 4,290 |
| QC | Quebec | 2,778 |
| BC | British Columbia | 1,665 |
| SK | Saskatchewan | 801 |
| AB | Alberta | 1,525 |
| MB | Manitoba | 704 |
| NS | Nova Scotia | 463 |
| NL | Newfoundland and Labrador | 328 |
| NB | New Brunswick | 315 |
| PE | Prince Edward Island | 71 |
| NT | Northwest Territories | 56 |
| YT | Yukon | 40 |
| NU | Nunavut | 30 |

### Caribbean & Other NANP Regions

| Code | Region | NPA |
|------|--------|-----|
| JM | Jamaica | 876 |
| BM | Bermuda | 441 |
| DR | Dominican Republic | 809/829/849 |
| TR | Trinidad and Tobago | 868 |
| BA | Bahamas | 242 |
| BD | Barbados | 246 |
| CQ | Cayman Islands | 345 |
| BV | British Virgin Islands | 284 |
| AI | Anguilla | 264 |
| TC | Turks and Caicos | 649 |
| GN | Grenada | 473 |
| DM | Dominica | 767 |
| SA / ZF | Saint Vincent and the Grenadines | 784 |
| SM / NN | Saint Martin / Sint Maarten | 721 |
| KA | Saint Kitts and Nevis | 869 |
| RT | Montserrat | 664 |
| AN | Netherlands Antilles | 599 |

---

## Sample Data

### CSV

```
201,200,Jersey City,NJ
201,202,Hackensack,NJ
212,220,New York,NY
310,200,Los Angeles,CA
```

### JSON

```json
[
  { "npa": "201", "nxx": "200", "city": "Jersey City", "state": "NJ" },
  { "npa": "201", "nxx": "202", "city": "Hackensack",  "state": "NJ" },
  { "npa": "212", "nxx": "220", "city": "New York",    "state": "NY" },
  { "npa": "310", "nxx": "200", "city": "Los Angeles", "state": "CA" }
]
```

---

## Usage Examples

### Python — CSV

```python
import csv

with open('npa_nxx_city.txt', newline='') as f:
    reader = csv.reader(f)
    for npa, nxx, city, state in reader:
        print(f"{npa}-{nxx} -> {city}, {state}")
```

### Python — JSON

```python
import json

with open('npa_nxx_city.json') as f:
    data = json.load(f)

# Build lookup map for O(1) access
lookup = {f"{r['npa']}-{r['nxx']}": r for r in data}

print(lookup['212-220'])
# {'npa': '212', 'nxx': '220', 'city': 'New York', 'state': 'NY'}
```

### Python — Pandas

```python
import pandas as pd

# From CSV
df = pd.read_csv('npa_nxx_city.txt', header=None,
                 names=['npa', 'nxx', 'city', 'state'], dtype=str)

# From JSON
df = pd.read_json('npa_nxx_city.json', dtype=str)

# Lookup
row = df[(df['npa'] == '212') & (df['nxx'] == '220')]
print(row)
```

### JavaScript / Node.js

```javascript
const fs = require('fs');

const data = JSON.parse(fs.readFileSync('npa_nxx_city.json', 'utf8'));

// Build a lookup map for O(1) access
const map = new Map(data.map(r => [`${r.npa}-${r.nxx}`, r]));

console.log(map.get('212-220'));
// { npa: '212', nxx: '220', city: 'New York', state: 'NY' }
```

### TypeScript

```typescript
import * as fs from 'fs';

interface NpaNxx {
  npa: string;
  nxx: string;
  city: string;
  state: string;
}

const data: NpaNxx[] = JSON.parse(fs.readFileSync('npa_nxx_city.json', 'utf8'));

const map = new Map<string, NpaNxx>(
  data.map(r => [`${r.npa}-${r.nxx}`, r])
);

const result = map.get('310-200');
console.log(result?.city); // Los Angeles
```

### Go — CSV

```go
package main

import (
    "encoding/csv"
    "fmt"
    "os"
)

type Record struct {
    NPA, NXX, City, State string
}

func main() {
    f, _ := os.Open("npa_nxx_city.txt")
    defer f.Close()

    reader := csv.NewReader(f)
    rows, _ := reader.ReadAll()

    lookup := make(map[string]Record, len(rows))
    for _, row := range rows {
        key := row[0] + "-" + row[1]
        lookup[key] = Record{row[0], row[1], row[2], row[3]}
    }

    r := lookup["212-220"]
    fmt.Printf("%s, %s\n", r.City, r.State) // New York, NY
}
```

### Go — JSON

```go
package main

import (
    "encoding/json"
    "fmt"
    "os"
)

type Record struct {
    NPA   string `json:"npa"`
    NXX   string `json:"nxx"`
    City  string `json:"city"`
    State string `json:"state"`
}

func main() {
    data, _ := os.ReadFile("npa_nxx_city.json")

    var records []Record
    json.Unmarshal(data, &records)

    lookup := make(map[string]Record, len(records))
    for _, r := range records {
        lookup[r.NPA+"-"+r.NXX] = r
    }

    r := lookup["212-220"]
    fmt.Printf("%s, %s\n", r.City, r.State) // New York, NY
}
```

### C# / .NET

```csharp
using System.Text.Json;

record NpaNxx(string Npa, string Nxx, string City, string State);

var json = File.ReadAllText("npa_nxx_city.json");
var records = JsonSerializer.Deserialize<List<NpaNxx>>(json,
    new JsonSerializerOptions { PropertyNameCaseInsensitive = true })!;

var lookup = records.ToDictionary(r => $"{r.Npa}-{r.Nxx}");

var result = lookup["212-220"];
Console.WriteLine($"{result.City}, {result.State}"); // New York, NY
```

### C# / .NET — CSV with CsvHelper

```csharp
using CsvHelper;
using CsvHelper.Configuration;
using System.Globalization;

record NpaNxx(string Npa, string Nxx, string City, string State);

class NpaNxxMap : ClassMap<NpaNxx>
{
    public NpaNxxMap()
    {
        Map(m => m.Npa).Index(0);
        Map(m => m.Nxx).Index(1);
        Map(m => m.City).Index(2);
        Map(m => m.State).Index(3);
    }
}

using var reader = new StreamReader("npa_nxx_city.txt");
var config = new CsvConfiguration(CultureInfo.InvariantCulture) { HasHeaderRecord = false };
using var csv = new CsvReader(reader, config);
csv.Context.RegisterClassMap<NpaNxxMap>();
var records = csv.GetRecords<NpaNxx>().ToList();
```

### Java

```java
import com.fasterxml.jackson.core.type.TypeReference;
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class NpaNxxLookup {
    record NpaNxx(String npa, String nxx, String city, String state) {}

    public static void main(String[] args) throws Exception {
        var mapper = new ObjectMapper();
        List<NpaNxx> records = mapper.readValue(
            new File("npa_nxx_city.json"),
            new TypeReference<>() {}
        );

        Map<String, NpaNxx> lookup = records.stream()
            .collect(Collectors.toMap(r -> r.npa() + "-" + r.nxx(), r -> r));

        var r = lookup.get("212-220");
        System.out.println(r.city() + ", " + r.state()); // New York, NY
    }
}
```

### Kotlin

```kotlin
import com.fasterxml.jackson.module.kotlin.jacksonObjectMapper
import com.fasterxml.jackson.module.kotlin.readValue
import java.io.File

data class NpaNxx(val npa: String, val nxx: String, val city: String, val state: String)

fun main() {
    val mapper = jacksonObjectMapper()
    val records: List<NpaNxx> = mapper.readValue(File("npa_nxx_city.json"))
    val lookup = records.associateBy { "${it.npa}-${it.nxx}" }

    val r = lookup["212-220"]
    println("${r?.city}, ${r?.state}") // New York, NY
}
```

### Ruby

```ruby
require 'json'
require 'csv'

# From JSON
data = JSON.parse(File.read('npa_nxx_city.json'))
lookup = data.each_with_object({}) { |r, h| h["#{r['npa']}-#{r['nxx']}"] = r }

p lookup['212-220']
# {"npa"=>"212", "nxx"=>"220", "city"=>"New York", "state"=>"NY"}

# From CSV
lookup = {}
CSV.foreach('npa_nxx_city.txt') do |npa, nxx, city, state|
  lookup["#{npa}-#{nxx}"] = { npa: npa, nxx: nxx, city: city, state: state }
end
```

### PHP

```php
<?php
// From JSON
$data = json_decode(file_get_contents('npa_nxx_city.json'), true);
$lookup = array_column($data, null, null);
$lookup = [];
foreach ($data as $r) {
    $lookup["{$r['npa']}-{$r['nxx']}"] = $r;
}
print_r($lookup['212-220']);

// From CSV
$lookup = [];
if (($fh = fopen('npa_nxx_city.txt', 'r')) !== false) {
    while (($row = fgetcsv($fh)) !== false) {
        [$npa, $nxx, $city, $state] = $row;
        $lookup["$npa-$nxx"] = compact('npa', 'nxx', 'city', 'state');
    }
    fclose($fh);
}
```

### Rust

```rust
use std::collections::HashMap;
use std::fs::File;
use std::io::{BufRead, BufReader};

#[derive(Debug)]
struct Record { city: String, state: String }

fn main() {
    let file = File::open("npa_nxx_city.txt").unwrap();
    let mut lookup: HashMap<String, Record> = HashMap::new();

    for line in BufReader::new(file).lines().flatten() {
        let p: Vec<&str> = line.splitn(4, ',').collect();
        if p.len() == 4 {
            lookup.insert(
                format!("{}-{}", p[0], p[1]),
                Record { city: p[2].to_string(), state: p[3].to_string() },
            );
        }
    }

    if let Some(r) = lookup.get("212-220") {
        println!("{}, {}", r.city, r.state); // New York, NY
    }
}
```

### Swift

```swift
import Foundation

struct NpaNxx: Decodable { let npa, nxx, city, state: String }

let data = try! Data(contentsOf: URL(fileURLWithPath: "npa_nxx_city.json"))
let records = try! JSONDecoder().decode([NpaNxx].self, from: data)
let lookup = Dictionary(uniqueKeysWithValues: records.map { ("\($0.npa)-\($0.nxx)", $0) })

if let r = lookup["212-220"] {
    print("\(r.city), \(r.state)") // New York, NY
}
```

---

## Database Examples

### PostgreSQL

```sql
CREATE TABLE npa_nxx (
    npa   CHAR(3)       NOT NULL,
    nxx   CHAR(3)       NOT NULL,
    city  VARCHAR(100)  NOT NULL,
    state CHAR(2)       NOT NULL,
    PRIMARY KEY (npa, nxx)
);

-- Import from CSV (server-side)
COPY npa_nxx (npa, nxx, city, state)
FROM '/absolute/path/to/npa_nxx_city.txt'
DELIMITER ',' CSV;

-- Or client-side via psql
-- \copy npa_nxx FROM 'npa_nxx_city.txt' DELIMITER ',' CSV;

-- Lookup
SELECT city, state FROM npa_nxx WHERE npa = '212' AND nxx = '220';

-- Records per state, sorted
SELECT state, COUNT(*) AS records
FROM npa_nxx
GROUP BY state
ORDER BY records DESC;
```

### MySQL / MariaDB

```sql
CREATE TABLE npa_nxx (
    npa   CHAR(3)       NOT NULL,
    nxx   CHAR(3)       NOT NULL,
    city  VARCHAR(100)  NOT NULL,
    state CHAR(2)       NOT NULL,
    PRIMARY KEY (npa, nxx),
    INDEX idx_state (state)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- Import from CSV
LOAD DATA INFILE '/path/to/npa_nxx_city.txt'
INTO TABLE npa_nxx
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
(npa, nxx, city, state);

-- Lookup
SELECT city, state FROM npa_nxx WHERE npa = '212' AND nxx = '220';
```

### Oracle

```sql
CREATE TABLE npa_nxx (
    npa   CHAR(3)        NOT NULL,
    nxx   CHAR(3)        NOT NULL,
    city  VARCHAR2(100)  NOT NULL,
    state CHAR(2)        NOT NULL,
    CONSTRAINT pk_npa_nxx PRIMARY KEY (npa, nxx)
);

-- SQL*Loader control file (npa_nxx.ctl):
-- LOAD DATA
-- INFILE 'npa_nxx_city.txt'
-- INTO TABLE npa_nxx
-- FIELDS TERMINATED BY ','
-- (npa, nxx, city, state)

-- External table alternative
CREATE TABLE npa_nxx_ext (
    npa CHAR(3), nxx CHAR(3), city VARCHAR2(100), state CHAR(2)
)
ORGANIZATION EXTERNAL (
    TYPE ORACLE_LOADER
    DEFAULT DIRECTORY data_dir
    ACCESS PARAMETERS (
        RECORDS DELIMITED BY NEWLINE
        FIELDS TERMINATED BY ','
    )
    LOCATION ('npa_nxx_city.txt')
);

-- Lookup
SELECT city, state FROM npa_nxx WHERE npa = '212' AND nxx = '220';
```

### SQLite

```sql
CREATE TABLE npa_nxx (
    npa   TEXT NOT NULL,
    nxx   TEXT NOT NULL,
    city  TEXT NOT NULL,
    state TEXT NOT NULL,
    PRIMARY KEY (npa, nxx)
);

-- Import via sqlite3 CLI:
-- .mode csv
-- .import npa_nxx_city.txt npa_nxx

-- Lookup
SELECT city, state FROM npa_nxx WHERE npa = '212' AND nxx = '220';
```

### Redis

```bash
# Bulk load via pipeline
while IFS=',' read -r npa nxx city state; do
  echo "SET npa:${npa}-${nxx} \"${city},${state}\""
done < npa_nxx_city.txt | redis-cli --pipe

# Lookup
redis-cli GET npa:212-220
# "New York,NY"
```

```python
# Python — load as hashes with redis-py
import redis, csv

r = redis.Redis()
pipe = r.pipeline(transaction=False)

with open('npa_nxx_city.txt') as f:
    for i, (npa, nxx, city, state) in enumerate(csv.reader(f)):
        pipe.hset(f"npa:{npa}-{nxx}", mapping={"city": city, "state": state})
        if i % 1000 == 999:
            pipe.execute()

pipe.execute()

# Lookup
print(r.hgetall("npa:212-220"))
# {b'city': b'New York', b'state': b'NY'}
```

### MongoDB

```javascript
// Import via mongoimport (shell):
// mongoimport --db telecom --collection npa_nxx \
//   --file npa_nxx_city.json --jsonArray

// Node.js bulk insert
const { MongoClient } = require('mongodb');
const fs = require('fs');

const client = new MongoClient('mongodb://localhost:27017');
await client.connect();
const col = client.db('telecom').collection('npa_nxx');

const data = JSON.parse(fs.readFileSync('npa_nxx_city.json'));
await col.insertMany(data);
await col.createIndex({ npa: 1, nxx: 1 }, { unique: true });
await col.createIndex({ state: 1 });

// Lookup
const result = await col.findOne({ npa: '212', nxx: '220' });
console.log(result.city, result.state); // New York NY

// Records per state
const stats = await col.aggregate([
  { $group: { _id: '$state', count: { $sum: 1 } } },
  { $sort: { count: -1 } }
]).toArray();
```

### Elasticsearch / OpenSearch

```bash
# Create index with mapping
curl -X PUT "localhost:9200/npa_nxx" -H 'Content-Type: application/json' -d '{
  "mappings": {
    "properties": {
      "npa":   { "type": "keyword" },
      "nxx":   { "type": "keyword" },
      "city":  { "type": "text", "fields": { "keyword": { "type": "keyword" } } },
      "state": { "type": "keyword" }
    }
  }
}'
```

```python
from elasticsearch import Elasticsearch, helpers
import json

es = Elasticsearch()

with open('npa_nxx_city.json') as f:
    data = json.load(f)

actions = [
    {"_index": "npa_nxx", "_id": f"{r['npa']}-{r['nxx']}", "_source": r}
    for r in data
]
helpers.bulk(es, actions)

# Lookup
result = es.get(index="npa_nxx", id="212-220")
print(result["_source"])

# Full-text city search
hits = es.search(index="npa_nxx", query={"match": {"city": "Jersey City"}})
```

### Weaviate (Vector DB — semantic city search)

```python
import weaviate, json

client = weaviate.Client("http://localhost:8080")

client.schema.create_class({
    "class": "NpaNxx",
    "vectorizer": "text2vec-transformers",  # auto-embeds 'city' field
    "properties": [
        {"name": "npa",   "dataType": ["text"]},
        {"name": "nxx",   "dataType": ["text"]},
        {"name": "city",  "dataType": ["text"]},
        {"name": "state", "dataType": ["text"]},
    ]
})

with open('npa_nxx_city.json') as f:
    data = json.load(f)

with client.batch(batch_size=500) as batch:
    for r in data:
        batch.add_data_object(r, "NpaNxx")

# Semantic search: find prefixes near "New York"
results = (
    client.query
    .get("NpaNxx", ["npa", "nxx", "city", "state"])
    .with_near_text({"concepts": ["New York"]})
    .with_limit(5)
    .do()
)
print(results["data"]["Get"]["NpaNxx"])
```

### Qdrant (Vector DB)

```python
from qdrant_client import QdrantClient
from qdrant_client.models import Distance, VectorParams, PointStruct
from sentence_transformers import SentenceTransformer
import json

client = QdrantClient("localhost", port=6333)
model = SentenceTransformer("all-MiniLM-L6-v2")

client.recreate_collection("npa_nxx", vectors_config=VectorParams(size=384, distance=Distance.COSINE))

with open('npa_nxx_city.json') as f:
    data = json.load(f)

# Embed city names as vectors
points = [
    PointStruct(
        id=i,
        vector=model.encode(r['city']).tolist(),
        payload=r
    )
    for i, r in enumerate(data)
]

client.upsert("npa_nxx", points=points)

# Semantic search
query_vector = model.encode("New York City").tolist()
results = client.search("npa_nxx", query_vector=query_vector, limit=5)
for r in results:
    print(r.payload)
```

---

## License

This dataset is provided as-is for reference purposes.

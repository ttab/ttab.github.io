---
layout: page
title: Search API
---

## Search for TT content

```
GET https://tt.se/api/search?p=<productCode>&ak=<apiKey>    # ttninjs
```

```
GET https://tt.se/api/search?q=lorem+ipsum&p=TTINR&ak=<apiKey>      # search for "lorem ipsum" in the "TTINR" product
```

```
GET https://tt.se/api/search?q=lorem+ipsum&p=FOTO&ak=<apiKey>       # search for "lorem ipsum" in the "FOTO" product
```

## Query parameters

{:.table}
Name                      | Description 
--------------------------|--------------
`?ak=<apiKey>`            | Use this api key to authenticate. With this, no login is needed.
`?q=<query>`              | Use this out parameter to ...
`?p=<productCode>,<productCode>,...`        | Filter search result by products. Lookup your available product codes with the search API
`?agr=<agreementId>,<agreementId>,...`      | Filter search result by agreements. Lookup your available agreements with the search API
`?trs=2015-10-10`         | Start date
`?tre=2015-10-14`         | End date
`?tr=h `                  | Time range: hour
`?tr=d `                  | Time range: day
`?tr=w `                  | Time range: week
`?tr=m `                  | Time range: month
`?tr=y `                  | Time range: year
`?s=<size>`               | Size of search result (default: 20)
`?fr=<from>`              | Index into search result
`?sort=asc|desc`          | Sort ascending or descending (default)

# Lookup your products/agreements

```
GET https://tt.se/api/search/agreements?ak=<apiKey> 
```

## Query parameters

{:.table}
Name                      | Description 
--------------------------|--------------
`?ak=<apiKey>`            | Use this api key to authenticate. With this, no login is needed.

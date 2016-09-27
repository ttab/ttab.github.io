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

```
?ak=<apiKey>            # Use this api key to authenticate. With this, no login is needed.
```

```
?q=<query>              # Use this out parameter to ...
```

```
?p=<productCode>        # Filter search result by products. Lookup your available product codes with the search API
```

```
?agr=<agreementId>      # Filter search result by agreements. Lookup your available agreements with the search API
```

```
?trs=2015-10-10         # start date
```

```
?tre=2015-10-14         # end date
```

```
# time range
```

```
?tr=h                   # hour
```

```
?tr=d                   # day
```

```
?tr=w                   # week
```

```
?tr=m                   # month
```

```
?tr=y                   # year
```

## Lookup your products/agreements

```
GET https://tt.se/api/search/agreements?ak=<apiKey> 
```

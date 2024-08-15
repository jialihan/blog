## System Design - TinyUrl

### 1. Requirement

Functional Requirements:

- convert long url to short url
- when giving short url, redirect user to long url
- any expire date？
- can user **customize** the url？eg: go link

Non-functional requirements:

- low latency
- high availability(no downtime): as a user, i don't want to receive a failure result when convert or redirect. eg: when recruiters paste short url

### 2. estimate Capacity

- url length
- `read vs write` ratio? eg: `200 read : 1 writes`
- how many urls? how frequently make requests to server (per second, per minute...)?

  - eg: `X (per seconds) * 60 * 60 * 24 * 365 = Y (total per year)`,
  - when `X=40`,` 3.5 million` requests per day.
  - when store 10 years: `3.5m * 365 * 10` = `13 billion` urls for 10 years
  - each `char = 1 byte`, eg: `100 byte` per url.
  - DB Storage needs: `100 byte * 13 billion` = **`1.3 Tera Bytes`**

- how many "chars" are allowed? number/letter? eg: `a-z, A-Z, 0-9 = 62 chars`
  ```
  length --> counts of combinations
  1  --> 62
  2  --> 62^2
  ...
  n  --> 62^n  = 2^6n = Y (total per year)
  ```

### 3. Architecture

### 3.1 Long to short service (create -> write into DB)

- issue1: conflict for 2 servers, what if generate the same token?
- issue2: what if token is used up? - need to clean up and re-use expired token.
- issue3: token service **security** to `encode`?
  - 1. hashing algorithm such as MD5, SHA-256, or CRC32
  - 2. encade using base62 or base64
- issue4: app server avoid **single point of failure**, use `LB` to have multiple servers.

![image](../assets/tinyurl_architecture_01.png ":size=840")

### 3.2 Short to long service (read from DB and redirect)

- issue3: since it's mostly **Read** from DB, add a **in-memory cache**, Redis upon the DB.

![image](../assets/tinyurl_architecture_02.png ":size=820")

### 4. DB

- since it's a key-value data, choose NoSQL is better.
- nosql easy to scale, using sharding, eg: letter-range-based, or hash-based (calc the token into hash key).
  - eg: use letters to `buckets` DB : `bucket [a-l], buket[m-t], ....`
- why cache？ also for those **most frequently requested URL**, eg: created by celebrity.
- what if cache is full? eg: remove the Least Recently used (LRU cache).

#### 4.1 Schema

```
Link
- shorturl
- longurl
- userId
- createDate
- expireDate
```

```
User
- userId
- name
- email
```

### 5. Improvemnt

#### How to support customized url created by url?

How to avoid conflicts with long urls in DB？

- set rules: `>= 8 chars`, `must contain special chars`, etc..


## Keywords

- NoSQL
- document-oriented database

## What is a document in firestore?

A lightweight record that contains fields, which map to values.

> arrays or nested objects, are called maps

## What is a reference in firestore?

A reference is a lightweight object that just points to a location in your database.

- 拿`collection reference`：
  - querying the documents in the collection

- 拿`document reference`:
  - 對該document進行讀寫

拿Reference的小技巧：

```swift
let documnetRef = db.document("users/jason")
```

## Index types in Cloud Firestore

1. single-field indexes
2. composite indexes

## Data Structuring for cloud firestore

- Always store numbers as doubles

### Options

1. Document：
2. MultiCollections:
3. SubCollections within documents:

## Write

### Options

1. 在某個集合中新增檔案，並指定DocumentID
2. 在某個集合中新增檔案，使用Cloud Firestore自動產生的DocumentID
3. 在某個集合中新增空檔案（使用Cloud Firestore自動產生的DocumentID），日後在增加資料

### Set document

> 建立 / 覆蓋某個檔案

```swift
// Update one field, creating the document if it does not exist.
db.collection("cities").document("BJ").setData([ "capital": true ], merge: true)
```

如果使用`set()`建立檔案，一定需要給一個DocumentID，但是不是每次我們要建立的資料的ID都需要我們自己產生，這時候可以使用`add()`，這樣就可以請Cloud Firestore幫我們建立這筆資料的DocumentID

> Behind the scenes, .add(...) and .doc().set(...) are completely equivalent, so you can use whichever is more convenient.
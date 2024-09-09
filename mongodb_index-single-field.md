1. Pre-condition:
 - create db: ```use [name database]```
 - create collection: ```db.createCollection("products")```
 - insert 100000 record:
   
   ```
   for (let i = 0; i < 500000; i++) {
      db.products.insert({
          name: "Product " + i,
          category: ["Electronics", "Fashion", "Appliances", "Toys", "Sports", "Books"][Math.floor(Math.random() * 6)],
          price: Math.round(Math.random() * 2000 * 100) / 100,
          stock: Math.floor(Math.random() * 500) + 1
      });
    }
 - check insert: ```db.products.countDocuments()```

2. Testing
   
    2.1 Run Query Without Index
     - you can run the query as normal and use `explain()` to see how it is executed.
     - cmd: ```db.products.find({ category: "Electronics" }).explain("executionStats")```
     - stage: `stage: 'COLLSCAN'`
       
      <img width="888" alt="image" src="https://github.com/user-attachments/assets/31b4025d-09ae-4500-a105-8ccefbbe0dbc">


    2.1 Run Query With Index (a single-field index)
     - before: create index `db.products.createIndex({ category: 1 })` 
     - you can run the query as normal and use `explain()` to see how it is executed.
     - cmd: ```db.products.find({ category: "Electronics" }).explain("executionStats")```
     - stage: `stage: 'IXSCAN'`
       
       <img width="888" alt="image" src="https://github.com/user-attachments/assets/2c6d9a4c-d358-4eb2-aaf9-d37bb090540f">

  
3. Note:
   - MongoDB creates a unique index on the _id field during the creation of a collection.
   - MongoDB uses B-tree (balanced tree) structures by default for indexing. B-trees are efficient for lookups, insertions, and deletions, making them suitable for managing data in a way that maintains order and optimizes query performance.
   

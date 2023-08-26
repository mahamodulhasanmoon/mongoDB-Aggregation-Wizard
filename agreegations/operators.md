# pipeline operators

## **Match operator**

- The `$match` operator in MongoDB is used to filter documents based on specified criteria. In the given example, the operator is used to retrieve all the documents in the `collection` where the value of the `field` is greater than `5`.

Here's an example of using `$Match` operator in MongoDB:

```bash
db.collection.aggregate([
  { $match: { field: { $gt: 5 } } }
])

```

This will return all the documents in the `collection` where the value of the `field` is greater than `5`.

## ************Group Operator************

- `$group` operator is used in MongoDB to group documents by a specific field and apply aggregate functions to each group. For example, you can group a collection of sales by product and calculate the total revenue for each product using the `$sum` aggregation function. The syntax for using the `$group` operator is as follows:

```bash
db.collection.aggregate([
  { $group: { _id: "$field", newField: { $sum: "$value" } } }
])

```

In this example, the `$group` operator groups the documents in the `collection` by the value of the `field` field and calculates the sum of the `value` field for each group. The `_id` field is used to specify the field to group by, and the `newField` field is used to specify the name of the new field that will contain the aggregated result.

 here's an example of using the `$group` operator in MongoDB:

Suppose you have a `sales` collection with the following documents:

```bash
{ "_id": 1, "product": "apple", "quantity": 10, "price": 1.5 }
{ "_id": 2, "product": "orange", "quantity": 20, "price": 2.0 }
{ "_id": 3, "product": "banana", "quantity": 30, "price": 2.5 }
{ "_id": 4, "product": "apple", "quantity": 5, "price": 1.2 }
{ "_id": 5, "product": "banana", "quantity": 15, "price": 2.0 }

```

If you want to group the documents by the `product` field and calculate the total revenue for each product, you can use the following pipeline:

```bash
db.sales.aggregate([
    { $group: { _id: "$product", revenue: { $sum: { $multiply: [ "$quantity", "$price" ] } } } }
])

```

This will return the following output:

```bash
{ "_id" : "banana", "revenue" : 67.5 }
{ "_id" : "orange", "revenue" : 40 }
{ "_id" : "apple", "revenue" : 22.5 }

```

In this example, the `$group` operator groups the documents by the `product` field and calculates the revenue for each group using the `$sum` aggregation function. The `$multiply` operator is used to multiply the `quantity` and `price` fields for each document before summing them up.

## $Project Operator

The `$project` operator is used to select specific fields from the documents in a collection and return them in the result set. It can also be used to add new fields to the documents or modify the existing fields.

Here's an example of using the `$project` operator in MongoDB:

Suppose you have a `sales` collection with the following documents:

```bash
{ "_id": 1, "product": "apple", "quantity": 10, "price": 1.5 }
{ "_id": 2, "product": "orange", "quantity": 20, "price": 2.0 }
{ "_id": 3, "product": "banana", "quantity": 30, "price": 2.5 }
{ "_id": 4, "product": "apple", "quantity": 5, "price": 1.2 }
{ "_id": 5, "product": "banana", "quantity": 15, "price": 2.0 }

```

And you only need to retrieve the `product` and `price` fields, you can use the following pipeline:

```bash
db.sales.aggregate([
    { $project: { _id: 0, product: 1, price: 1 } }
])

```

This will return the following output:

```bash
{ "product" : "apple", "price" : 1.5 }
{ "product" : "orange", "price" : 2 }
{ "product" : "banana", "price" : 2.5 }
{ "product" : "apple", "price" : 1.2 }
{ "product" : "banana", "price" : 2 }

```

In this example, the `$project` operator is used to select only the `product` and `price` fields from the documents in the `sales` collection and exclude the `_id` field. The `1` value indicates that the field should be included in the result set, while `0` indicates that it should be excluded.

## $sort Operator

`sort operator` is stage in MongoDB's aggregation pipeline is used to reorder documents based on specified criteria. It's commonly used to arrange documents in a specific order before further processing or displaying the result

Examples:
Assuming we have a collection of students documents with the following structure:

```
{
  "_id": 1,
  "name": "Alice",
  "score": 85
}
```
Sort by a Single Field in Ascending Order:

```
{
  $sort: {
    "score": 1   // Sort by score in ascending order
  }
}
```
This will arrange the documents based on the "score" field in ascending order.

Sort by a Single Field in Descending Order:

```
{
  $sort: {
    "score": -1   // Sort by score in descending order
  }
}
```
This will arrange the documents based on the "score" field in descending order.

Sort by Multiple Fields:

``` bash
{
  $sort: {
    "score": -1,  
    "name": 1       
  }
}
```
This will first sort the documents by "score" in descending order, and for documents with the same score, it will sort by "name" in ascending order.

Sort by Nested Fields:

```javaScript

{
  $sort: {
    "address.city": 1   // Sort by the city within the nested "address" field
  }
}
``` 
This will sort documents based on the "city" field within the nested "address" field.

Sorting by Text Index Score:

``` javaScript

{
  $sort: {
    "score": { $meta: "textScore" }
  }
} 
```
If you're using a text index for full-text search, you can sort by the text index score to prioritize the most relevant search results.

Remember that the $sort stage is often used early in the aggregation pipeline to arrange documents before applying further transformations or analyses. Sorting can impact performance, especially on large datasets, so it's important to use it strategically based on your application's needs.
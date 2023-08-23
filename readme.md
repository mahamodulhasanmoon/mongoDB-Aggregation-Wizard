# Aggregation Pipeline Example

## Here is Some sample Data  which I imported my Database:

```json
db.orders.insertMany( [
   { _id: 0, name: "Pepperoni", size: "small", price: 19,
     quantity: 10, date: ISODate( "2021-03-13T08:14:30Z" ) },
   { _id: 1, name: "Pepperoni", size: "medium", price: 20,
     quantity: 20, date : ISODate( "2021-03-13T09:13:24Z" ) },
   { _id: 2, name: "Pepperoni", size: "large", price: 21,
     quantity: 30, date : ISODate( "2021-03-17T09:22:12Z" ) },
   { _id: 3, name: "Cheese", size: "small", price: 12,
     quantity: 15, date : ISODate( "2021-03-13T11:21:39.736Z" ) },
   { _id: 4, name: "Cheese", size: "medium", price: 13,
     quantity:50, date : ISODate( "2022-01-12T21:23:13.331Z" ) },
   { _id: 5, name: "Cheese", size: "large", price: 14,
     quantity: 10, date : ISODate( "2022-01-12T05:08:13Z" ) },
   { _id: 6, name: "Vegan", size: "small", price: 17,
     quantity: 10, date : ISODate( "2021-01-13T05:08:13Z" ) },
   { _id: 7, name: "Vegan", size: "medium", price: 18,
     quantity: 10, date : ISODate( "2021-01-13T05:10:13Z" ) }
] )
```

## Notes: in this Database there is an ordered list of fastfood like pizza pepperoni,cheese,vegan, and there is 3 type of size “small” ,”medium” and “large”

<br>

# Task 1: Filtering by Medium Size Orders
**To filter orders with a medium size:**

```bash
db.orders.aggregate([
{$match:{size:"medium"}}
])
```
## Output Result:
- **Orders with "Medium" size details**

my  output Result is :

```jsx
{
  _id: 1,
  name: 'Pepperoni',
  size: 'medium',
  price: 20,
  quantity: 20,
  date: 2021-03-13T09:13:24.000Z
},
{
  _id: 4,
  name: 'Cheese',
  size: 'medium',
  price: 13,
  quantity: 50,
  date: 2022-01-12T21:23:13.331Z
},
{
  _id: 7,
  name: 'Vegan',
  size: 'medium',
  price: 18,
  quantity: 10,
  date: 2021-01-13T05:10:13.000Z
}

```

now i wanna filtered by medium size of pizza name and details

```jsx
db.orders.aggregate([
// for search by medium list
{$match:{size:"medium"}},

])
```

- $match  state filter it via medium size
- and pass data  genated filtered output

- ************************************************Now time to grouped by  name and totalQuantity =************************************************

```bash
db.orders.aggregate( [

   // Stage 1: Filter pizza order documents by pizza size
   {
      $match: { size: "medium" }
   },

   // Stage 2: Group remaining documents by pizza name and calculate total quantity
   {
      $group: { _id: "$name", totalQuantity: { $sum: "$quantity" } }
   }

] )
```

****************here is my output:****************

```jsx
[
   { _id: 'Cheese', totalQuantity: 50 },
   { _id: 'Vegan', totalQuantity: 10 },
   { _id: 'Pepperoni', totalQuantity: 20 }
]
```

same way i wanna get small size of orderQuantity:

```jsx
db.orders.aggregate([
{$match:{size:"small"}},
{$group:{_id:"$name",totalQuatity:{$sum:"$quantity"}}}
])
```

this is your  Result: Am I Right?

```jsx
[{
  _id: 'Vegan',
  totalQuatity: 10
},
{
  _id: 'Pepperoni',
  totalQuatity: 10
},
{
  _id: 'Cheese',
  totalQuatity: 15
}]
```

**can you find out total small size order  medium size order or large order?** 

let’s try it

```jsx
db.orders.aggregate([
{$match:{size:{$in:["small","medium"]}}},
{$group:{_id:"$name",totalQuatity:{$sum:"$quantity"}}}
])
```

**wow great :**

**can you calculate total  order between 2 dates?**

let’s try it:

first of all 

```jsx
db.orders.aggregate( [

   // Stage 1: Filter pizza order documents by date range
   {
      $match:
      {
         "date": { $gte: new ISODate( "2020-01-30" ), $lt: new ISODate( "2022-01-30" ) }
      }
   },

   // Stage 2: Group remaining documents by date and calculate results
   {
      $group:
      {
         _id: { $dateToString: { format: "%Y-%m-%d", date: "$date" } },
totalOrderValue:{ $sum:{$multiply:["$price","$quantity"]}},
// make agarage
averageOrderQuantity: { $avg: "$quantity" }
      
      }
   },
//format it via decending order
{
      $sort: {_id: -1 }
   }

 ] )

```


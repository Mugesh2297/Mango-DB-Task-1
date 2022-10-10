# Mango-DB-Task-1
The  Files Consists of Answers of the Questions in Task

db.createCollection("productDetails");
db.productDetails.find();
1. Find all the information about each products

 db.productDetails.find();
 
2.Find the product price which are between 400 to 800

db.productDetails.find( {product_price : {$gt: 400 ,$lt: 800 } });

3. Find the product price which are not between 400 to 600

db.productDetails.find( { product_price: { $not: { $gt: 400 , $lt:600 } } } )

4. List the four product which are greater than 500 in price 

 db.productDetails.find({product_price:{$gte:500}}).limit(4)
 
5. Find the product name and product material of each products

db.productDetails.find({},{product_name:1,product_material:1})

6. Find the product with a row id of 10

  db.productDetails.find({id:"10"});
  
7. Find only the product name and product material

db.productDetails.find({},{product_name:1,product_material:1,_id:0})

8. Find all products which contain the value of soft in product material 

 db.productDetails.find({product_material:"Soft"});
 
9. Find products which contain product color indigo  and product price 492.00

db.productDetails.find({ $or : [ {product_color : 'indigo'}, {product_price : 492.00} ] });

10. Delete the products which product price value are same


var duplicates = [];

db.productDetails.aggregate([
  
  { $group: { 
    _id: { product_price: "$product_price"},
    dups: { "$addToSet": "$_id" }, 
    count: { "$sum": 1 } 
  }},
  { $match: { 
    count: { "$gt": 1 }    
  }}
],

)               
.forEach(function(doc) {
    doc.dups.forEach( function(dupId){ 
        duplicates.push(dupId);   
        }
    )
})

printjson(duplicates);     

db.productDetails.remove({_id:{$in:duplicates}})
db.productDetails.find();


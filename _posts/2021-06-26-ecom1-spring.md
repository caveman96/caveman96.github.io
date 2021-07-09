---
title: E-COM 1- Building a REST server and APIs with Java and Spring
author: Silesh Chandran
date: 2021-06-25 20:10:00 +0530
categories: [backend, projects]
tags: [backend, java, spring, rest, api, projects]
toc: true
comments: true
---

This post will serve as the starting point for a lot of big things to come. The plan is to incrementally develop a project (an e-commerece application) and experiment with and implement various industry relevant features such as load balancing, caching, synchronization etc. To do this we need a starting point. In this post I will cover the basics of getting started with a simple REST server with Java and Spring. 

## Requirements
Lets begin by defining our requirements. We will need to create the following APIs to get started:
1. To add a new product listing
2. To remove an exisitng listing
3. To get available products

## Desigining the class
For the scope of these requirements we only need to design two simple classes, the product listing and the Shop class. The shop contains a list of products and exposes this with a method.

```java
public class Product {
    String productId;
    String productName;
    String productDescription;
    Double price;
	
	...
}
	
public class Shop {
    String shopId;
    String shopName;
    HashMap<String, Product> products;
	
	...
}

```

## Getting started
We can use the spring initializr (https://start.spring.io/) to get started with a spring boot project. Add spring web to the dependancies. Others can be added later. This downloaded zip file can be extracted and imported into the IDE of your choice, I'm using intellij.

![Desktop View](../../assets/img/ECOM/spring.png)

## Add internal logic
The next step would be to create our product listing class and add the required methods corresponding to each API.
We can create a shop class that keeps track of product listings (similar to how Aliexpress operates). Product listings is a hashmap of products mapped by product Ids and we can define the methods needed to add, edit and remove listings.

```java

public class Shop {

	....

    public ArrayList<Product> getProducts(){
        return  new ArrayList<Product>(products.values());
    }

    public Product getProductById(String id){
        return products.containsKey(id)?products.get(id):null;
    }

    public void addProduct(Product product){
        products.put(product.getProductId(), product);
    }

    public void removeProduct(String id){
        products.remove(id);
    }
}

```

## Exposing APIs
Now that we have the logic ready, all thats left to do is expose the methods out as APIs. To do this first we create a RESTController class. This will serve as our controller for all our services. Here we can make use of Spring annotations to expose functions defined in the Shop class out as APIs. We have a Post api for adding and editing listings, a get api which returs a list of all products, and a delete api to remove a particular listing.
```java
@RestController
public class ProductListingController {
    Shop myShop = null;
    Gson gson;
    @PostConstruct
    public void initialize(){
        myShop = new Shop("S001", "TechShop");
        gson = new Gson();
    }

    @GetMapping(path = "/products")
    public ArrayList<Product> getProducts(){
        return myShop.getProducts();
    }

    @PostMapping(path = "/products")
    public void addProduct(@RequestBody Product product){
        myShop.addProduct(product);
    }

    @GetMapping(path = "/products/{productId}")
    public Product getProcductById(@PathVariable String productId){
        return myShop.getProductById(productId);
    }

    @DeleteMapping(path = "/products/{productId}")
    public String deleteProduct(@PathVariable String productId){
        if(myShop.getProductById(productId)!=null) {
            myShop.removeProduct(productId);
            return "success";
        }
        return "failure";

    }
}
```
The annotations @Getmapping, @PostMapping and @DeleteMapping can be used to expose the methods out as apis. The path for each can be given as well. For the post api that is used to add a new product, we annotate the argument with @RequestBody. This allows for the product info to be sent as the body of the request. Similarly for the get and delete apis we can make use of @PathVariable to read the input argument from the request url as a path variable.
Spring will handle serialization and deserialization so reading and returning a Product object is an easy task.

## Testing the APIs
To start the server first download maven (https://maven.apache.org/download.cgi), extract the zip file and add the bin folder to Path so that it can be used from anywhere. Maven is used for managing dependancies and building the project.
To execute, go to the root folder and run:

 mvn clean spring-boot:run
 
This will start the server in localhost:8080. 
We can make use of PostMan to make API calls and keep track of our parameters. 

![Desktop View](../../assets/img/ECOM/postapi.png)
_Post the product details_

![Desktop View](../../assets/img/ECOM/getapi.png)
_Use the get api to view details_

## Whats next?
Now that we have a simple server ready, we can further enhance it. In the next post, we will see how to create a DB using MongoDB and move the storage of product listings to it. Thank you for reading!
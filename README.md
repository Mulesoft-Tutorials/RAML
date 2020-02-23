# RAML

RESTful API Modeling Language (RAML) is a YAML-based language for describing RESTful APIs. It provides all the information necessary to describe RESTful APIs, providing a simpler way to design APIs.

# Structure of RAML

  ## Enter the Root
```
#%RAML 1.0
---
title: e-comm API
baseUri: http://localhost:9080/api/{version}
version: v1
```
## ENTER RESOURCES

As a thoughtful API designer, it's important to consider how your API consumers will use your API. It's especially important because in many ways, as the API designer YOU control the consumption

```
/customers
/orders
```
Notice that these resources all begin with a slash (/). In RAML, this is how you indicate a resource. Any methods and parameters nested under these top level resources belong to and act upon that resource. Now, since each of these resources is a collection of individual objects, we'll need to define some sub-resources to fill out the collection.

## ENTER METHODS

Here's where it starts to get interesting, as you decide what you want the developer to be able to do with the resources you've made available. Let's quickly review the 4 most common HTTP verbs:

**GET** - Retrieve the information defined in the request URI.

**PUT** - Replace the addressed collection. At the object-level, create or update it.

**POST** - Create a new entry in the collection. This method is generally not used at the object-level.

**DELETE** - Delete the information defined in the request URI.

You can add as many methods as you like to each resource of your orders API, at any level. However, each HTTP method can only be used once per resource. Do not overload the GET (you know who you are).

```
/customers
  get:
  post:
/orders
  get:
  post:
```

## ENTER URI PARAMETERS

The resources that we defined are collections of smaller, relevant objects. You, as the thoughtful API designer, have realized that developers will most likely want to act upon these more granular objects.
This is a URI parameter, denoted by surrounding curly brackets in RAML:
```
/orders
/{order-id}
```
## ENTER QUERY PARAMETERS

Great job so far! Now, let's say you want your API to allow even more powerful operations. You already have collections-based resource types that are further defined by object-based URI parameters. But you also want developers to be able perform actions like filtering a collection. Query parameters are a great way to accomplish this.

```
/customer
  get:
    queryParameters:
      firstName:
        displayName: FirstName
        type: string
        description: First Name of the customer
        example: Rajesh
        required: false
      lastName:
        displayName: LastName
        type: string
        description: Last Name of the customer
        example: Kumar
        required: false
      id:
        displayName: ID
        type: number
        minLength: 5
        example: 22343
```
## ENTER RESPONSES

Responses MUST be a map of one or more HTTP status codes, and each response may include descriptions, examples, or schemas.

```
/customer
  get:
    queryParameters:
      firstName:
        displayName: FirstName
        type: string
        description: First Name of the customer
        example: Jason
        required: false
      lastName:
        displayName: LastName
        type: string
        description: Last Name of the customer
        example: Bourne
        required: false
      id:
        displayName: ID
        type: number
        minLength: 5
        example: 22343
    responses:
      200:
        body:
          application/json:
            example: 
              {
                "_meta": {
                  "messages": []
                },
                "customers": [
                  {
                    "identifier": "deb89058-77fb-4e97-8c83-6506c38cacf8",
                    "firstName": "Jason",
                    "lastName": "Bourne",
                    "paymentMethods": [
                      {
                        "paymentMethodType": {
                          "medium": "CARD",
                          "issuer": "BANK",
                          "name": "VISA"
                        }
                      }
                    ],
                    "deliveryAddresses": [
                      {
                        "identifier": "deb89058-77fb-4e97-8c83-6506c38cacf8",
                        "type": "BILLING",
                        "address": "Main street 122",
                        "city": "San Francisco",
                        "state": "California",
                        "postalCode": "90210",
                        "recipient": "Jason Bourne",
                        "country": "US",
                        "secondaryAddress": "",
                        "mailStopCode": "",
                        "attention": ""
                      }
                    ]
                  }
                ]
              }
  
```
## RESOURCE TYPES

The "collection/collection-item" pattern

```
#%RAML 1.0
  title:

  /resources:
    get:
    post:
    /{resourceId}:
      get:

```

So, we found two different type of resources. The item (represented by an id), and the collection (containing all the items). It would be nice to be able to define these types, and declare the resources of those types. Luckily, there is a way to do this in RAML. Resource Types in RAML

```
resourceTypes:
  collection:
    get:
    post:
  collection-item:
    get:
```

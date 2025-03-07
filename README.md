﻿# Generation of Orders

The purpose for this project was to generate training data for a order prediction model. I used __synthetic data generation__ technique using mockaroo in order to generate user data for my app. Here is an example.

```json
{
  "firstName":"Brett",
  "lastName":"Tuxwell",
  "profileImageUrl":null,
  "email":"brett.tuxwell@gmail.com",
  "phoneNumber":"+1 408 371 1098",
  "gender":"FEMALE",
  "age":74,"password":
  "Brett123",
  "defaultDeliveryAddress": {
    "streetLine":"15 Fairfield Hill",
    "postalCode":"95118",
    "city":"San Jose",
    "county":"California",
    "country":"United States"
  },
  "defaultBillingAddress": {
    "streetLine":"15 Fairfield Hill",
    "postalCode":"95118",
    "city":"San Jose",
    "county":"California",
    "country":"United States"
  }
}



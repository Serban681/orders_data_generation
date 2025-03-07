# Generation of Orders

The purpose for this project was to generate training data for a order prediction model. I used __synthetic data generation__ technique using mockaroo in order to generate user data for my app. Here is an example:

```json
{
  "firstName":"Brett",
  "lastName":"Tuxwell",
  "profileImageUrl":null,
  "email":"brett.tuxwell@gmail.com",
  "phoneNumber":"+1 408 371 1098",
  "gender":"FEMALE",
  "age":74,
  "password": "Brett123",
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
```

Much of it is pretty straight forward. But let's take a deeper look two of the fields:
1. The gender can basically have 3 values: MALE, FEMALE and NOT_MENTIONED. Basically, I have used a categorical distribution with the following probabilities:
   - MALE: 0.4
   - FEMALE: 0.4
   - NOT_MENTIONED: 0.2
2. The age is a bit more complicated:
   
   ![image](https://github.com/user-attachments/assets/91e833e4-e9d7-4b15-9652-cde6b3c35749)

   [Age DistributionScript](./user_age_distribution.ipynb)

   

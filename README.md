# Generation of Orders

<details>
  <summary>
    <h2>Users Generation</h2>
  </summary>
  <br />
  The purpose for this project was to generate training data for a order prediction model. I used __synthetic data generation__ technique using mockaroo in order     to generate user data for my app. Here is an example:
  
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
  1. The gender can have 3 values: MALE, FEMALE and NOT_MENTIONED. Basically, I have used a categorical distribution with the following probabilities:
     - MALE: 0.4
     - FEMALE: 0.4
     - NOT_MENTIONED: 0.2
  2. The age distribution is a bit different. Here is a script for it: [Age Distribution Script](./user_age_distribution.ipynb)
     
     ![image](https://github.com/user-attachments/assets/91e833e4-e9d7-4b15-9652-cde6b3c35749)
  
     I used two normal distributions scaled them and concatenated them. Then I created CSVs and added them to mockaroo in order to assign the age to each user.   

</details>

<details>
  <summary>
    <h2>Likelihood of buying boxing</h2>
  </summary>
  <br />

  This is the script I am describing: [Likelihood of buying modelling script](./likelyhood_of_buying_gloves.ipynb)

  We want to have a certain amount of orders based on age and gender. 
  We will consider three situations for each gender:
    - The user is a first time buyer
    - The user is a second time buyer
    - The user is buys for the third time or beyond

  Naturally the likelihood decreases as users buy more and more of that specific product. Since we are modelling the likelihood of buying for boxing gloves we will create a higher tendency of buying among young men.

  Here is an example:

  ![image](https://github.com/user-attachments/assets/030964f6-d112-4acd-91ad-b568f8b7e243)

</details>
   

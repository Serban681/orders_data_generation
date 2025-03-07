# Generation of Orders

This repo contains all the necessary parts in order to generate enough data to train an orders prediction model based on user data, previous orders and indiviual characteristics of each user.

<details>
  <summary>
    <h2>Users Generation</h2>
  </summary>
  <br />
  I used synthetic data generation technique using mockaroo in order     to generate user data for my app. Here is an example:
  
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
    <h2>Likelihood of buying boxing gloves</h2>
  </summary>
  <br />

  This is the script I am describing: [Likelihood of buying script](./likelyhood_of_buying_gloves.ipynb)

  We want to have a certain amount of orders based on age and gender. 
  We will consider three situations for each gender:
    - The user is a first time buyer
    - The user is a second time buyer
    - The user is buys for the third time or beyond

  Naturally the likelihood decreases as users buy more and more of that specific product. Since we are modelling the likelihood of buying for boxing gloves we will create a higher tendency of buying among young men.

  Here is an example:

  ![image](https://github.com/user-attachments/assets/030964f6-d112-4acd-91ad-b568f8b7e243)

</details>


<details>
  <summary>
    <h2>User Registration</h2>
  </summary>
  <br />

  This is the script I am describing: [User Registration Over Time](./user_registration_modelling.ipynb)

  With this script I basically wanted to show a tendency of more and more users registrating over time. An exponential growth of user registrations.

  The image below represents the number of users registered in each day.

  ![image](https://github.com/user-attachments/assets/a21d8b11-13b5-42f1-b928-26ed7ffbbc5f)
  
</details>

<details>
  <summary>
    <h2>Orders Modelling</h2>
  </summary>
  <br />

  This is the script I am describing: [Orders Modelling](./boxing_gloves_orders_modelling.ipynb)  

  To achieve the following model I divided each year into four seasons each with it's own function:
    
  - Winter - an exponential function
  - Spring - a logarithmic function
  - Summer - a linear function
  - Autumn - just a random poison distribution

  The image bellow represents the total number of orders over time:
  
  ![image](https://github.com/user-attachments/assets/c8a95c04-32df-4cdc-881a-723c4368ba0d)
</details>

<details>
  <summary>
    <h2>Orders Generation</h2>
  </summary>
  <br />

  This is the script I am describing: [Orders Generation](./order_generation.ipynb)

  Finally we get to use all that data we genreated earlier and combine it into something more practical that we can use to test the API and train the model.

  First I assign to each user a random registration date from the ones generated before.
  ```python
  users_df['registration_date'] = shuffled_registration_dates_list[:len(users_df)]
  ```

  This is the main algorithm that combines both the users and orders data frames in order to asociate a user to each order
  ```python
  def create_orders(buyers_df, orders_df):
    for index, row in orders_date_df.iterrows():
        buyers_df = assign_probability_to_first_time_buyers(row['date'], buyers_df)
  
        if buyers_df is not None and not buyers_df.empty:
            for i in range(int(row['number_of_orders'])):
                userIndex = pick_based_on_probability(buyers_df['probability'].values)
  
                pickedBuyer = buyers_df.iloc[userIndex]
                orderEntry = pd.DataFrame({'date': [row['date']], 'userId': pickedBuyer['id']})
                
                newProbability, newBuyerType = get_probability_by_age_gender(pickedBuyer['age'], pickedBuyer['gender'], pickedBuyer['buyer_type'])
  
                buyers_df.iloc[userIndex, buyers_df.columns.get_loc('probability')] = newProbability
                buyers_df.iloc[userIndex, buyers_df.columns.get_loc('buyer_type')] = newBuyerType
  
                orders_df = pd.concat([orders_df, orderEntry])
  
    return orders_df
  ```

  This is the data we end up with. I can add more details like payment type in the api, but our job is done here as we have succesfully asociated a user to each order.
  ```json
  {
    "date":"2021-03-30",
    "userId":1073,
    "productId":0
  }
  ```
  
</details>

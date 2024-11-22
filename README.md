# Predicting And Segmenting Churned Users In Marketing
Using Machine Learning to predict user churn then segment these users into groups.

## **I. Introduction**

### **1. Situation**

- One ecommerce company has a project on **predicting churned users** and **segment users** in order to **offer potential promotions**.
  
- Data Dictionary:

  ![Data Dictionary](https://github.com/user-attachments/assets/fcbc7520-45a2-4263-92cd-cb1ca220df47)

### **2. Task**

- Build the Machine Learning model for predicting churned users. Choose six Feature Importance from the model that they are the behaviors of churned users. 
 
- Based on six Feature Importance, segment these churned users into groups so that the company offers some special promotions for them.

## **II. Supervised Learning**

### **1. EDA**
#### **1.1. Handle Missing/Duplicate/Incorrect Values** 
- Check

  ![Check](https://github.com/user-attachments/assets/00d813d5-ddc7-4c89-98c7-6efa26e6d46b)

  => Six isna features are numeric columns (Tenure, HourSpendOnApp, CouponUsed, DaySinceLastOrder, OrderAmountHikeFromlastYear, OrderCount, WarehouseToHome)
  + Tenure, HourSpendOnApp, CouponUsed, DaySinceLastOrder have 0 in values => replace 0.
  + OrderAmountHikeFromlastYear do not have 0 in values but it is a percentage increase in order from last year. If the number orders last year = the number orders in this year => can have 0 in values => replace 0.
  + OrderCount do not have 0 in values but it is Total number of orders has been places in last month. If the customer did not buy anything last month => can have 0 in values => replace 0.
  + WarehouseToHome  => cann't have 0 in values => check outlier => WarehouseToHome cover outliers => replace median
 
    ![WarehouseToHome](https://github.com/user-attachments/assets/c114eb45-5909-4822-bc32-be2464f04ea0)
  
- Handle numeric columns
  + Handle when features have 0 in values => replace 0
 
    ![Replace 0](https://github.com/user-attachments/assets/9a9638d7-eeb2-4cfa-afad-bc04bf07b0c1)
  
  + Handle when features cover have outliers => replace Median
 
    ![Replace Median](https://github.com/user-attachments/assets/3225fc91-9e99-4700-aec5-27acce9dbc8a)

#### **1.2. Check imbalanced**

![Check imbalanced](https://github.com/user-attachments/assets/044ea56e-d759-4648-a03d-af89fb174c48)

=> The ratio of label 1 on total is 16% => We can continue with the EDA and ML model 

#### **1.3. Univariate Analysis** 

![Univariate Analysis](https://github.com/user-attachments/assets/2c4d34e8-d182-472a-8a6c-713f4cdbd06a)

![Univariate Analysis 1](https://github.com/user-attachments/assets/e7353d2a-80c4-4e46-9f1a-fe8f0cc8e544)

As shown the unique values of each numeric column, there are 5 columns that have low unique values (less than 10 values), which are CityTier, HourSpendOnApp, NumberOfDeviceRegistered, SatisfactionScore and Complain.

  => HourSpendOnApp, NumberOfDeviceRegistered, SatisfactionScore, Complain have numeric dtype but don't have category meaning.
  
  => CityTier has numeric dtype but has category meaning. => convert to object
  ![Univariate Analysis result](https://github.com/user-attachments/assets/d8d44587-bb3e-438a-bf11-47de56fe3e9f)

### **2. Transforms features**

![Transforms Features](https://github.com/user-attachments/assets/04ee1670-5e85-4655-b0b6-734fee86905f)

### **3. Apply Random Forest model**
- Split train/test set

  ![Split traintest set - SL](https://github.com/user-attachments/assets/419bf1b0-0782-4f1c-b271-763787a45a8f)
  
- Normalization

  ![Normalization - SL](https://github.com/user-attachments/assets/5f57e0c9-a256-497d-9607-e4ccd28de680)

- Random Forest Classifier

  ![Random Forest Classifier](https://github.com/user-attachments/assets/4002850e-9c1d-4895-b7ec-9973263ca0cd)

=> The accuracy index on the test set is 94% that means this model has correct predictions and correct features importance. 

### **4. Choose top 6 features importance to analyze**

- Show Feature Importance from model.

  ![shown features importance](https://github.com/user-attachments/assets/07ce63af-c883-4950-a982-a3a24d1479ca)

  ![result FI](https://github.com/user-attachments/assets/5c8a9bdc-493d-44e4-9584-02eb0ae49cd0)

- From the Feature Important chart, choose the 6 highest features: Tenure, Complain, DaySinceLastOrder, CashbackAmount, MaritalStatus_Single, PreferedOrderCat_Mobile Phone. Get churned users. 

  ![choose 6 FI](https://github.com/user-attachments/assets/5aff37c2-c9cb-4c45-8672-146670ecafe9)

  ![6FI](https://github.com/user-attachments/assets/2a2d1b45-c96e-40c3-9175-ab81d22b1322)

## **III. Unsupervised Learning**

### **1. Dimension Reduction**

![Dimension Reduction 1](https://github.com/user-attachments/assets/f15f85fe-58f8-4784-bfbd-662f42fc6cb1)

![Dimension Reduction 2](https://github.com/user-attachments/assets/84a368e6-a946-4002-b96c-67bb98084a6a)

### **2. Apply K-Means model**

- Choosing K => Choose k=4

- 
  
- Apply K-Means

- 

- Silhouette Score => silhouette_score = 0.5, shows that the data is clustered quite well. 

- 
  
- Distribution of clusters

### **3. GridSearchCV model**


## **IV. Conclusion**
The majority of churned users are:
 - Using Mobile Phone
 - Tenure in ranges from 0 to 20
 - DaySinceLastOrder in ranges from 0 to 20 recent days
 - CashbackAmount	in ranges from 100 to 325

 => According to the distribution, the order from most to few users in the cluster is 1-3-2-0.
1. Cluster 1:
  - CashbackAmount: Large (about 190-250)
2. Cluster 3:
  - CashbackAmount: Smallest (about 100-140)
3. Cluster 2:
  - CashbackAmount: Small (about 140-190)
4. Cluster 0:
  - CashbackAmount: Largest (about >250)

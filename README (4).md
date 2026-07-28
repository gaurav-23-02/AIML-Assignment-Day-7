Assignment 7: Mall Customer Segmentation using K-Means Clustering and PCA
Student Information
Name: Gaurav Kumar
Registration Number: 23BEY10041
Application Number: IN26010961
Email ID: dikagra123@gmail.com
Objective
The objective of this assignment is to perform customer segmentation using the K-Means Clustering algorithm. The dataset is analyzed to identify groups of customers with similar purchasing behavior based on their annual income and spending score. Principal Component Analysis (PCA) is applied to reduce the dimensionality of the dataset and visualize the customer segments in two dimensions.

Dataset Link
Kaggle Dataset:
https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python

Libraries Used
Pandas
NumPy
Matplotlib
Scikit-learn
StandardScaler
LabelEncoder
KMeans
PCA
Methodology
Loaded the dataset using Pandas.
Displayed the first five records and explored the dataset structure.
Identified numerical and categorical features.
Checked for missing values.
Removed the CustomerID column as it is not useful for clustering.
Encoded the Gender column using LabelEncoder.
Standardized the numerical features using StandardScaler.
Applied the Elbow Method to determine the optimal number of clusters.
Trained the K-Means Clustering model using the selected value of K = 5.
Assigned cluster labels to each customer.
Applied Principal Component Analysis (PCA) to reduce the dataset to two dimensions.
Visualized the customer clusters using scatter plots and PCA.
Results
The Elbow Method indicated that 5 is the optimal number of customer clusters.
K-Means successfully grouped customers based on their annual income and spending score.
PCA reduced the high-dimensional data into two principal components, enabling easy visualization of customer segments.
The generated plots clearly showed distinct customer groups that can support targeted marketing strategies.
Conclusion
This assignment demonstrated the use of K-Means Clustering for customer segmentation and Principal Component Analysis for dimensionality reduction. The model successfully identified five meaningful customer groups, allowing businesses to better understand customer behavior and design personalized marketing campaigns. While K-Means is simple and effective, it requires the number of clusters to be specified beforehand and may be sensitive to outliers. PCA proved valuable by simplifying the dataset while retaining most of the important information, making the clusters easier to visualize and interpret.


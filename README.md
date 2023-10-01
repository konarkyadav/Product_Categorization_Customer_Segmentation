Introduction
In today's data-driven world, businesses are constantly seeking innovative ways to optimize product offerings and enhance customer segmentation in the retail industry. This pressing challenge can be effectively addressed through the utilization of big data analytics. To tackle this problem head-on, we have leveraged the UK’s retailer’s transactional dataset, a rich and real-world collection of transactional data obtained from an online retail store. This dataset serves as a valuable source of insights into customer behavior, purchasing patterns, and detailed product information, enabling us to gain a comprehensive understanding of the retail landscape.
Our analysis encompasses two pivotal techniques: product clustering using Yules coefficient and RFM customer segmentation. The utilization of Yules coefficient facilitates the identification of associations between diverse products based on their co-occurrence in customer transactions. This enables us to group together similar products and uncover valuable patterns, thereby aiding inventory management and revealing lucrative cross-selling opportunities.
Moreover, we have employed RFM customer segmentation, a powerful methodology that evaluates customers based on three essential dimensions: recency, frequency, and monetary value of purchases. By categorizing customers into distinct groups through this segmentation technique, we can obtain profound insights into their purchasing behavior, allowing for the development of tailored and personalized marketing strategies.
By applying these cutting-edge methodologies to the leveraged dataset, our primary objective is to extract actionable insights that will empower retailer to optimize their product assortments, enhance customer satisfaction, and elevate overall business performance in the highly competitive retail industry. Through this comprehensive analysis, we aim to equip business with the tools and knowledge needed to thrive in an ever-evolving marketplace.
Initial Analysis
1.	A notable count of approximately 1.4k records lacks a comprehensive description of the corresponding stockkeeping unit. We can curtail the occurrence of such missing descriptions by exploring the mapping between SKUcodes and descriptions derived from alternative records.
2.	Invoices distinguished by the prefix 'C' signify canceled orders, delineating a distinct subset within the dataset.
3.	Our dataset encompasses an array of entries encompassing manual adjustments, damages, postage charges, dotcom charges, and Amazon fees. Notably, their SKUcodes do not commence with a digit, necessitating their exclusion from the product clustering analysis.
4.	Remarkably, the top 10 SKUs collectively contribute to approximately 10% of the overall sales, revealing their significant influence on the revenue landscape. It is worth highlighting that both Dotcom Postage and Postage hold a prominent position within this esteemed roster.
5.	The United Kingdom emerges as a dominant force, accounting for a striking 84% of the total sales. This underscores the imperative to accord the utmost priority to customers hailing from the UK, as they constitute the majority within the customer base.
6.	September 2011 witnessed an impressive 40% surge in sales, more remarkable considering the absence of any newly added SKUs during that period. This notable growth could be attributed to strategic collaborations forged with additional wholesalers, thereby fortifying the revenue streams.
7.	An intriguing observation unveils that the top 20% of SKUs epitomize a staggering 80% of the total sales, exemplifying the Pareto principle in the dynamic retail realm. This realization unveils an opportunity to eliminate underperforming SKUs, streamlining the product portfolio for enhanced profitability.
Notably, SKU number 23084 emerges as a notable presence within both the most frequently purchased units as well as the esteemed roster of revenue-generating SKUs, attesting to its prominence and commercial significance within the dataset.

Analysis
In this section, we will cover different aspects of data analysis such as data cleaning, data preparation, models’ building, and models’ evaluation.
Here is an outline of the steps performed:
•	Data Cleaning:
o	Removed canceled orders and adjusted transactions from the dataset.
o	Filtered out irrelevant columns and retained the necessary ones for analysis.
•	Yules Coefficient and Product Clustering:
o	Prepared the data by creating product combinations that appeared in the dataset.
o	Applied product clustering using both k-means and hierarchical clustering techniques.
o	Examined the results to identify clusters and hierarchical relationships between products.
•	RFM Customer Segmentation:
o	Conducted customer segmentation based on three dimensions: recency, frequency, and monetary value.
o	Calculated recency by determining the latest day a customer visited the retailer.
o	Calculated frequency by counting the number of visits made by each customer.
o	Calculated monetary value by aggregating the total transaction amount for each customer.
•	K-means and DBSCAN Clustering for Customer Segmentation:
o	Calculated the recency, frequency and monetary values of customers and treated it as input for the k-means algorithm.
o	Applied k-means and DBSCAN clustering to group customers into distinct clusters based on their similarity.
o	Analyzed the results to understand different customer segments and their characteristics.
Through this analysis, we learned the following insights beyond the basic analysis:
•	Hidden Patterns: Despite the initial appearance of a relatively shallow dataset, our analysis using Yules coefficient and clustering techniques revealed hidden patterns and relationships among products. This demonstrates the importance of exploring deeper insights beyond surface-level observations.
•	Product Hierarchy: The hierarchical clustering approach allowed us to create a hierarchy of products based on their relationships. This insight can be leveraged for inventory management, cross-selling strategies, and optimizing product assortments.
•	Customer Segmentation: By employing RFM customer segmentation and k-means clustering, we successfully grouped customers into distinct clusters based on their recency, frequency, and monetary value. This enables targeted marketing strategies, personalized promotions, and tailored customer retention initiatives.

During our preliminary analysis, we discerned that the transactional data harbored pertinent information pertaining to adjustments made on the balance sheet. Amongst these adjustments were distinct components such as .com postage fee, amazon fee, and debts, which necessitated meticulous removal before proceeding with any transformative processes. Furthermore, we expunged the inclusion of gift cards, as their association with specific “StockCodes” was nonexistent. Subsequently, we embarked upon the computation of the Yules coefficient of association, a statistical measure that quantifies the degree of dependence between categorical variables, facilitating the subsequent implementation of diverse clustering methodologies.
Moreover, in our pursuit of customer segmentation, we initially opted to employ the k-means clustering algorithm. This choice was guided by the inherent characteristics of the dataset, wherein one of the categorical attributes, namely "Country," displayed a diminished variability, predominantly comprised of customers originating from the United Kingdom. Consequently, the utilization of k-means clustering as an initial approach would facilitate the formation of distinct customer segments, enabling enhanced targeting and tailored marketing strategies.

Data Cleaning
After initializing Pyspark, we performed data cleaning using PySpark to remove canceled orders and adjusted transactions. This step helps in ensuring the accuracy and integrity of the data, providing a reliable foundation for further analysis. You can refer figure 1 for seeing the data cleaning.
 
Figure 1: Data Cleaning code

Product Clustering
Figure 2 presents an insightful overview of the Yules coefficient association technique employed for product clustering. It offers a visual representation of the intricate interplay between two products, highlighting their associations through the analysis of transactional combinations. By delving into this illustration, we gain a deeper understanding of the underlying relationships and connections between various products within the dataset. Check figure 2 and 3.
 
Figure 2: Yules coefficient demonstration
 
Figure 3: Yules coefficient formula
The underlying concept behind the utilization of the Yules coefficient was to discern and aggregate products based on their inherent similarity. To achieve this objective, we computed a square matrix encompassing the Yules coefficient for every conceivable pair of Stock codes within the dataset. Subsequently, we performed the k-means algorithm iteratively, employing varying values of k, with the aim of identifying the optimal number of product clusters. The outcomes derived from the k-means clustering were subject to analysis through two distinct methodologies: silhouette analysis and the elbow curve method. These techniques provided valuable insights into the effectiveness and cohesion of the clustering results. Refer figure 4.
 
Figure 4: Silhouette and Elbow curve analysis for product clustering
After performing the methods mentioned above, we inferred that there is a discrepancy between the optimal value of k suggested by the elbow method and the decreasing silhouette scores with an increasing k value. Such discrepancies can make it challenging to determine the appropriate number of clusters. In such cases, it is important to consider additional factors and interpret the results cautiously. Here are a few possible interpretations: 
•	Complex Data Structure: The dataset may have a complex structure that does not conform well to the assumptions of the k-means algorithm. The decreasing silhouette scores could indicate that the data points are not well-separated into distinct clusters or that the clusters become more overlapping as the number of clusters increases. In such cases, k-means might not be the most suitable algorithm, and alternative clustering methods or dimensionality reduction techniques could be explored. 
•	Overfitting with Elbow Method: The elbow method examines the rate of decrease in inertia as the number of clusters increases. It is possible that the elbow method suggests a higher k value (e.g., 5) due to the decrease in inertia, but the silhouette scores indicate that the resulting clusters are not well-separated. This could be an indication of overfitting, where the clusters become too specific and do not capture the underlying patterns or true structure of the data.
Keeping in mind the above reasons we chose Hierarchical clustering.

Customer Segmentation
 
Figure 5: Customer Segmentation
Figure 5 provides a comprehensive depiction of the customer segmentation analysis conducted on the dataset under investigation. The objective of this study was to ascertain an effective approach for segmenting customers, thereby enabling the retailer to devise tailored marketing campaigns and strategies tailored to their distinct target audiences. By delving into this analysis, we aimed to deliver profound insights into the underlying dynamics of the customer base, facilitating the identification of specific customer segments characterized by unique needs and behaviors. Armed with these data-driven findings, the retailer can make informed decisions to amplify customer satisfaction, bolster customer retention, and elevate the overall performance of the business. 
	To conduct customer segmentation, we initially determined the recency values, which indicate the number of days since customers' most recent transactions with the retailer. Additionally, we computed the frequency column, representing the count of purchases made by each customer, and the monetary value, denoting the total amount of money spent by customers at the store. Subsequently, in the first round of the k-means algorithm, we used standardized (normalized) values of the actual continuous attributes - recency, frequency, and monetary. We iteratively used the k-means algorithm by varying the k value parameter from 2 to 11 and plotted the silhouette score and inertia to identify the optimal value of k. Figure 6 shows the results of the output.
 
     Figure 6: Optimal clusters choosing output.
From the figure 6, we observed that the optimum value of k is 2, if the clustering is performed using normalized actual attributes. Upon further examination of data by splitting them into two clusters, we noticed that the presence of outliers was heavily influencing the creation of clusters. All the outliers were getting clubbed into one cluster while the algorithm was failing to identify the patterns amongst other data points. Figure 7 are the cluster sizes when k was chosen to be 2.
 
Figure 7: Clustering result with standardized data as input
Hence, to mitigate the impact of outliers, we created pentiles of RFM and performed clustering on those pentiles rather than those actual continuous variables. The clustering outcomes were then subjected to analysis using two established methodologies: silhouette analysis and the elbow curve method. These techniques furnished valuable insights into the coherence and efficacy of the clustering results, aiding in the interpretation of the segmentation process. For a visual representation of this analysis, kindly refer to Figure 8.
 
Figure 8: Silhouette and Elbow curve analysis for customer segmentation
From the figure 8, we inferred that the optimal number of clusters to be chose for clustering is 4.
In our exploration of DBSCAN, we conducted a thorough examination of various parameter combinations. Initially, when we set the minimum sample size to 100 and experimented with different epsilon values ranging from 0.01 to 0.1, the algorithm suggested a staggering 14 clusters, which seemed implausible given the dataset. However, upon increasing the minimum sample size to 300, the algorithm recommended a more reasonable 2 clusters. However, 3.7k out of 4.3k customers were not assigned to any cluster as these were considered as noisy points. Additionally, we observed that the silhouette score did not exhibit significant improvement with variations in the epsilon value. Conversely, in the case of k-means clustering, we witnessed an enhancement in the silhouette score with 4 clusters, prompting us to proceed with this configuration. Refer figure 9.
 
Figure 9: DBSCAN output for Customer segmentation

	To tackle the drawbacks of DBSCAN and k-means (with normalized actual data), we decided to form our marketing strategies for the four clusters that were identified using k-means algorithm based on pentiles of RFM metrics.
Conclusion
During the process of product clustering, we successfully identified five significant categories that emerged from the analysis:
•	The Eclectic Gift Assortment: This category encompasses a diverse range of products tailored for various occasions such as birthdays, Valentine's Day, and more.
•	The Christmas and Vintage Collection: This cluster specifically consists of Christmas-themed products, forming a small but distinct group.
•	The Vibrant and Whimsical Gift Collection: This category comprises an array of fancy and exotic items, including unique pieces like Chinese dragon portraits and Cinderella key rings. 
•	The Gift Wrap and Decor Collection: Within this cluster, customers can find an assortment of home decor products, including wall clocks and pillow covers, offering an opportunity to enhance the ambiance of living spaces.
•	The Kitchen and Stationery Essentials with Sweet Delights: This particular category is dedicated to kitchen-related products, such as scissors, knives, kitchen towels, alongside delightful sweet treats.
Now armed with these clearly defined clusters, the retailer can effectively categorize and optimize their product catalog on their website. Furthermore, by delving into the performance of each SKU within these clusters, the retailer can address assortment optimization concerns, identifying and potentially eliminating underperforming items. Refer figure 10 for the visual.
Level1	Gifts (4045 SKUs)
Level2	Merry Vintage Whimsy Collection (2618 SKUs)	Charming Treasures: Gift Wrap, Kitchen, Stationery & Sweet Delights Collection (1427 SKUs)
Level3	Festive Vintage Gift Melange (1504 SKUs)	Vibrant and Whimsical Gift Collection (1114 SKUs)	Charming Treasures: Gift Wrap, Kitchen, Stationery & Sweet Delights Collection (1427 SKUs)
Level4	Festive Vintage Gift Melange (1504 SKUs)	Vibrant and Whimsical Gift Collection (1144 SKUs)	Gift Wrap and Decor Collection (687 SKUs)	Kitchen and Stationery Essentials with Sweet Delights (740 SKUs)
Level5	Eclectic Gift Assortment (1110 SKUs)	Christmas and Vintage Collection (394 SKUs)	Vibrant and Whimsical Gift Collection (1114 SKUs)	Gift Wrap and Decor Collection (687 SKUs)	Kitchen and Stationery Essentials with Sweet Delights (740 SKUs)

Figure 10: Product hierarchy using clustering
During the comprehensive customer segmentation analysis, we employed meticulous techniques to identify and classify customers into four distinct clusters, each characterized by their unique traits and purchasing patterns:
•	Platinum Elite Customers: This cluster consists of loyal, valuable customers who regularly engage with the online store. They epitomize brand loyalty, making frequent visits and high-value transactions, driving substantial sales. To enhance brand advocacy and loyalty, offer exclusive rewards, personalized recommendations, and VIP perks.
•	Occasional/Dormant Shoppers: Contrasting the Platinum Elite Customers, this cluster comprises infrequent visitors who make minor purchases. Their sporadic engagement and limited spending minimally contribute to sales. To re-engage them, use targeted promotions, limited time offers, and personalized incentives to increase their visit and purchase frequency.
•	Moderate Engagers/New Customers: This cluster includes new customers showing promising involvement and making noteworthy transactions. They offer growth potential and require targeted marketing. Nurture their interest with personalized onboarding, tailored recommendations, and promotions to accelerate their transition to loyal customers.
•	Dedicated Shoppers: In this cluster, frequent visitors show consistent engagement. While their transactions may be smaller than Platinum Elite Customers, their presence generates steady sales. Maintain their loyalty with loyalty programs, discounts, and personalized communication to maximize lifetime value and encourage repeat purchases.
Platinum Elite Customers: Dominant segment (28% of customer base, 73% sales). Occasional/Dormant Shoppers: 30% customer base, 5% sales. Tailor initiatives, allocate resources for deepening engagement with Platinum Elite Customers and re-engaging Occasional/Dormant Shoppers. Figure 9 shows cluster differences in recency and frequency.
 
Figure 11: Visual showing relationship between Recency and Frequency for all the clusters
![image](https://github.com/konarkyadav/Product_Categorization_Customer_Segmentation/assets/15073072/9997272e-9da1-4ea2-b387-6c3813db393a)

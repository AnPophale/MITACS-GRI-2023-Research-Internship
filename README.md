# MITACS GRI 2023 Summer Research Internship 
### Fuzzy clustering based Partial Least Squares (PLS) modelling for microgel property prediction

I was one of the top 1000 students from India selected for the MITACS Globalink Research Internship 2023 to pursue a fully funded summer research internship at McMaster University, Canada. I worked on data driven modelling for microgel property prediction under Prof. Prashant Mhaskar in the Department of Chemical Engineering at McMaster University The final project presentation is available [here](https://github.com/AnPophale/MITACS-GRI-2023-Research-Internship/blob/main/Project%20Presentation.pdf). A brief explanation of the project is given below:

**What are microgels?**  
Microgels are colloidal gels, which are 3D cross linked polymer networks suspended in a solvent. For this project, we model the properties of pH and temperature multi-responsive microgels which are used as drug carriers in drug delivery applications. The Volume Phase Transition Temperature (VPTT) is an important property of these microgels which is the temperature at which they undergo a swelling/de-swelling transition.  

It is difficult to predict such properties of microgels from first principles based models because of the complex dynamics, difficult to measure parameters and coupling between physical and chemical mechanisms affecting the VPTT. Hence, a latent variable based data driven approach is used to model the VPTT directly in terms of the chemical composition used to synthesize the microgel which we call as the recipe. With such a model, a model inversion will allow us to predict the recipe for application tailored microgels. 

**Partial Least Squares Modelling:**  
Partial Least Squares (PLS) modelling is a latent variable based approach that projects both the independent and dependent variables onto a lower dimensional space. This approach generates two key components:
* Loadings which are used to transform data between the original and latent spaces.
* Scores which represent the data in this reduced latent space.

The decomposition of the variables (X and Y) into scores (t,u) and the corresponding loadings (P,Q) is shown in Fig. 1 and the further explained in the following sections. The relation between the scores in the latent space is known as the inner PLS relation, while the relation between the original variables is known as the outer PLS relation. 

<p align="center">
  <img src="https://github.com/user-attachments/assets/f1045041-356e-4c31-9576-d66306039f66" alt="Fig 1: Partial Least Squares approach" style="width: 40%;">
</p>
<p align="center">
  <em>Figure 1: Partial Least Squares approach</em>
</p>

Steps involved PLS modelling:
* Dimensionality Reduction: The independent and dependent variables are projected onto a lower dimensional space in a way which maximizes the correlation between the generated PLS scores.
* Linear Regression: A linear regression is performed on these PLS scores. The model is then mapped back to the original dimensions using the loadings.
* Predictions: During prediction, the previously computed PLS loadings are used to project new independent data into the score space. The predicted scores for the dependent variables are calculated using the regression model, and these scores are then transformed back into the original space using the loadings.

This procedure is similar to Principal Component Regression (PCR) but the key difference between PCR and PLS is the generation of scores. In PCR, the variables are projected to maximize their individual variance while PLS maximizes the covariance between the variables. The Nonlinear Iterative Partial Least Squares (NIPALS) algorithm is used to implement the PLS model in MATLAB. This procedure is described in Fig. 2, 
where X and Y are the independent and dependent variables, b is the linear regression parameter, u and t are the PLS scores and w, p, q are the PLS loadings. h which is the number of PLS factors is a key parameter which needs to be tuned in PLS models and it represents the number of times the data is projected onto a lower dimensional latent space.

<p align="center">
  <img src="https://github.com/user-attachments/assets/16b91f6b-a2a4-4e60-a1c6-2ede10ea5e21" alt="Fig 2: The NIPALS Algorithm" style="width: 80%;">
</p>
<p align="center">
  <em>Figure 2: The NIPALS Algorithm</em>
</p>

**Modelling Strategy:**  
The swelling profile which represents the temperature vs size of the microgel is used as the dependent variable while the composition of different chemicals used for the synthesis of the microgel is used as the independent variable. A PLS model is used to predict the swelling profiles based on this data and then using a sigmoid curve fit, the transition temperature is calculated where a sharp change is observed in the profile. This procedure is repeated at 2 different pH values as this transition is also pH dependent. The dataset which was used for the development of these models is an experimental dataset and hence is limited in size.

<p align="center">
  <img src="https://github.com/user-attachments/assets/3b799e0d-402d-48cf-85ae-9d54b2562c10" alt="Fig 3: The proposed modelling approach" style="width: 75%;">
</p>
<p align="center">
  <em>Figure 3: The proposed modelling approach</em>
</p>

The first method is a simple PLS approach to predict the swelling profiles as described before. In [1] it was shown that using a clustering based PLS model can outperform a standard PLS model. This uses a k means clustering approach which first categorizes the data into clusters so that each data point lies in only one cluster and then several independent PLS models are developed amongst these clusters. In case of predictions, first the independent variable is assigned to an appropriate cluster and then the corresponding PLS model is used for predictions. In such clustering based PLS models, the number of clusters c is a key parameter along with h, the number of PLS factors.

<p align="center">
  <img src="https://github.com/user-attachments/assets/87ca7ee5-694a-4bb4-9d70-d9ce0cd8f1ee" alt="Fig 4: Hard vs Soft Clustering" style="width: 75%;">
</p>
<p align="center">
  <em>Figure 4: Hard vs Soft Clustering</em>
</p>

In this project, the key idea is to test if fuzzy clustering performs better than k means clustering in this approach. k means clustering is a type of hard clustering in case of which each data point belongs to only one cluster. On the other hand, fuzzy or c means clustering is a soft clustering technique wherein each data point has a certain probability of being in each of the cluster which generates a membership matrix for all data points and clusters. This difference between k means and fuzzy clustering in highlighted in Fig. 4. Further, we use a weighted average of the predictions from different PLS models using the membership matrix. In case of fuzzy clustering another important parameter is the f, which affects the amount of overlap between clusters. A f value of 0 represents no overlapping between clusters which is equivalent to k means clustering. Both the clustering methods follow an iterative algorithm to calculate the cluster centers based on the minimization of an objective function based on the distances of all data points from the cluster centers.

Based on the previous discussion, in this work we compare the performance of following types of PLS models:
* Linear PLS
* K Means PLS (As suggested in [1])
* Inner Fuzzy PLS (From literature) - Fuzzy clustering in applied to the inner PLS relation (relation between the scores in the latent space) and each data point (scores) is assigned to the their most probable cluster.
* Outer Fuzzy PLS (Our suggestion) - Here, data in the original space is weighted using the membership matrix, so each PLS model is influenced by all data points scaled by their membership. This can be thought of as an extension of the k means approach if it is formulated in terms of the membership function (which would include only zeros and ones). This is further depicted in Fig. 3 given below.

<p align="center">
  <img src="https://github.com/user-attachments/assets/717f62c9-9e14-427a-bf2b-2695b2de9ec7" alt="Fig 5: Fuzzy clustering based PLS model suggested in this study" style="width: 80%;">
</p>
<p align="center">
  <em>Figure 5: Fuzzy clustering based PLS model suggested in this study</em>
</p>

* Outer Fuzzy PLS 2 (From literature) - Fuzzy clustering applied to the data in the original space and each data point is assigned to the cluster in which it has highest membership value.

**Results and Conclusions:**  
As the training data is limited, a jackknife approach is used, where one observation is excluded from the dataset during training and later used for testing. This ensures that the model is tested on an unseen sample, and this process is repeated for all data points. Another approach used is similar to a k-fold cross validation, where we separate out k observations from the data for testing and use the rest of the data for training. In each of the methods different combinations of the parameters h, c, and f are used as defined previously and the parameters with the best overall results are selected.

Some of the results using the above methods are given in the figures below:

<p align="center">
  <img src="https://github.com/user-attachments/assets/fae5d3b3-f8bc-41c0-8c34-e525be698b88" alt="Fig 6: Comparison of different methods in predictions of swelling profiles" style="width: 80%;">
</p>
<p align="center">
  <em>Figure 6: Comparison of different methods in predictions of swelling profiles</em>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/f152a635-ddca-4391-81ff-f1478698467b" alt="Fig 7: Comparison of different methods in predictions of the VPTT" style="width: 80%;">
</p>
<p align="center">
  <em>Figure 7: Comparison of different methods in predictions of the VPTT</em>
</p>

The results obtained show that our suggested method outperforms other methods in the Jackknife Approach. But in the k-fold approach, the results for our method are similar to the other methods used, the results being dependent on the selected testing data which could be because of the size of the data and the train test split. Hence, further investigation is needed on how the performance of the models can be compared in a better way with the constraint of having a limited dataset.

**References:**  
[1] Tayebi, S. S., Keane, E., Preciado Rivera, N., Hoare, T., & Mhaskar, P. (2022). Predicting the Volume Phase Transition Temperature of Multi-Responsive Poly(N-isopropylacrylamide)-Based Microgels Using a Cluster-Based Partial Least Squares Modeling Approach. ACS Applied Polymer Materials, 4(12), 9160–9175. https://doi.org/10.1021/acsapm.2c01487.

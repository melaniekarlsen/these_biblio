---
citekey: janssenApplicationSHAPValues2022
aliases: ["Janssen et al. (2022) Application of SHAP values for inferring the optimal functional form of covariates in pharmacokinetic modeling"]
title: "Application of SHAP values for inferring the optimal functional form of covariates in pharmacokinetic modeling"
authors: Alexander Janssen, Mark Hoogendoorn, Marjon H. Cnossen, Ron A. A. Mathôt, for the OPTI-CLOT Study Group and SYMPHONY Consortium
year: 2022
publisher: "CPT: Pharmacometrics & Systems Pharmacology"
doi: 10.1002/psp4.12828
---
# [Application of SHAP values for inferring the optimal functional form of covariates in pharmacokinetic modeling](zotero://select/library/items/MV7VX34N)

> [!info]- Info - [**Zotero**](zotero://select/library/items/MV7VX34N) | [**DOI**](https://doi.org/10.1002/psp4.12828) | [**PDF**](file:////homemtp/u1018748/Zotero/storage/P95NLQFB/Janssen%20et%20al.%20-%202022%20-%20Application%20of%20SHAP%20values%20for%20inferring%20the%20optim.pdf)
>
> **Bibliography**: Janssen, Alexander, Mark Hoogendoorn, Marjon H. Cnossen, Ron A. A. Mathôt, and for the OPTI-CLOT Study Group and SYMPHONY Consortium. “Application of SHAP Values for Inferring the Optimal Functional Form of Covariates in Pharmacokinetic Modeling.” _CPT: Pharmacometrics & Systems Pharmacology_ 11, no. 8 (2022): 1100–1110. [https://doi.org/10.1002/psp4.12828](https://doi.org/10.1002/psp4.12828).
> 
> **Authors**::  [[Alexander Janssen]],  [[Mark Hoogendoorn]],  [[Marjon H. Cnossen]],  [[Ron A. A. Mathôt]],  [[ ]]
> 
> 
> 
> **Collections**:: [[Covariable model building]]
> 
> **First-page**: 1101

> [!abstract]-
> 
> In population pharmacokinetic (PK) models, interindividual variability is explained by implementation of covariates in the model. The widely used forward stepwise selection method is sensitive to bias, which may lead to an incorrect inclusion of covariates. Alternatives, such as the full fixed effects model, reduce this bias but are dependent on the chosen implementation of each covariate. As the correct functional forms are unknown, this may still lead to an inaccurate selection of covariates. Machine learning (ML) techniques can potentially be used to learn the optimal functional forms for implementing covariates directly from data. A recent study suggested that using ML resulted in an improved selection of influential covariates. However, how do we select the appropriate functional form for including these covariates? In this work, we use SHapley Additive exPlanations (SHAP) to infer the relationship between covariates and PK parameters from ML models. As a case-study, we use data from 119 patients with hemophilia A receiving clotting factor VIII concentrate peri-operatively. We fit both a random forest and a XGBoost model to predict empirical Bayes estimated clearance and central volume from a base nonlinear mixed effects model. Next, we show that SHAP reveals covariate relationships which match previous findings. In addition, we can reveal subtle effects arising from combinations of covariates difficult to obtain using other methods of covariate analysis. We conclude that the proposed method can be used to extend ML-based covariate selection, and holds potential as a complete full model alternative to classical covariate analyses.
> 

> [!quote]- Citations
> 
> ```query
> content: "@janssenApplicationSHAPValues2022" -file:@janssenApplicationSHAPValues2022
> ```

---
## Overall Comment(s):
In this study, they fosud on tree-based ML algorithms (as their exists an exact method for the computation of SHAP values for these types of models). Specifically, they used a RF and an XGBoost. Goal: evaluate the value of combining ML + SHAP for enriching ML-based covariate analysis in the context of PK models. They used RF and XGB to predict empirical Bayes estimates of the PK parameters. Then they performed SHAP on the most accurate model, which was RF.
Dataset used: retrospective dataset of patients with hemophilia A receiving clotting factor VII 5FVIII) while undergoing surgery. 13 covariates were chosen for analysis. Five of them contained NA, which were imputed by mean (cont) or addition of a separate category (cat).
Empirical bayes estimates (target of the tree-based models): obtained by fitting a base two-compartment model to the data using NONMEM. 
Random effects were only estimated for the clearance and central volume.
RF was fit to predict imputed dataset, XGBoost was fit to predict imputed dataset & the non-imputed one. 
10-fold-CV for estimation of test set error.... THAT'S BAD ML PRACTICE!!!!!!!!!!!




# What could be done differently in this article:
* TEST YOUR METHOD ON AN EXTERNAL DATASET!!!!!!!!!!!!! THERE IS NO UNBIASED ERROR ESTIMATE OF THE MODEL HERE IN THIS ARTICLE!!!!!!!
* !!!!!!!!!!! Why computing the RMSE (or whatever) of predicted FVIII levels by solving the two-compartment model using the test set predicted PK parameters ??? You're checking if your method is able to imitate the base model well, here. ACTUALLY, why even chosing the Bayes estimates of the PK parameters as a target in the first place ????? You want to minimize the errors on the concentration! not on a previously-found model !!!!!!!!!!!
  ===> OK I got it! I'm stupid. When they will perform predictions on new patients, that's exactly the pipeline that they're going to use. So they want to make sure it works. Makes sense LOL
---
## Reading notes
%% begin annotations %%

*Imported on 2023-11-19 23:38*

### 🧩 Definitions and concepts %% fold %%

- & In stepwise methods, covariate selection is determined by a significant change in the objective function value following inclusion or exclusion of each covariate. [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=5Z7SVBH6) 
- **Lire ces refs car de quel biais parlent-ils ???**:
	- & Due to the ordered nature of this process, the method may suffer from bias and multiplicity issues.1–3 [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=N9D6VQYL) 
- **???**:
	- & The full fixed effects model (FFEM), which is based on a full model fit, was introduced to reduce selection bias.4 In this method, all covariates of interest are tested simultaneously and included if they result in a clinically relevant change of the typical PK parameters. Although an improvement over the stepwise method, the FFEM is not able to solve all prior issues. In both methods, an assumption must be made about the functional form describing the relationship between the covariate and the PK parameters. [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=CUFFHLL6) 
- & In summary, we identify a need for a covariate selection method which performs both a full model fit, while simultaneously estimating the optimal functional form of each covariate. [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=5FCVUVMX) 
- **Read that!!!**:
	- & A recent study describes the use of machine learning (ML) for performing covariate selection for PK models.5 Here, the authors discuss how combining ML algorithms with covariate importance scores can be used to obtain a similar or better selection of covariates compared to stepwise methods. [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=92QDWIJM) 
- & These methods might thus reduce the issue of selecting suboptimal functional forms when testing covariates for inclusion. [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=SMM94ZFP) 
- **Tree-based methods may be biased (when giving covariate importance)**:
	- & For tree-based methods (e.g., random forests8 or gradient boosting trees9), examples include counting the number of uses of each covariate, or more sophisticated measures, such as Gini or permutation importance. Although often found to be relatively accurate, there are situations where these measures may be biased.10 [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=XVCDKXCW) 
- **Right!!!**:
	- & In addition, they only provide a single score of importance without information about the relationship between each covariate and model output. After obtaining a set of important covariates, how do we now select the functional form to implement these covariates without again resorting to stepwise methods? [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=RTGYVRPW) 
- & The use of SHAP might improve upon importance scores by also allowing for the analysis of the relationship between covariates and model output. Its use for covariate selection has, however, not yet been explored. [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=3YHP7XNX) 
- & A decision tree is an algorithm that groups observations into bins WHAT QUESTION DID THIS STUDY ADDRESS? Can we use ML models to infer the optimal functional form to represent the relationship between covariates and PK parameters using SHapley Additive exPlanations? WHAT DOES THIS STUDY ADD TO OUR KNOWLEDGE? This study presents an extension to covariate selection procedures using ML methods. The resulting framework allows for the detection of intricate effects of covariates, which far exceed the capabilities of classical (linear) covariate analyses. In addition, it is more flexible with respect to covariate importance scores generally used in ML-based covariate selection. HOW MIGHT THIS CHANGE DRUG DISCOVERY, DEVELOPMENT, AND/OR THERAPEUTICS? By learning the optimal functional form of covariates based on data the complexity of covariate selection and implementation is reduced. This accelerates PK model development and might help improve the accuracy of PK parameter estimates. 2 1 6 3 8 3 0 6 , 2 0 2 2 , 8 , D o w n l o a d e d f r o m h t t p s : / / a s c p t . o n l i n e l i b r a r y . w i l e y . c o m / d o i / 1 0 . 1 0 0 2 / p s p 4 . 1 2 8 2 8 b y C o c h r a n e G e r m a n y , W i l e y O n l i n e L i b r a r y o n [ 1 7 / 1 1 / 2 0 2 3 ] . S e e t h e T e r m s a n d C o n d i t i o n s ( h t t p s : / / o n l i n e l i b r a r y . w i l e y . c o m / t e r m s a n d c o n d i t i o n s ) o n W i l e y O n l i n e L i b r a r y f o r r u l e s o f u s e ; O A a r t i c l e s a r e g o v e r n e d b y t h e a p p l i c a b l e C r e a t i v e C o m m o n s L i c e n s [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=BCGXXSJC) 
- & (appropriately called leaves), which share a similar value for the response variable. Each tree is composed of multiple layers, where the observation is split into two leaves based on the value of one of the covariates. In a random forest, the model prediction is averaged over multiple independently fit trees. Each tree is fit using a subset of the data adding stochasticity to the learning process aiming to reduce overfitting. In gradient boosting trees (e.g., XGBoost), the trees are built sequentially, so that additional decision trees are added if they improve the prediction of the previous model ensemble. Each tree is thus fit to improve the mistakes of the previous tree. The objective function also contains a regularization term which penalizes the addition of complex models. In contrast to the classic random forest implementation, XGBoost supports missing values.13 [(p. 1102)](zotero://open-pdf/library/items/P95NLQFB?page=1102&annotation=5WEY2FRR) 

### ⚙️ Methodology %% fold %%

- ! In this study, we will focus on tree-based ML algorithms, as there exists an exact method for the computation of SHAP values for these types of models.12 Specifically, we will use the random forest and XGBoost algorithms. [(p. 1101)](zotero://open-pdf/library/items/P95NLQFB?page=1101&annotation=H88NZAV8) 
- ! Our goal is to evaluate the value of combining ML and SHAP for enriching ML-based covariate analysis in the context of PK models. To this end, we will fit a random forest and XGBoost model to predict empirical Bayes estimates of PK parameters and perform a SHAP analysis on the most accurate model. As a case study, we use a retrospective dataset of patients with hemophilia A receiving clotting factor VIII (FVIII) while undergoing surgery.14 We explore the output of the SHAP analysis and present how it can be used for understanding the relationship between covariates and PK parameters. [(p. 1102)](zotero://open-pdf/library/items/P95NLQFB?page=1102&annotation=FFTAY4XT) 
- ! 13 covariates were chosen for analysis [(p. 1102)](zotero://open-pdf/library/items/P95NLQFB?page=1102&annotation=KGS5FSV3) 
- ! Five covariates contained missing values. Missing values were either imputed by mean (for continuous variables) or addition of a separate category (for categorical variables). [(p. 1102)](zotero://open-pdf/library/items/P95NLQFB?page=1102&annotation=Y53UG93E) 
- ! Empirical Bayes estimates of the PK parameters were obtained by fitting a base two-compartment model to the data using NONMEM (ICON Development Solutions, Ellicott City, MD16). Random effects were only estimated for the clearance and central volume parameters in order to improve model stability. A combined additive and proportional error model was used. [(p. 1102)](zotero://open-pdf/library/items/P95NLQFB?page=1102&annotation=EYVA3S6H) 
- **They mean, the VARIANCES of the res error estimates, right?**:
	- ! We fixed the residual error estimates to 𝜎1 = 0.08 (additive error) and 𝜎2 = 0.17 (proportional error) to improve model stability and shrinkage while matching earlier findings. [(p. 1102)](zotero://open-pdf/library/items/P95NLQFB?page=1102&annotation=A4Q7HHIS) 
- ! Random forest (Python scikit-learn package, version 0.23.2) and XGBoost (Python xgboost package, version 1.4.2) models were fit to predict the empirical Bayes estimated clearance and central volume distribution parameters independently. We fit the XGBoost models to both the original (containing missing values) and imputed data set. We performed a 10-fold cross-validation for the estimation of test set error and for SHAP value calculation. Default model hyperparameters were used (see Material S1 for details). Model accuracy was represented as the average mean absolute error (MAE) ± one SD of PK parameter predictions on the 10 test sets. We also calculated the root mean squared error (RMSE) of predicted FVIII levels by solving a twocompartment model using the test set predicted PK parameters. The empirical Bayes estimated inter-compartmental clearance and peripheral volume parameters were directly used for all patients. FVIII level predictions were performed in the Julia programming language (Julia Computing, Inc., version 1.6.0) using the DifferentialEquations package (version 6.17.1).19 The RMSE was presented as the mean and SD of the RMSE calculated for each individual patient. [(p. 1102)](zotero://open-pdf/library/items/P95NLQFB?page=1102&annotation=BAZZUCEK) 

### ⚙️ Testing data %% fold %%

- $ We fit both a random forest and XGBoost model to predict empirical Bayes estimated PK parameters originating from a base NLME model. The random forest resulted in slightly more accurate PK parameter predictions compared to the XGBoost models. Next, influential covariates can, for example, be selected using importance scores.5 Finally, after performing a SHAP analysis, we are able to examine the relationship between each covariate and the PK parameters in greater detail. [(p. 1106)](zotero://open-pdf/library/items/P95NLQFB?page=1106&annotation=PKAQRYM7) 


%% end annotations %%

%% Import Date: 2023-11-19T23:38:17.122+01:00 %%

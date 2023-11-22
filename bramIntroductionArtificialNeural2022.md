---
citekey: bramIntroductionArtificialNeural2022
aliases: ["Bräm et al. (2022) Introduction of an artificial neural network-based method for concentration-time predictions"]
title: "Introduction of an artificial neural network-based method for concentration-time predictions"
authors: Dominic Stefan Bräm, Neil Parrott, Lucy Hutchinson, Bernhard Steiert
year: 2022
publisher: "CPT: pharmacometrics & systems pharmacology"
doi: 10.1002/psp4.12786
---
# [Introduction of an artificial neural network-based method for concentration-time predictions](zotero://select/library/items/XH6M8EGM)

> [!info]- Info - [**Zotero**](zotero://select/library/items/XH6M8EGM) | [**DOI**](https://doi.org/10.1002/psp4.12786) | [**PDF**](file:////homemtp/u1018748/Zotero/storage/ZD2AJFEN/Bräm%20et%20al.%20-%202022%20-%20Introduction%20of%20an%20artificial%20neural%20network-based.pdf)
>
> **Bibliography**: Bräm, Dominic Stefan, Neil Parrott, Lucy Hutchinson, and Bernhard Steiert. “Introduction of an Artificial Neural Network-Based Method for Concentration-Time Predictions.” _CPT: Pharmacometrics & Systems Pharmacology_ 11, no. 6 (June 2022): 745–54. [https://doi.org/10.1002/psp4.12786](https://doi.org/10.1002/psp4.12786).
> 
> **Authors**::  [[Dominic Stefan Bräm]],  [[Neil Parrott]],  [[Lucy Hutchinson]],  [[Bernhard Steiert]]
> 
> **Tags**: #Read, #Annoted, #DataLitReview, #LSTM, #ANN
> 
> **Collections**:: [[New methods]]
> 
> **First-page**: 746

> [!abstract]-
> 
> Pharmacometrics and the application of population pharmacokinetic (PK) modeling play a crucial role in clinical pharmacology. These methods, which describe data with well-defined equations and estimate physiologically interpretable parameters, have not changed substantially during the past decades. Although the methods have proven their usefulness, they are often resource intensive and require a high level of expertise. We investigated whether a method based on artificial neural networks (ANNs) may provide an alternative approach for the prediction of concentration-time curve to supplement the gold standard methods. In this work, we used simulated data to overcome the requirement for a large clinical training data set, implemented a pharmacologically reasonable network architecture to improve extrapolation to different dosing schemes, and used transfer learning to quickly adapt the predictions to new patient groups. We demonstrate that ANNs are able to learn the shape of concentration-time curves and make individual predictions based on a short sequence of PK measurements. Furthermore, an ANN trained on simulated data was applied to real clinical data and was demonstrated to extrapolate to different dosing schemes. We also adapted the ANN trained on simulated healthy subjects to simulated hepatic impaired patients through transfer learning. In summary, we demonstrate how ANNs could be leveraged in a PK workflow to efficiently make individual concentration-time predictions, and we discuss the current limitations and advantages of such an ANN-based method.
> 

> [!quote]- Citations
> 
> ```query
> content: "@bramIntroductionArtificialNeural2022" -file:@bramIntroductionArtificialNeural2022
> ```

---
## Overall Comment: 

Good definitions in the article.
New method entirely based on LSTMs and dense layers. 
Inputs : an initial CT sequence and some dosing regimen.
Output: the next concentration in that sequence
Training is fast. Training entirely done on simulated data from PBPK model. Predictions made both on simulated and real clinical data. Predictions are good, whether on single-dose or multiple-dose predictions, even with never-seen-before (from the trained dataset) dosing regimens. However, predictions on a new sub-population does require re-training on a data from this new population, however only a small dataset is required thanks to transfer learning. 
Only able to predict a single next concentration: makes the error propagate a bit. 
Aim of this method is very different to those of classical popPK modeling. This one could be mostly used to simulate new unseen dosing regimens, even only with simulated data from PBPK modeling. 
Method could highly benefit from adding patients characteristics as input, both because it currently predicts little interindividual variability and because it could help undergo the need to feed the model with a very dense CT sequence.

---
## Reading notes
%% begin annotations %%

*Imported on 2023-11-11 21:24*

### 🧩 Definitions and concepts %% fold %%

- **Pharmacometrics**:
	- & Pharmacometrics describes the field of quantitative and qualitative analyses of pharmacological data through modeling, for example, the modeling of pharmacokinetic (PK) data from a clinical study.1 It is an integral part in the approval of new pharmaceutical products through health authorities and a key element for personalized dosing. Although the state-of-the-art methods in pharmacometrics have proven their usefulness for many years, few parts of typical workflows are automated, there is a requirement for a high level of expertise of the modeler, and there is room for improvement in terms of efficiency. [(p. 746)](zotero://open-pdf/library/items/ZD2AJFEN?page=746&annotation=L5N77NI3) 
- **ANN**:
	- & ANNs belong to the family of the supervised machinelearning (ML) methods. ANNs are able to approximate linear and nonlinear functions by creating a network of calculation steps and calibrating the model parameters of this network to data. [(p. 747)](zotero://open-pdf/library/items/ZD2AJFEN?page=747&annotation=JINUWS66) 
- **Hidden layers**:
	- & The hidden layers define the calculation steps that lead from the input to the output layer. For different calculations and data types, different types of hidden layers can be used. Two common layer types are densely connected layers and long short-term memory14 (LSTM) layers. Although dense layers are used to handle static data points, LSTM layers are specifically tailored to temporal sequences of data points. [(p. 747)](zotero://open-pdf/library/items/ZD2AJFEN?page=747&annotation=8DNSF6BP) 
- **ANN training**:
	- & The weights of an ANN are adjusted during training, usually using a gradient-based method, to minimize a loss/objective function assessing the difference between predicted and observed outputs. With an increasing number of hidden layers and parameters to calibrate, an increased training data set is required to manage the risk of overfitting. [(p. 747)](zotero://open-pdf/library/items/ZD2AJFEN?page=747&annotation=PSJGJYMY) 
- **Concept of transfer learning**:
	- & If the available data set for a specific problem is not large enough, the concept of transfer learning can be used.16 An ANN previously trained on a similar but not identical [(p. 747)](zotero://open-pdf/library/items/ZD2AJFEN?page=747&annotation=6FRRVG4N) 
- & problem can be retrained on a new data set. Because the previously trained ANN already learned to accomplish a similar task, some of the weights do not need to be adjusted and can be fixed for the retraining. The reduced number of adjustable weights requires a smaller data set for the retraining compared with the initial training. [(p. 748)](zotero://open-pdf/library/items/ZD2AJFEN?page=748&annotation=JPEMZWTB) 

### ⚙️ Methodology %% fold %%

- **One ANN, able to predict C-T profiles with and without dosing events, composed of two subnetworks : a curve network and a dose network**:
	- ! The ANN we illustrate in this work predicts concentration-time profiles at times with and without dosing events. Therefore, we structured the network architecture into two subnetworks. One subnetwork, later referred to as the curve network, is used to describe the concentration at a timepoint when no dose is administered. The other subnetwork, the dose network, is used to describe the concentration increase following a dosing event. [(p. 748)](zotero://open-pdf/library/items/ZD2AJFEN?page=748&annotation=SYJ29EC6) 
- **Curve network**:
	- ! The input to the curve network is a concentrationtime profile (sequence), and the output is the next concentration in this sequence. The curve network is composed of two hidden LSTM layers that decompose a concentration sequence into parameters representing the shape of the sequence (Figure S1). In a subsequent densely connected layer, these parameters are used in a nonlinear combination to predict the concentration at the next time step. [(p. 748)](zotero://open-pdf/library/items/ZD2AJFEN?page=748&annotation=V6WCUCP6) 
- **Dose network**:
	- ! The input to the dose network is a concentration sequence including the peak concentration from the first dosing and the dosing sequence with the doses at each timepoint relative to the first dose. Both inputs are processed with two hidden LSTM layers and one densely connected layer. The vectors resulting from the densely connected layers were concatenated and processed with three additional densely connected layers. This architecture allows the dose network to draw a connection between a dose and the concentration increase after a dose in individual patients. The output of the dose network was added to the output of the curve network (Figure S2). [(p. 748)](zotero://open-pdf/library/items/ZD2AJFEN?page=748&annotation=UWKEWP3F) 
- **The curve network is first trained independently. Objective = MSE, optimizer = Adam

The dose network is trained via the training of the overall network, with the weights of the curve network being fixed.**:
	- ! Curve network training To train the curve network, we included samples from the single-dose and multiple-dose data and split them into sequences of different lengths. The multiple-dose data was split such that for each sequence the last dosing was at least 10 h before the end of the sequence to avoid capturing dosing effects in the curve network, which may be adjusted for drugs with different absorption profiles (Figure S3c). The number of individual sequences generated through this procedure was shown to cover the variability in the training data set. We added a normally distributed proportional error with a mean of 0 and a standard deviation of 0.1 to all sequences and used them as input for the curve network. As target output for the training, we used the concentration one step forward in time relative to the last concentration in the input sequence. We used the Adam-optimizer19 for parameter optimization, a standard optimizer for training ANNs, and mean squared error as the loss function. Dose network training To train the weights of the dose network, we trained the overall network with the weights of the curve network fixed. The multiple-dose data were split into sequences including samples with the last concentration in the sequence located in a range up to 10 data points after a previous dosing to capture the effect of a new dose (Figure S3b). These sequences served as input to the fixed curve network. The inputs to the dose network were the concentration sequence and the dosing sequence as described in the ANN Architecture section. [(p. 748)](zotero://open-pdf/library/items/ZD2AJFEN?page=748&annotation=9JZ9NHNA) 

### ⚙️ Testing data %% fold %%

- **Trained 10 different ANNs, all from simulated data**:
	- $ To assess the sensitivity of the ANN to different training data, 10 ANN variants were trained with 10 different seeds to assemble the random training data. [(p. 748)](zotero://open-pdf/library/items/ZD2AJFEN?page=748&annotation=W9BBYNVH) 
- **Tested predictions on the training data, to see if it works "in general"**:
	- $ With each of these 10 ANNs, we predicted the complete concentration-time curve for the subjects from the training data set to test whether the neural networks are able to approximate the shape of PK concentration-time curves in general. We inspected the predicted concentrations one [(p. 748)](zotero://open-pdf/library/items/ZD2AJFEN?page=748&annotation=PTBB5DYF) 
- $ 10, and 50 time steps ahead for the single-dose data and at the trough and the peak predictions of the third, fourth, and fifth doses for the multiple-dose data. [(p. 749)](zotero://open-pdf/library/items/ZD2AJFEN?page=749&annotation=J5V54CLB) 
- **Tested predictions on real clinical data, still with the same trained model (from simulated data)**:
	- $ To investigate the translatability from simulated to observed clinical data, we used a data set of 53 subjects with single-dose administration and six subjects with multipledose administrations.17 Measurements from 0 to 8 h were initially selected as the input sequence. Because the concentrations in the single-dose study were only measured at 0, 1, 2, 4, and 8 h, the concentrations at 3, 5, 6, and 7 h were estimated through logarithmic interpolation. Starting from this input sequence with nine concentrations per subject, the concentration-time curves up to 312  h were predicted through iteratively predicting the next concentration and appending the input sequence with the predicted value. To determine the goodness of fit, the measured concentrations were compared with the predicted values at the corresponding timepoint. [(p. 749)](zotero://open-pdf/library/items/ZD2AJFEN?page=749&annotation=VEVQTVVH) 
- **Tested predictions on new simulated, unseen-before dose regimens**:
	- $ To test whether ANNs are able to extrapolate and make accurate predictions for dose regimens they were not trained on, the PBPK model was used to simulate additional data. The dose regimens for these simulations included an initial dose of 20 mg followed by 10 mg twice daily after 24 h or by 40 mg once every second day. The dosing scheme passed to the dose network was adjusted accordingly to predict the whole concentrationtime curves based on an initial concentration sequence. [(p. 749)](zotero://open-pdf/library/items/ZD2AJFEN?page=749&annotation=8ZC6D4DA) 
- **Predictions made on a new simulated different supopulation. The model must be retrained for every subpopulation!!! Though transfer learning from common patients is used.**:
	- $ To demonstrate this functionality, the PBPK model was used to simulate a data set of patients with hepatic impairment with a decreased clearance rate compared with the original population. This data set was split such that 20 patients were used for the retraining and 50 patients were used to evaluate the retrained ANN. Because hepatic impairment is expected to have no influence on the characteristics of drug absorption, only the curve network was retrained and investigated. However, retraining the dose network would also be possible for other scenarios. During the retraining, the weights in the LSTM layers were fixed and only the weights in the dense layers were adjusted because we assumed that the LSTM layers can also describe the new curve and the main changes must be done in the nonlinear parameter combination part of the ANN. [(p. 749)](zotero://open-pdf/library/items/ZD2AJFEN?page=749&annotation=TSDBZSU8) 

###  %% fold %%

- **Method limit!**:
	-  In the ANN approaches, the parameters are not physiologically relevant, and therefore a new ANN must be trained for a new patient group. To minimize the required data, the ANN trained on the simulated data for common patients was used for transfer learning. [(p. 749)](zotero://open-pdf/library/items/ZD2AJFEN?page=749&annotation=Y7FIH2UI) 

### 💡 Results %% fold %%

- **Worked well on trained dataset (single-dose and multiple-dose)**:
	- @ The predicted concentration-time curves of the ANNs for the simulated single-dose and multiple-dose data are in agreement with the simulated profiles generated using the PBPK model (Figure 2a,b). [(p. 749)](zotero://open-pdf/library/items/ZD2AJFEN?page=749&annotation=Y2YV8KUX) 
- **The error "propagates and accumulates" because for a far-ahead prediction, multiple iterations of predicting the next concentration and appending the output to the next input are done**:
	- @ The single-dose predictions for the concentration one step ahead are very close to the simulated values (Figure 2c). With multiple iterations of predicting the next concentration and appending the input to the neural network with the prediction, the residual between the predicted value and the simulated value increases. This results in a decrease of the regression slope from 0.95 to 0.85 and 0.49 and of the correlation coefficient from 0.98 to 0.94 and 0.67 for onestep ahead, 10-step ahead, and 50-step ahead predictions, respectively (Figure 2c–e). [(p. 749)](zotero://open-pdf/library/items/ZD2AJFEN?page=749&annotation=ZEUMY64Y) 
- **Low predicted interindividual variability: maybe makes sense since no individual parameters were given as input to the model!**:
	- @ Interestingly, the predicted values for different subjects lay in a rather narrow range, whereas the underlying simulated values differ more from each other. This suggests an interindividual variability in the predictions that is too low. [(p. 749)](zotero://open-pdf/library/items/ZD2AJFEN?page=749&annotation=BLB3HSQ3) 
- **Works well on predicting real single-dose data! (Still with the model being trained on simulated data)**:
	- @ For a similar analysis using observed clinical patient data, the range of predictions of the 10 neural networks covered the observed concentrations in most cases (Figure  3a–c). The ANNs were not only able to make good predictions for one part of the curve but also for the entire profile. [(p. 750)](zotero://open-pdf/library/items/ZD2AJFEN?page=750&annotation=ST2BQTQ5) 
- **Works well on predicting real multiple-dose data too! (Again, still with the model being trained on simulated data)**:
	- @ The concentration-time curves were predicted for a multiple-dose schedule using the 10 neural networks (Figure  3d–f). With multiple dosing, we noted a slightly lower R2 of 0.75 compared with the single-dose predictions. Nevertheless, the observed concentrations were within the prediction range of the neural networks for most subjects, and the ANNs were able to predict the accumulation and the steady-state concentration. [(p. 750)](zotero://open-pdf/library/items/ZD2AJFEN?page=750&annotation=NX9SD6PA) 
- **Very nice! Predictions were made with only the information on the dosing regimen (and the initial C-T sequence as usual). This test data is also simulated.**:
	- @ The ANNs were also able to make predictions for dose regimens that were not included in the initial training data set (Figure 4) [(p. 750)](zotero://open-pdf/library/items/ZD2AJFEN?page=750&annotation=G3DWG6G6) 
- **For a new population, a little dataset from this population is necessary for retraining (with transfer learning from common population) before making accurate predictions**:
	- @ After the transfer learning with a small data set of 20 hepatically impaired patients, the ANNs clearly showed different predictions. ANN-generated profiles after retraining were much closer to the concentration-time curve of the hepatically impaired patients than the profiles prior to retraining (Figure 5). [(p. 751)](zotero://open-pdf/library/items/ZD2AJFEN?page=751&annotation=JFNQP5TH) 
- **This method is fast!**:
	- @ The training of the ANN was performed within 1 h on a conventional laptop (specification of computational hardware in Table  S3). A prediction of a concentration-time profile with 300 prediction steps takes <30 s. Predictions for multiple subjects can be made in parallel. [(p. 752)](zotero://open-pdf/library/items/ZD2AJFEN?page=752&annotation=AXNPT4JB) 

### ⭐ Discussion %% fold %%

- **This model has not the same aim as conventional popPK modeling**:
	- ~ The ANN-based predictions shown here cannot be compared directly with the predictions of conventional population PK modeling because many of the conventional diagnostic metrics and plots are not applicable (more information in Table  S2). Also, the aim and application of the two methods would not be the same. The conventional approach delivers interpretable predictions and PK parameters that allow the influence of covariates to be explored. This capability is key for individualized as well as population-level predictions and is an established method, which is expected in support of the approval of a new drug. However, these strengths come at the cost of the required expertise and model development time. [(p. 752)](zotero://open-pdf/library/items/ZD2AJFEN?page=752&annotation=FYLTVQ8T) 
- **Limit to the method and potential use of this new method**:
	- ~ However, in the form presented here, they do not provide any rationale for the predictions and cannot be linked to PK processes or patient covariates. Although this limitation may be addressed through, for example, including patient characteristics in the input to the ANN, the current implementation might be more applicable for tasks such as the fast exploration of new data and simple extrapolations to novel dose regimens. Furthermore, the predicted concentration-time curves could allow derivation of secondary PK parameters, such as concentration trough levels (Figure  S4) or the area under the curve to have guidance for decisions. [(p. 752)](zotero://open-pdf/library/items/ZD2AJFEN?page=752&annotation=UDIKN2QT) 
- **This method requires small set of real clinical data, unlike peoples beliefs.**:
	- ~ As a further key outcome of this study, we demonstrated the possibility to train an ANN on a large amount of simulated data and then retrain it on a small set of measured clinical data, thus overcoming the requirement for large clinical training data sets. This invalidates the common perception that ML methods are not available to areas of drug development where data sets are relatively small. [(p. 752)](zotero://open-pdf/library/items/ZD2AJFEN?page=752&annotation=DT9PMCTC) 
- **neural-ODE allows for predictions on an unrestricted continuous time scale. Further improvement of the current article's method might be done in order to have this ability too. Important in order to avoid the propagation of error.**:
	- ~ A different approach that uses ANNs for similar predictions are neural ordinary differential equations (ODEs)20 as also investigated by Lu et al.21 The neural ODE method combines an ANN with an ODE solver that allows predictions on an unrestricted continuous time scale. As well as these benefits, neural ODEs have some drawbacks, and the choice of method may depend on the purpose and the application. [(p. 753)](zotero://open-pdf/library/items/ZD2AJFEN?page=753&annotation=LNMKGBZU) 
- **They also plan on adding some patients characteristics to the model in future work, since the current model doesn't predict a lot of interindividual variability**:
	- ~ On the other hand, the decreasing accuracy may indicate underestimation of the interindividual variability in the predictions (decreasing regression slope in Figure  2c–e and f–h). This could be addressed by basing the predictions on a combination of a concentration input sequence and some patients’ characteristics to increase the information about an individual patient provided to the ANN, which we plan to pursue in future work. [(p. 753)](zotero://open-pdf/library/items/ZD2AJFEN?page=753&annotation=PWP28EYQ) 
- **Big limitation : currently, the model is trained on measurements from a dense and discrete time scale which is unrealistic. To overcome this need, patients characteristics could be fed to the model. Or using neural ODEs!**:
	- ~ One key limitation of the current approach is the discretization of time steps as hourly sampling is not a common clinical practice. The main purpose of this densely measured input sequence is to provide information of an individual patient to the ANN. Similar information could also be given by patients’ characteristics. Providing these characteristics as an additional input to the ANN would allow for a shorter input sequence and reduce the need for a long, densely measured input sequence. Using neural ODEs as mentioned previously may be another approach to address this problem. [(p. 753)](zotero://open-pdf/library/items/ZD2AJFEN?page=753&annotation=AW3PG5Q2) 
- **The (potential) power of transfer learning from simulated data!!**:
	- ~ The results of the predictions for real data demonstrate the feasibility of training a neural network on simulated PK data and applying it to real clinical data. In this example, we used a PBPK model that had previously been shown to successfully simulate observed clinical data, and as a result the transition from simulated to observed data was feasible without any additional refinements of the ANN. PBPK models are often developed preclinically prior to clinical studies and could therefore be used to train a preliminary ANN before retraining with the first clinical data. There is even the prospect that a generic ANN could be trained on a large data set containing simulated data from multiple generic PK models capturing generic PK behaviors, before retraining with drug-specific data. Such speculations will require further investigation of transfer learning,16 which was used in this study to adjust the ANN to a different patient group but might also provide solutions for different challenges. [(p. 753)](zotero://open-pdf/library/items/ZD2AJFEN?page=753&annotation=QHKUH3S3) 


%% end annotations %%

%% Import Date: 2023-11-11T21:24:15.712+01:00 %%

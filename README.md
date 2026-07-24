# R- Social Exchange Model

# Case: Human resources and quality management

## The problem: 
The company want to clarify how quality of interactions, specifically leader-member exchange (LMX) and team-member exchange (TMX), affects employee self-efficacy (EFF) and job satisfaction (SAT), thereby ultimately lead to better quality management (TQM).

**EFA, CFA:** used to discover factors for the quality management and relationships between factors and variables in the survey questions, and to test the model consistency with the data

**SEM:** forming qualitative models, thereby validating the model to interpolate the relationship between factors and their effects on quality management

## EFA

Firstly, create a scree plot of the data by the function scree(dat), collecting the plot below: 
<img width="1004" height="685" alt="image" src="https://github.com/user-attachments/assets/bc0c2c7e-6e57-4a7c-b5f8-a0bc6622c30d" />
Based on the plot, there are five factors whose eigenvalues are over 1, so it can be said that in this case, the appropriate number of factors is 5.
Running KMO(dat), collecting the KMO test result:
Overall MSA =  0.82
```text
 q1     q2     q3   q4   q5     q6   q7     q8     q9  q10  q11  q12  q13  q14  q15  q16  q17   q18  q19  q20  q21  q22   r1     r2    r3    r4    r5   r6   r7 
0.76   0.87   0.74 0.80 0.81   0.85 0.90   0.69   0.80 0.71 0.88 0.83 0.80 0.77 0.71 0.78 0.85 0.84 0.86 0.79 0.85 0.83   0.95 0.84   0.81 0.83   0.92 0.85 0.93
```
The value of KMO test is over 0.5, so the data is sufficient.
Running dev(cov(dat)), which calculates the determinant test, collects the result: 6.433468e-23. The number is too small, so it is not likely to cause multicollinearity.
Because of the assumption of the dependence between factors, the oblique rotation is used for EFA, so the function used in the EFA is fit.efa <- efa(dat, nfactors = 5, rotation = ‘oblimin’) (oblimin is used because of its precision).
Running summary(fit.efa, fit.measures = TRUE), collect the standardized loadings: (* = significant at 1% level)
        f1      	f2      	f3      	f4	     f5      	
q1   	0.863*              
q2   	0.924*                                           
q3   	0.951*                                           
q4   	0.940*                                           
q5   	0.987*              
q6   	0.915*                                           
q7           		0.796*      .       .                    
q8           		0.932*                                   
q9           		0.938*                                   
q10          		0.687*              .       .            
q11          		0.841*                                   
q12         		 0.909*                      .            
q13              .   		0.756*                           
q14                  		0.943*                           
q15                  		0.910*                           
q16                  		0.919*                           
q17                 		 0.930*              .            
q18                          			0.846*      .            
q19                      .   			0.838*      .            
q20                          			0.932*                   
q21                         			 0.978*                   
q22                          			0.873*                   
r1                                   				0.936*           
r2                                   				0.949*          
r3                                  				 0.960*          
r4                                   				0.959*           
r5                                   				0.926*           
r6                                  				 0.980*          
r7                                   				0.938*           
With values over 0.6, it can be safe to say that these values show the association between the factors and variables, specifically:
- f1 is associated with variables q1 -> q6: LMX
- f2 is associated with variables q7 -> q12: TMX
- f3 is associated with variables q13 -> q17: EFF
- f4 is associated with variables q18 -> q22: SAT
- f5 is associated with variables r1 -> r7: TQM

The EFA fosters the theoretical measurement structure of the social exchange model.
2. (CFA) Based on the result of EFA and the assumed variable, establish the CFA model as follows:
model.cfa <- '
LMX =~ q1 + q2 + q3 + q4 + q5 + q6
TMX =~ q7 + q8 + q9 + q10 + q11 + q12
EFF =~ q13 + q14 + q15 + q16 + q17
SAT =~ q18 + q19 + q20 + q21 + q22
TQM =~ r1 + r2 + r3 + r4 + r5 + r6 + r7’
Running the summary(model.cfa, fit.measures = TRUE), collects some important measures:
- CFI: 0.948
- TLI: 0.942
- RMSEA: 0.084
RMSEA is the root mean squared error of approximation. It is considered smaller than 0.05 to indicate better match between the model and data, but in this case it reaches 0.084. However, the CFI and TLI are relatively high. CFI is Comparative Fit Index and Tucker-Lewis Index. If they are over 0.9, it means the model is better and ensures parsimony. Therefore, the model ensures quality of fit.
Running some functions for measures of construct reliability and convergent and discriminant validity:
comprelSEM(fit.cfa):
LMX   TMX   EFF   SAT   TQM 
0.981 0.962 0.971 0.972 0.992 
AVE(fit.cfa):
LMX   TMX   EFF   SAT   TQM 
0.900 0.821 0.878 0.876 0.956 
Htmt(model.cfa, dat): 
LMX   	TMX   	EFF   	SAT   	TQM
LMX	1.000                        
TMX 	0.114 	1.000                  
EFF 	0.271 	0.679 	1.000            
SAT 	0.371 	0.310 	0.181 	1.000      
TQM 	0.394 	0.379 	0.375 	0.682 	1.000
The comprelSEM measures the reliability of the model, which is better with higher values, at least over 0.7. The AVE measures convergent validity, which is better with values higher than 0.5. The HTMT measures the discriminant validity, which is better with values lower than 0.9. Based on the results above, it can be safe to say that the measurement models via CFA are in acceptable ranges regarding construct reliability and convergent and discriminant validity, and do not need any more appropriate actions.
(SEM)
1.	The first model, where all latent variables have a direct effect only on TQM, is demonstrated below:
model.sem1 <- '
LMX =~ q1 + q2 + q3 + q4 + q5 + q6
TMX =~ q7 + q8 + q9 + q10 + q11 + q12
EFF =~ q13 + q14 + q15 + q16 + q17
SAT =~ q18 + q19 + q20 + q21 + q22
TQM =~ r1 + r2 + r3 + r4 + r5 + r6 + r7
TQM ~ LMX + TMX + EFF + SAT’
Fit the model: fit.sem1 <- sem(model.sem1, dat)
Assessing measures of construct reliability and convergent and discriminant validity, collecting the results below:
compRelSEM(fit.sem1): LMX   TMX   EFF   SAT   TQM 
   	    0.981   0.962  0.971  0.972  0.992
       AVE(fit.sem1): LMX   TMX   EFF   SAT   TQM 
                 	           0.900   0.821  0.878 0.876  0.956
      htmt(model.sem1, dat): 
      	LMX   	TMX   	EFF   	SAT   	TQM
LMX 	1.000                        
TMX 	0.114 	1.000                  
EFF 	0.271 	0.679 	1.000            
SAT 	0.371 	0.310 	0.181 	1.000      
TQM 	0.394 	0.379 	0.375 	0.682 	1.000
        Running summary(fit.sem1, fit.measures = TRUE), collecting the indexes below:
- CFI: 0.948
- TLI: 0.942
- AIC: 2502.236
- BIC: 2644.651
The second model where LMX and TMX have direct effects on TQM, EFF and SAT, and EFF and SAT have a direct effect on TQM:
model.sem2 <- '
LMX =~ q1 + q2 + q3 + q4 + q5 + q6
TMX =~ q7 + q8 + q9 + q10 + q11 + q12
EFF =~ q13 + q14 + q15 + q16 + q17
SAT =~ q18 + q19 + q20 + q21 + q22
TQM =~ r1 + r2 + r3 + r4 + r5 + r6 + r7
EFF ~ LMX + TMX
SAT ~ LMX + TMX
TQM ~ LMX + TMX + EFF + SAT’
Fit the model: fit.sem2 <- sem(model.sem2, dat)
Assessing measures of construct reliability and convergent and discriminant validity, collecting the results below:
compRelSEM(fit.sem2): LMX   TMX   EFF   SAT   TQM 
                    0.981  0.962  0.971 0.972  1.010
AVE(fit.sem2): LMX   TMX   EFF   SAT   TQM 
          	     0.900   0.821  0.878 0.876  0.972
htmt(model.sem2, dat):       
LMX   	TMX   	EFF   	SAT   	TQM
LMX 	1.000                        
TMX 	0.114 	1.000                  
EFF 	0.271 	0.679 	1.000            
SAT 	0.371 	0.310 	0.181 	1.000      
TQM 	0.394 	0.379 	0.375 	0.682 	1.000
        Running summary(fit.sem2, fit.measures = TRUE), collecting the indexes below:
- CFI: 0.948
- TLI: 0.942
- AIC: 2500.922
- BIC: 2641.243
All the measures regarding CFI, TLI, comprelSEM, AVE and htmt from the two models are in acceptable ranges, so no reason to eliminate any one for validation. 
AIC is Akaiki Information Criterion, BIC is Bayesian Information Criterion. These numbers measure the likelihood of the data with the given model, and the pair of models with lower indexes would be preferred. From the above results, we can see that the AIC and BIC of model 2 are lower than those of model 1, so model 2 is the better model.
2. Using Model 2, to consider the net effect of TMX, creating a model that validates the mediated effect:
model.net <- '
LMX =~ q1 + q2 + q3 + q4 + q5 + q6
TMX =~ q7 + q8 + q9 + q10 + q11 + q12
EFF =~ q13 + q14 + q15 + q16 + q17
SAT =~ q18 + q19 + q20 + q21 + q22
TQM =~ r1 + r2 + r3 + r4 + r5 + r6 + r7
EFF ~ LMX + a1*TMX
SAT ~ LMX + a2*TMX
TQM ~ b1*EFF + b2*SAT + LMX + b3*TMX
b_ind := a1*b1+a2*b2
b_net := b_ind + b3'
b_ind is to calculate the indirect effect, while the b_net is to calculate the total net effect of TMX
Fit the model: fit.net <- sem(model.net, dat)
After running summary(fit.net, fit.measures = TRUE), collect the results below: 
- CFI: 0.948
- TLI: 0.942
-> The model ensures the quality of fit.
Defined Parameters:
                   Estimate  	Std.Err  	z-value	  P(>|z|)
    b_ind      0.591    	0.215    	2.755	    0.006
    b_net      0.604    	0.218    	2.776	    0.006
The CFI and TLI are over 0.9, so the model is qualitatively fit. Based on the results, it is noticeable that the indirect effect of TMX on TQM, which EFF and SAT mediated, is statistically positive (β = 0.591, p = 0.006 < 0.05, so the null hypothesis of b_ind being zero can be rejected). Also, the total net effect of TMX on TQM is considerable (β = 0.604, p = 0.006 < 0.05, so the null hypothesis of b_net being zero can be rejected). These points out that the team-member exchange affects the quality management directly and by driving the employees’ self-efficacy and job satisfaction.
(3) Managerial implications:
- The EFA and CFA prove the assumption that the quality management strongly follows the social exchange model, which is robustly driven by relationships and interactions in workplaces.
- The model 2 being better confirms that self-efficacy and job satisfaction mediate the relationship between quality of interaction and total quality management, which means that the feeling of confidence and contentment plays an important role for employees to contribute to the quality improvement.
- The mediated model of TMX indicates that stronger teamwork relationships strongly enhance the total quality management both directly and indirectly, emphasizing the essence of colleagues' support and collaboration.
=> From the managers’ perspective, it is essential not to overlook the latent effect of employees’ experience with interactions with leadership and teammates on the quality management. The company should encourage leadership training and especially team cooperation, thereby enhancing the work psychology of employees, ultimately driving direct and indirect improvement of the business quality.

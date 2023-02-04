# User Adoption Challenge

![adopt](notebook/images/adopted_users.jpeg)


## Problem Identification Overview

Companies invest huge amount of money and efforts in technologies, but this would not be profitable if users are not using the tech. The biggest barrier to user adoption is keeping users being attracted to the developed tools that the company has provided.

In this project, I analyzed the pattern of data regarding to the adoption rate, and built a model to predict the adoption rate from user's information.

## Data Inspection

- **takehome_users**
	- name: the user's name
	- object_id: the user's id
	- email: email address
	- creation_source: how their account was created. This takes on one of 5 values:
		- PERSONAL_PROJECTS: invited to join another user' personal workspace
		- GUEST_INVITE: invited to an organization as a guest (limited permissions)
		- ORG_INVITE: invited to an organization (as a full member)
		- SIGNUP: signed up via the website
		- SIGNUP_GOOGLE_AUTH: signed up using Google Authentication (using a Google email account for their login id)
	- creation_time: when they created their account
	- last_session_creation_time: unix timestamp of last login
	- opted_in_to_mailing_list: whether they have opted into receiving marketing emails
	- enabled_for_marketing_drip: whether they are on the regular marketing email drip
	- org_id: the organization (group of users) they belong to
	- invited_by_user_id: which user invited them to join (if applicable).

- **takehome\_user\_engagement**
	- time_stamp
	- user_id
	- visited


## Exploratory Data Analysis

<p align="center" width=100%>
   <img width=49% src="output/figures/count_source.png">
   <img width=49% src="output/figures/adoptionRate_source.png">
   <div align="center"> <i> Figure 1. Nmber count (left) and adoption rate (right) for the ways that users signed up. </i> 
   </div>
</p>

- Users were signed up moslty as a full member of an orgianization. However, the number of non-adopted users is also large for this case.
- The adoption rate is the highest when users are invited to an orgizniation a a guest, and the lowest when users are invited to join another user's personal workspace.
- Users who sign up using Google Authentication are very likely adopted.

<p align="center" width=100%>
   <img width=49% src="output/figures/count_opt.png">
   <img width=49% src="output/figures/adoptionRate_opt.png">
   <div align="center"> <i> Figure 2. Nmber count (left) and adoption rate (right) for the choices whether they have opted into receiving marketing emails </i> 
   </div>
</p>

- The adoption rate is slightly increased when the users have opted into receiving marketing emails.

<p align="center" width=100%>
   <img width=49% src="output/figures/count_marketing.png">
   <img width=49% src="output/figures/adoptionRate_marketing.png">
   <div align="center"> <i> Figure 3. Nmber count (left) and adoption rate (right) for the choices whether they are on the regular marketing email drip </i> 
   </div>
</p>

- The adoption rate is slightly increased when the users are on the regular markketing email drip.


## Feature Engineering
- The data is highly imbalanced. The adoption rate is only 13%.
	- Imbalanced data is resampled via SMOTE method.
- Categorical features are transformed by one-hot code transformation.
- add addtional features of interest:
	- `active_time`: time period that users are active
	- `account_time`: time period since users signed up
- eliminate features that are highly correlated to other feature

<p align="center" width=100%>
   <img width=100% src="output/figures/heatmap.png">
   <div align="center"> <i> Figure 4. Heatmap of features that presents correlation of features to others </i> 
   </div>
</p>

## Modeling
- make use of a Random Forest model
- conduct hyper-parameter tuning

<p align="center" width=100%>
   <img width=70% src="output/figures/roc_auc.png">
   <div align="center"> <i> Figure 5. Receiver Operating Characteristic curve for the adopted users </i> 
   </div>
</p>

- The model performance is extraordinary. 

<p align="center" width=100%>
   <img width=70% src="output/figures/confusion_matrix.png">
   <div align="center"> <i> Figure 6. Confusion Matrix for the predicted adopted users </i> 
   </div>
</p>

## Interpretation of results

<p align="center" width=100%>
   <img width=70% src="output/figures/feature_importance.png">
   <div align="center"> <i> Figure 7. Feature Importance that is computed by permutation feature importance </i> 
   </div>
</p>

- active time is the most critical feature to evaluate the adoption of users.

### SHAP Analysis

<p align="center" width=100%>
   <img width=49% src="output/figures/shap_bar.png">
   <img width=49% src="output/figures/shap_violin.png">
   <div align="center"> <i> Figure 8. Feature Importance that is computed by SHAP analysis: bar chart(left) and violin chart(right) </i> 
   </div>
</p>

It is obvious that

- active time is ciritical to evaluate the adoption of users.
- account time is also of importance.

I recommend that to increase the number of adopted users, the organization needs to encourage existing users to log in and try using programs even though they opened the account for a while ago.

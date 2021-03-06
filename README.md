# Adclick Fraud Detection Visualisation #

DESCRIPTION

data_clean.py - this file take in formated data .csv file, and generate 2 training sets in .pickle (data and label files separately) with the same size (one generated after negative down sampling, one generated without negative down sampling), and 5 testsets in .pickle without negative down sampling (data and label files separately).

run.py - this file take in cleaned training set in .pickle, and generate a trained model in .pickle corresponding to the type of classifier user selected. 

inference.py - this file take in test set in .pickle, and generate the prediction result in .csv file. The format is the same as the raw data .csv.

backend.py - the file contains flask app which serves as a restful api connect frontend visualization and backend model.  

/templates/interactive_visualization.html – show the feature statistics of fraud clicks  

/templates/client_interface.html – the UI that ask the client to provide feature values of a click  

/static/js/intercative_visualization.js – the d3 code for statistics visualization  




INSTALLATION

System requirements: python2.7+, Flask-Cors
python 3.x: pip3 install Flask-Cors
			pip3 install xgboost (only if doing model training/inference)
python 2.x: pip install Flask-Cors
			pip install xgboost (only if doing model training/inference)




EXECUTION

Run following command:

python backend.py

then open http://127.0.0.1:5000/ to browse the website

Please note since all the test data are encoded, we can only accept integer input. Below is a table indicating the correspondence between the real value and integer encoded value for each feature (we only list part of the entire table):  

os:  
Android: 1  
iOS: 2  
Symbian: 3  
Windows Phone: 4  
BlackberryOS: 5  

ip:  
143.215.81.10: 1  
69.63.176.0: 2  
209.191.88.254: 3 

device:  
Samsung S9: 1  
iPhone X: 2  
iPhone XR: 3  
Pixel 3: 4  
Sony Xperia XZ3: 5 

app:  
Tik Tok: 1  
Yelp: 2  
Airbnb: 3  
Uber: 4  
Chrome: 5  
Tumblr: 6  





NOTE

1. Data cleaning

Run following command to save data and labels. One training set xxx.pickle and 5 testing sets xxx1.pickle, xxx2.pickle, xxx3.pickle, xxx4.pickle, xxx5.pickle will be saved to the [output_unbalanced_set_path] and [output_balanced_set_path] specified. The training set size will be the same for both balanced and unbalanced trainset.  

python3 data_clean.py --file=[csv_file_path] --mat_ub=[output_unbalanced_set_path/xxx.pickle] --mat_b=[output_balanced_set_path/xxx.pickle] --y_ub=[output_labels_path_for_unbalanced_set/xxx.pickle] --y_b=[output_labels_path_for_balanced_set/xxx.pickle]

2. Model training

Run following command to fit data and labels using specified method, visualize the training AUC and precision recall matrix and save the model

python3 run.py --mat=[data_file_path/xxx.pickle] --y=[label_file_path/xxx.pickle] --method=[[xgboost][rf][svm][nn]] --model_path=[path to save model/xxx.pickle]

3. Model inference

Run following command to inference the pretrained model on test sets, visualize the testing AUC and precision recall matrix, and dump the prediction result in data_file_path/xxx.csv]

python3 inference.py --test_data=[data_file_path/xxx.pickle] --test_label=[label_file_path/xxx.pickle] --model=[pretrained_model_path/xxx.pickle]


4. Please note that the header of raw .csv dataset file has to be in the format of 

ip, app, device, os, channel, click_scalar, attribute_time, is_attributed
.., .., .., .., .., .., .., {0, 1} 

# Document_Classification
Learning about Document Classification!

Introduction
I will be working on Document Categorization with a given set of labeled documents (6 categories) to categorize new documents. 

Hypothesis
I will be using 8 models for classification (NLTK’s NaiveBayes, SKlearn’s MultinomialNB, BernoulliNB, SVC, LogisticRegression, SGDClassifier, Voting Classifer and Random Forest) and document preprocessing techniques to find the best method. Ideally the resultant accuracy should be above 90% to finalize any models. As the human language has many semantic and syntactic ambiguity, I will also look to see how we can minimize this to improve the model.

Data Description
The data consist of a few formats, namely Scanned and Digital PDFs, 0kb error files, Words document, PowerPoint slides, md & h extension files. There are also documents in non-English language as well. All the data are presorted into folders which is its respective category.

Data Pre-process
Due to my limited programming proficiency and time, I am only able to convert Digital PDFs. This is done by using the PDF Plumber library to extract the text and saving it in a text file .txt of the same name. The rest of the document formats will be ignored for now as it consists of less than 10% of the overall data, which should have minimum impact of the end results. I have also defined a Language detect and translate function (langdetect and google_trans_new Library) and implemented it while extracting data, so all text files will be in English.

NLTK and Gensim library will be utilized in the next step to read the text and preprocessed with a function in the following manner:
1)	Tokenize and lowercase all the words
2)	Remove punctuation, numbers and stopwords (using a default stopwords library, which is not customized to this assignment)
3)	Stem the words to its base form
4)	Create a dictionary of all the unique words in the data set and transform the words into a vector (TFIDF)
5)	Add labels to all the documents and split 70% Training set , 30% Testing set

Data Analysis
The dataset given has an uneven distribution that may affect results, with a lot of data on RandomPDF and NDA. Accuracy results on the test set will be used for initial evaluation of models. The run time on my laptop has come up to about 16mins (mainly taken up by RandomForest) with the highest accuracy at ~77% with the SVC classifier. By using the show_most_informative_features(20) function, it can be seen that the top 20 words do not contain any redundant words such as ‘the’ or ‘same’. Voting Classifier shows a very split decision making among all classifiers less Random Forest as the confidence for the first 5 text is at about 50%. The unsatisfactory performance is most probably attributed to a large dictionary of unique words used to classify (565929 unique words). 
The next step would be to add a simple feature extraction function and limit to the top 3000 words as features. This can be adjusted in the future to see which is the most optimal number of word limit. The accuracy has significantly improved across all models, with the SGD_Classifer at ~97% as highest. The feature extraction has not only improved the overall accuracy, but the runtime has reduced to 1min 47 secs.
The Bias and Variance of the models will be then calculated and be plotted in a chart. Overall, the SGD_Classifier bias is near 0% and the variance is at 2%. (Observation: models have either a high bias and low variance or vice versa)

Solution & Results
With accuracy, bias, and variance, I will conclude that the ideal classifier to use would be the SGD_Classifier. Precision, Recall and F measure will be used to evaluate further as the imbalance dataset might affect accuracy results. I have highlighted the 3 categories (Addendums, Endorsement and Payslips) that have particularly low scores despite the high overall accuracy. On the confusion matrix, the problem seems to be on mainly Addendums and RandomPDFs, where most documents are falsely classified as RandomPDFs or Addendums, or actual addendums falsely classified as others. 
With this, I decided to pluck a random sample Addendum from the internet, convert it using the Language translator and PDF Plumber, and attempt to classify it. The results were successful, as the prediction is correct.

Conclusion
A couple of surprising results come to mind. One such is the ensemble learning such as voting classification and Random Forest do not outperform the other models in document classification. Although there is a very disproportionate data set, e.g 30+ Payslips to 1300+ RandomPDFs, the word feature extraction improved the overall performance.

Although an even distribution is ideal, an imbalanced dataset is a real and constant problem. To overcome this issue and to further improve performance in the future, I can investigate a few methods.

Firstly, we can investigate the misclassified documents (Addendums and RandomPDFs) and see if they are in fact pre labeled correctly. Secondly, we can investigate the words that featured mainly in the documents and see if those words are also mentioned commonly among other documents. If these words are found, then we can include them into the Stopwords library and remove them during preprocessing as they are not unique features for the classifier to identify. Thirdly, oversampling (SMOTE), undersampling (TomekLinks), or a combination of both (SMOTETomek) can be used to evaluate if it will improve the model’s issue of imbalanced dataset. Lastly, since RandomPDFs consist of random documents, we can investigate using unsupervised learning such as LDA to provide an insight to see what we can categorize it into and create more categories to even out the dataset.

There are numerous methods and models available, but through this assignment, I can appreciate the process of cleaning data, training models to evaluate and from the results, identify the issues to implement the right tools to improve.

Additional: Topic Insights on RandomPDF via LDA 
I will be using the Gensim LDA model to see what labels we can give to further classify the RandomPDFs. To find out the common topics in the RandomPDF, I will be firstly preprocessing all the RandomPDFs text. Next to find out the optimal number of topics to generate, I will be using the coherence model to iterate and find the highest score to topic. From the score and plotted chart, 8 is found to be the most optimal number of topics to generate. From there, I have generated a HTML using pyLDAvis to visualize the topics and words generated. This HTML can be used to investigate the common words in each topic, and then further refined by removing common words among all topics and re iterating again. From the visual, we can see that the 8 topics have minimum overlap with each other, as such 8 topics can be defined clearly by investigating the cluster of words and looking in the documents that content these words in detail.

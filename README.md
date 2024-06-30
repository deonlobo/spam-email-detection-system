<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spam Email Detection System</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            background-color: #f9f9f9;
            color: #333;
            margin: 0;
            padding: 20px;
        }
        h1 {
            color: #444;
        }
        h2, h3 {
            color: #555;
        }
        code {
            background-color: #eee;
            padding: 2px 4px;
            font-size: 90%;
            color: #c7254e;
        }
        pre {
            background-color: #f7f7f7;
            padding: 10px;
            border: 1px solid #ccc;
            overflow-x: auto;
        }
        .container {
            max-width: 800px;
            margin: auto;
            background: #fff;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        .table-container {
            overflow-x: auto;
        }
        table {
            border-collapse: collapse;
            width: 100%;
            margin-bottom: 20px;
        }
        table, th, td {
            border: 1px solid #ddd;
            padding: 8px;
        }
        th {
            background-color: #f2f2f2;
            text-align: left;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Spam Email Detection System</h1>
        <p>This project involves building machine learning classifiers to detect spam emails. The classifiers are trained using the <code>SMSSpamCollection</code> dataset, and various preprocessing steps are applied to clean and prepare the data for modeling.</p>
        
        <h2>Project Overview</h2>
        <p>The project follows these main steps:</p>
        <ol>
            <li>Read in and clean the text data</li>
            <li>Split the data into training and testing sets</li>
            <li>Vectorize the text data</li>
            <li>Train and evaluate machine learning models</li>
        </ol>
        
        <h2>Data Cleaning and Preprocessing</h2>
        <p>The text data is cleaned by:</p>
        <ul>
            <li>Removing punctuation</li>
            <li>Converting text to lowercase</li>
            <li>Tokenizing text</li>
            <li>Removing stopwords</li>
            <li>Applying stemming</li>
        </ul>
        
        <h3>Code Example:</h3>
        <pre><code class="language-python">import nltk
import pandas as pd
import re
from sklearn.feature_extraction.text import TfidfVectorizer
import string

stopwords = nltk.corpus.stopwords.words('english')
ps = nltk.PorterStemmer()

data = pd.read_csv("SMSSpamCollection.tsv", sep='\\t')
data.columns = ['label', 'body_text']

def count_punct(text):
count = sum([1 for char in text if char in string.punctuation])
return round(count/(len(text) - text.count(" ")), 3)\*100

data['body_len'] = data['body_text'].apply(lambda x: len(x) - x.count(" "))
data['punct%'] = data['body_text'].apply(lambda x: count_punct(x))

def clean_text(text):
text = "".join([word.lower() for word in text if word not in string.punctuation])
tokens = re.split('\\W+', text)
text = [ps.stem(word) for word in tokens if word not in stopwords]
return text
</code></pre>

        <h2>Data Splitting</h2>
        <p>The data is split into training and testing sets using an 80/20 ratio.</p>

        <h3>Code Example:</h3>
        <pre><code class="language-python">from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(data[['body_text', 'body_len', 'punct%']], data['label'], test_size=0.2)
</code></pre>

        <h2>Vectorization</h2>
        <p>The text data is vectorized using the <code>TfidfVectorizer</code>.</p>

        <h3>Code Example:</h3>
        <pre><code class="language-python">tfidf_vect = TfidfVectorizer(analyzer=clean_text)

tfidf_vect_fit = tfidf_vect.fit(X_train['body_text'])

tfidf_train = tfidf_vect_fit.transform(X_train['body_text'])
tfidf_test = tfidf_vect_fit.transform(X_test['body_text'])

X_train_vect = pd.concat([X_train[['body_len', 'punct%']].reset_index(drop=True),
pd.DataFrame(tfidf_train.toarray())], axis=1)
X_test_vect = pd.concat([X_test[['body_len', 'punct%']].reset_index(drop=True),
pd.DataFrame(tfidf_test.toarray())], axis=1)

X_train_vect.head()
</code></pre>

        <h2>Model Training and Evaluation</h2>
        <p>Two machine learning models are trained and evaluated:</p>
        <ul>
            <li>Random Forest Classifier</li>
            <li>Gradient Boosting Classifier</li>
        </ul>

        <h3>Random Forest Classifier:</h3>
        <pre><code class="language-python">from sklearn.ensemble import RandomForestClassifier

from sklearn.metrics import precision_recall_fscore_support as score
import time

rf = RandomForestClassifier(n_estimators=150, max_depth=None, n_jobs=-1)

start = time.time()
rf_model = rf.fit(X_train_vect, y_train)
end = time.time()
fit_time = (end - start)

start = time.time()
y_pred = rf_model.predict(X_test_vect)
end = time.time()
pred_time = (end - start)

precision, recall, fscore, train_support = score(y_test, y_pred, pos_label='spam', average='binary')
print('Fit time: {} / Predict time: {} ---- Precision: {} / Recall: {} / Accuracy: {}'.format(
round(fit_time, 3), round(pred_time, 3), round(precision, 3), round(recall, 3), round((y_pred==y_test).sum()/len(y_pred), 3)))
</code></pre>

        <h3>Gradient Boosting Classifier:</h3>
        <pre><code class="language-python">from sklearn.ensemble import GradientBoostingClassifier

gb = GradientBoostingClassifier(n_estimators=150, max_depth=11)

start = time.time()
gb_model = gb.fit(X_train_vect, y_train)
end = time.time()
fit_time = (end - start)

start = time.time()
y_pred = gb_model.predict(X_test_vect)
end = time.time()
pred_time = (end - start)

precision, recall, fscore, train_support = score(y_test, y_pred, pos_label='spam', average='binary')
print('Fit time: {} / Predict time: {} ---- Precision: {} / Recall: {} / Accuracy: {}'.format(
round(fit_time, 3), round(pred_time, 3), round(precision, 3), round(recall, 3), round((y_pred==y_test).sum()/len(y_pred), 3)))
</code></pre>

        <h2>Results</h2>
        <p>The Random Forest Classifier achieved the following results:</p>
        <ul>
            <li>Fit time: 1.782 seconds</li>
            <li>Predict time: 0.213 seconds</li>
            <li>Precision: 1.0</li>
            <li>Recall: 0.81</li>
            <li>Accuracy: 0.975</li>
        </ul>

        <p>The Gradient Boosting Classifier achieved the following results:</p>
        <ul>
            <li>Fit time: 186.61 seconds</li>
            <li>Predict time: 0.135 seconds</li>
            <li>Precision: 0.889</li>
            <li>Recall: 0.816</li>
            <li>Accuracy: 0.962</li>
        </ul>
    </div>

</body>
</html>

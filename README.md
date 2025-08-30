📚 Detailed README: Amazon Review Sentiment Analysis Project
This document provides a comprehensive guide to set up, run, and reproduce the results of the Amazon Review Sentiment Analysis project. This project implements an end-to-end machine learning pipeline that includes data preprocessing, sentiment model fine-tuning, review clustering, text summarization, and an interactive Gradio web application.

⚙️ Prerequisites & Environment Setup
To successfully run this project, you'll need the following:

Python: Version 3.7 or higher is required.

Google Colab: All Jupyter notebooks are designed for execution in a Google Colab environment. This provides access to free GPU resources (specifically, the T4 GPU as indicated in the notebook metadata), which are essential for model training.

Google Drive: The project relies heavily on Google Drive for storing large datasets and the trained machine learning model. You must mount your Google Drive within the Colab notebooks to access these resources.

📂 Project File Structure on Google Drive
Before you begin, ensure your Google Drive is organized as follows. This structure is critical for the notebooks to correctly locate input data and save output files. Create these folders if they don't exist.

/content/drive/MyDrive/
├── models/
│   └── distilbert_model/  # This directory will be created and populated by `main_sentiment_analysis.ipynb`
├── amazon_reviews_processed.csv  # Input for all notebooks (except the first if raw data is used directly)
├── amazon_reviews_clustered_and_categorized.csv  # Output from `main_clustering.ipynb`, Input for `main_Sentiment_Gradio_App.ipynb`
└── [Your Raw Data File Here, if applicable] # If you have raw data that needs initial processing before `amazon_reviews_processed.csv` is created.
🛠️ Installation of Dependencies
Each notebook requires specific Python libraries. You can install all necessary packages by running the following commands in a code cell at the beginning of each Jupyter notebook.

Mount Google Drive: This step is crucial to link your Colab environment with your Google Drive, allowing file access.

Python

from google.colab import drive
drive.mount('/content/drive')
Install Python Libraries: Execute these pip commands to install all required libraries. The -qqq flag ensures a quiet installation, reducing verbose output.

Bash

!pip install -qqq transformers datasets gradio scipy pandas scikit-learn
!pip install -qqq sentence-transformers
📝 Notebook Execution Guide & Reproducibility
It is critical to execute the notebooks in the specified order to ensure proper data flow and model availability.

1. main_sentiment_analysis.ipynb
This notebook is the foundation of the project. It handles data preprocessing, fine-tunes a DistilBERT model for sentiment classification, and saves the trained model for future use.

Input Files:

amazon_reviews_processed.csv (expected path: /content/drive/MyDrive/amazon_reviews_processed.csv)

Output Files:

distilbert_model/ (saved to /content/drive/MyDrive/models/distilbert_model/) containing the fine-tuned model weights and tokenizer.

training_args.bin

vocab.txt

Key Steps:

Data Loading and Preprocessing: Loads the processed Amazon review data.

Tokenization: Uses DistilBertTokenizerFast to tokenize the review text.

Model Fine-tuning: Fine-tunes DistilBertForSequenceClassification on your dataset using the Hugging Face Trainer API.

Evaluation: Evaluates the model's performance on a test set.

Model Saving: The final cells explicitly save the entire fine-tuned model and its tokenizer to /content/drive/MyDrive/models/distilbert_model/. Ensure this step completes without errors as subsequent notebooks depend on this output.

How to Run:

Open main_sentiment_analysis.ipynb in Google Colab.

Go to Runtime > Change runtime type and select GPU as the hardware accelerator.

Run the Google Drive mount cell.

Run the dependency installation cells.

Execute all remaining cells sequentially. Monitor the training progress.

2. main_clustering.ipynb
This notebook focuses on segmenting Amazon reviews into distinct groups or clusters based on their semantic content.

Input Files:

amazon_reviews_processed.csv (expected path: /content/drive/MyDrive/amazon_reviews_processed.csv)

Output Files:

amazon_reviews_clustered_and_categorized.csv (saved to /content/drive/MyDrive/amazon_reviews_clustered_and_categorized.csv) containing the original data with new cluster and meta_category columns.

Key Steps:

Data Loading: Loads the processed Amazon review data.

Sentence Embeddings: Generates numerical embeddings for each review using a SentenceTransformer model.

K-Means Clustering: Applies the K-Means algorithm to group reviews based on their embeddings.

Category Assignment: Assigns a meta_category to each cluster for better interpretability.

Save Clustered Data: Saves the DataFrame, now enhanced with cluster and meta_category columns, to amazon_reviews_clustered_and_categorized.csv. This file is crucial for the Gradio application.

How to Run:

Open main_clustering.ipynb in Google Colab.

Run the Google Drive mount cell.

Run the dependency installation cells.

Execute all remaining cells sequentially.

3. main_text_summarization.ipynb
This notebook demonstrates capabilities for generating concise summaries of review texts.

Purpose: To showcase and apply text summarization. This notebook is generally independent of the others in terms of input/output flow but can be used to summarize reviews from the processed dataset.

Key Steps:

Model Initialization: Loads a pre-trained summarization model (e.g., from Hugging Face Transformers).

Text Summarization: Takes input text and generates a shorter, coherent summary.

How to Run:

Open main_text_summarization.ipynb in Google Colab.

Run the Google Drive mount cell.

Run the dependency installation cells.

Execute the cells to see examples of text summarization.

Optional: This notebook includes commented-out code for integrating with the OpenAI API for summarization. If you wish to use this functionality, uncomment the relevant sections and ensure you provide your valid OpenAI API key.

4. main_Sentiment_Gradio_App.ipynb
This notebook hosts the project's interactive web application using Gradio. It allows users to input new reviews, get sentiment predictions, and explore clustered data.

Input Files:

Trained DistilBERT model (expected path: /content/drive/MyDrive/models/distilbert_model/)

amazon_reviews_clustered_and_categorized.csv (expected path: /content/drive/MyDrive/amazon_reviews_clustered_and_categorized.csv)

Key Steps:

Model Loading: Loads the previously fine-tuned DistilBERT sentiment model and its tokenizer.

Data Loading: Loads the clustered and categorized Amazon review data.

Gradio Interface Creation: Builds an interactive web interface.

Sentiment Prediction Function: Defines a function to take a review, predict its sentiment (positive/negative), and display related clustered information.

Application Launch: Launches the Gradio application, generating a public URL for access.

How to Run:

Open main_Sentiment_Gradio_App.ipynb in Google Colab.

Go to Runtime > Change runtime type and select GPU as the hardware accelerator.

Run the Google Drive mount cell.

Run the dependency installation cells.

Crucially, ensure that the distilbert_model directory is present in /content/drive/MyDrive/models/ and amazon_reviews_clustered_and_categorized.csv is in /content/drive/MyDrive/. The notebook's configuration explicitly points to these paths.

Execute all remaining cells. Once the Gradio app launches, you will see a public URL (e.g., https://XXXXX.gradio.live). Click this link to open the interactive web application in your browser.

⚠️ Troubleshooting Common Issues
FileNotFoundError:

Check Google Drive Mount: Ensure the drive.mount('/content/drive') cell ran successfully at the beginning of your notebook.

Verify File Paths: Double-check that your amazon_reviews_processed.csv, amazon_reviews_clustered_and_categorized.csv, and the distilbert_model directory are precisely in the paths expected by the notebooks (/content/drive/MyDrive/).

Correct Notebook Order: Make sure you have run main_sentiment_analysis.ipynb and main_clustering.ipynb before attempting to run main_Sentiment_Gradio_App.ipynb, as the latter depends on outputs from the former two.

Slow Model Training:

Confirm your Colab runtime is set to GPU. Go to Runtime > Change runtime type and select GPU.

Gradio App Not Launching/Accessible:

Ensure all previous cells in main_Sentiment_Gradio_App.ipynb have executed successfully.

Check your internet connection.

Sometimes, Gradio links can expire or have temporary issues. Try re-running the cell that launches the Gradio app to get a new URL.

Dependency Installation Errors:

Make sure you are running the !pip install commands correctly in a code cell.

Ensure you have a stable internet connection for package downloads.

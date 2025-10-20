📚 Detailed README: Amazon Review Sentiment Analysis Project

This document provides a comprehensive guide to set up, run, and reproduce the results of the Amazon Review Sentiment Analysis project, concluding with instructions on how to deploy the final web application to Hugging Face Spaces.

This project implements an end-to-end machine learning pipeline that includes data preprocessing, sentiment model fine-tuning, review clustering, text summarization, and an interactive Gradio web application.

⚙️ Prerequisites & Environment Setup  
To successfully run this project, you'll need the following:

* **Python:** Version 3.7 or higher is required.  
* **Google Colab:** All Jupyter notebooks are designed for execution in a Google Colab environment. This provides access to free GPU resources (specifically, the T4 GPU as indicated in the notebook metadata), which are essential for model training.  
* **Google Drive:** The project relies heavily on Google Drive for storing large datasets and the trained machine learning model.  
* **Git & Hugging Face CLI:** Required for the final deployment to Hugging Face Spaces.

📂 Project File Structure on Google Drive  
Before you begin, ensure your Google Drive is organized as follows. This structure is critical for the notebooks to correctly locate input data and save output files.  
/content/drive/MyDrive/  
├── models/  
│ └── distilbert\_model/ \# Created and populated by main\_sentiment\_analysis.ipynb  
├── amazon\_reviews\_processed.csv  
├── amazon\_reviews\_clustered\_and\_categorized.csv \# Final data file  
└── \[Your Raw Data File Here, if applicable\]  
🛠️ Installation of Dependencies  
Each notebook requires specific Python libraries. You can install all necessary packages by running the following commands in a code cell at the beginning of each Jupyter notebook:  
from google.colab import drive  
drive.mount('/content/drive')

\!pip install \-qqq transformers datasets gradio scipy pandas scikit-learn  
\!pip install \-qqq sentence-transformers

📝 Notebook Execution Guide & Reproducibility  
It is critical to execute the notebooks in the specified order to ensure proper data flow and model availability.

1. **main\_sentiment\_analysis.ipynb**  
   * **Purpose:** Data preprocessing and fine-tuning the DistilBERT model for sentiment classification.  
   * **Output Files:** distilbert\_model/ directory (containing the fine-tuned model weights and tokenizer) saved to Google Drive.  
2. **main\_clustering.ipynb**  
   * **Purpose:** Segmenting Amazon reviews into clusters based on semantic content and assigning meta-categories.  
   * **Output Files:** amazon\_reviews\_clustered\_and\_categorized.csv (containing the original data with new cluster and meta\_category columns) saved to Google Drive. This file is crucial for the Gradio application.  
3. **main\_text\_summarization.ipynb**  
   * **Purpose:** Demonstrates capabilities for generating concise summaries of review texts. (Optional showcase notebook).  
4. **main\_Sentiment\_Gradio\_App.ipynb (The Final App)**  
   * **Purpose:** Builds the interactive Gradio web application using the trained model and categorized data.  
   * **Key Step:** This notebook contains the final Python code (app.py equivalent) and confirms the model and data paths work before deployment.

## **🚀 Deployment to Hugging Face Spaces**

Once you have completed all the notebooks and have the necessary output files, you must gather them into a single local repository for deployment. This process makes your application accessible to the public via Hugging Face Spaces.

### **Step 1: Prepare the Local Directory**

1. **Create Project Folder:** On your local machine, create a clean directory named, for example, Amazon\_Review\_App.  
2. **Gather Assets:** Copy the following required files and folders into this new local directory:  
   * app.py (The final Gradio application script, exported from your last Jupyter notebook).  
   * requirements.txt (A list of all necessary Python libraries for the app to run, e.g., gradio, pandas, transformers, torch).  
   * amazon\_reviews\_clustered\_and\_categorized.csv (Your clustered data file).  
   * distilbert\_model/ (The entire directory containing the trained model and tokenizer files).

### **Step 2: Initialize Git and Deploy**

The model directory is large, so Git LFS (Large File Storage) is required for deployment to Hugging Face Spaces. Execute these commands sequentially in your terminal, starting from the root of your new project directory (Amazon\_Review\_App):

1. **Initialize Git Repository:**  
   git init

2. **Configure Git LFS and Remote:**  
   git lfs install  
   git remote add origin \[https://huggingface.co/spaces/YOUR\_USERNAME/YOUR\_SPACE\_NAME\](https://huggingface.co/spaces/YOUR\_USERNAME/YOUR\_SPACE\_NAME)

3. **Track Large Model Files:** (Essential for uploading the model efficiently)  
   git lfs track "distilbert\_model/\*"

4. **Stage, Commit, and Push:** (Enter your Hugging Face username and Access Token when prompted for credentials).  
   git add .  
   git commit \-m "Initial commit of Gradio app, model, and data"  
   git push \-u origin main

   * *Note: You must use a **Hugging Face Access Token** with 'Write' permission instead of your password when prompted for credentials.*

### **Step 3: Verify and Share**

1. Navigate to your Hugging Face Space URL.  
2. The Space will enter a **"Building"** phase as it installs the libraries from requirements.txt and starts app.py.  
3. Once the build completes successfully, the interactive Amazon Review Sentiment Analyser is live and ready for public use\!

⚠️ Troubleshooting Common Issues

* **KeyError: 'sentiment\_label'**: This means the column used for the Data Visualization chart does not exist in the loaded CSV. You must ensure your amazon\_reviews\_clustered\_and\_categorized.csv contains a sentiment column, or modify the load\_df function in app.py to correctly map or generate it.  
* **Authentication Failed on Push**: Ensure you use a Hugging Face **Access Token** (Role: Write) when prompted for a password during the git push step.
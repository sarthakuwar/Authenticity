# 📝 AI Text Summarizer & Detector

A Flask-based application that summarizes text using Facebook’s **BART-Large-CNN** model and detects whether text is **AI-generated** or **human-written** using an ensemble of machine learning models. Now deployable via **Docker on AWS ECS** with a RESTful API.

---

## 📚 Table of Contents

1. [Overview](#overview)  
2. [Features](#features)  
3. [Installation (Local)](#installation-local)  
4. [Docker & AWS Deployment](#docker--aws-deployment)  
5. [API Usage](#api-usage)  
6. [Model Reference](#model-reference)  
7. [Project Structure](#project-structure)  

---

## 🌟 Overview

This application offers two main functionalities:

- ✂️ **Text Summarization**: Uses Facebook's BART-Large-CNN to generate concise summaries of long texts or uploaded documents.
- 🧠 **AI Text Detection**: Classifies text as AI-generated or human-written using SVC, MultinomialNB, and ComplementNB models.

Users can interact via a **web interface** or directly call the **REST API** (ideal for automation or integration into other platforms).

---

## ✅ Features

- 🔥 **Text Summarization** using [BART-Large-CNN](https://huggingface.co/facebook/bart-large-cnn)
- 📁 File Uploads: Supports `.txt`, `.docx`, `.pdf`
- ✏️ Editable Output: Modify the generated summary before copying/saving
- 📋 Copy to Clipboard: Quickly copy results
- 🚼 Clear Input Option
- 🧠 **AI Text Detection** with ensemble classification
- 🚀 **Dockerized & Deployed**: Now accessible via API hosted on AWS ECS

---

## 🛠️ Installation (Local)

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/ai_text_summarizer.git
   cd ai_text_summarizer
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the Flask app:**
   ```bash
   python app.py
   ```

---

## 🐳 Docker & AWS Deployment

### Build the Docker image:
```bash
docker build -t ai-text-app .
```

### Run the container locally:
```bash
docker run -p 8000:8000 ai-text-app
```

### Deploy to AWS ECS:
- Push your Docker image to [Amazon ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)
- Configure ECS Fargate / EC2 with a task definition
- Expose port `8000` and access your app via public IP or load balancer

---

## 📱 API Usage

Once deployed (e.g., to http://3.27.65.213:8000), use the following endpoint to classify text:

### 🔍 Classify Text

```bash
curl -X POST "http://3.27.65.213:8000/api/classify" -F "text=This is a test input"
```

#### ✅ Example Response:

```json
{
  "input_text": "This is a test input",
  "results": {
    "MultinomialNB": {
      "prediction": "human",
      "probability": "41.7%"
    },
    "ComplementNB": {
      "prediction": "AI-generated",
      "probability": "54.7%"
    },
    "SVC": {
      "prediction": "AI-generated",
      "probability": "100.0%"
    },
    "average_probability": "65.5%"
  }
}
```

> You can easily integrate this endpoint with your frontend or another service to classify user-submitted text dynamically.

---

## 🧐 Model Reference

### ✂️ **Text Summarization**:
- Model: [facebook/bart-large-cnn](https://huggingface.co/facebook/bart-large-cnn)
- Capable of handling up to 1024 tokens
- Pretrained on large news datasets for high-quality abstractive summarization

### 🧠 **AI Text Detection**:
- **SVC (Support Vector Classifier)** – robust text classifier
- **MultinomialNB** – good baseline for document classification
- **ComplementNB** – performs better with imbalanced datasets

Ensemble predictions show each model’s decision with probabilities + overall average.

---

## 📁 Project Structure

```
ai_text_summarizer/
├── app.py                # Main Flask app
├── Dockerfile            # Docker image definition
├── requirements.txt      # Python dependencies
├── static/               # CSS, JS, image files
│   └── screenshot.png    
├── templates/            # HTML templates
│   ├── index.html        
│   ├── summarizer.html   
│   └── classify.html     
├── models/               # Pretrained AI detection models
│   ├── svc_model.pkl     
│   ├── mnb_model.pkl     
│   └── cnb_model.pkl     
└── README.md             # Project documentation
```

---



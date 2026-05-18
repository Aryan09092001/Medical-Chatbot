
# Medical Chatbot — A Conversational AI for Healthcare Q&A
### Powered by LLMs, LangChain, Pinecone, Flask & AWS

An end-to-end medical question-answering chatbot built using **Retrieval-Augmented Generation (RAG)**. Users can ask medical-related questions and receive informed responses backed by a vectorized medical knowledge base.

---

## 🚀 Project Overview

This chatbot combines the reasoning ability of Large Language Models with the precision of vector-based document retrieval. It pulls relevant medical context from a Pinecone vector database and uses an LLM to generate accurate, grounded answers — all served through a clean Flask web interface.

---

## 🛠️ Tech Stack

- **Python** — Core programming language
- **LangChain** — Framework for chaining LLM calls and retrieval
- **Flask** — Lightweight web server for the chat interface
- **Groq / OpenAI** — Language model inference
- **Pinecone** — Vector database for semantic search
- **Docker** — Containerization
- **AWS (ECR + EC2)** — Cloud deployment
- **GitHub Actions** — CI/CD automation

---

## 📋 Prerequisites

Before getting started, make sure you have:

- Python 3.10 installed
- Anaconda or Miniconda
- A Pinecone account (free tier works fine)
- A Groq or OpenAI API key
- Git installed locally

---

## 💻 Running the Project Locally

### Step 1 — Clone the repository

```bash
git clone https://github.com/Aryan09092001/Medical-Chatbot.git
```

### Step 2 — Set up a Conda environment

After moving into the project folder, create and activate a dedicated environment:

```bash
conda create -n medibot python=3.10 -y
```

```bash
conda activate medibot
```

### Step 3 — Install dependencies

```bash
pip install -r requirements.txt
```

### Step 4 — Configure environment variables

In the root folder of the project, create a file named `.env` and add your API credentials:

```ini
PINECONE_API_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
OPENAI_API_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

> 💡 **Tip:** Never commit your `.env` file to GitHub. It's already excluded by `.gitignore`.

### Step 5 — Build the vector index

This script processes your medical documents and stores their embeddings in Pinecone:

```bash
python store_index.py
```

### Step 6 — Launch the Flask app

```bash
python app.py
```

Now open your browser and head to:

```bash
open up localhost:
```

Your chatbot UI should be live and ready for questions. 🎉

---

# ☁️ Deploying to AWS via GitHub Actions (CI/CD)

The following walks through automating deployment using AWS services and GitHub Actions. Every time you push to `main`, the pipeline rebuilds your Docker image and refreshes the live deployment.

## What the pipeline does

The CI/CD setup automates this flow:

1. Builds a Docker image from your source code
2. Pushes the image to AWS Elastic Container Registry (ECR)
3. Pulls that image down onto your EC2 instance
4. Starts the container so your app is live

## Step 1 — Sign in to AWS Console

Head over to [aws.amazon.com](https://aws.amazon.com) and log into your account.

## Step 2 — Create an IAM User for Deployment

Provision a dedicated IAM user that GitHub Actions will use to interact with AWS.

**Required service access:**

1. **EC2** — Virtual machine where your app will run
2. **ECR** — Elastic Container Registry to host your Docker image

**Attach these IAM policies:**

1. `AmazonEC2ContainerRegistryFullAccess`
2. `AmazonEC2FullAccess`

## Step 3 — Create an ECR Repository

This is where your Docker images will be stored. Once created, copy and save the repository URI — you'll need it for GitHub secrets.

```
Save the URI: 315865595366.dkr.ecr.us-east-1.amazonaws.com/medicalbot
```

## Step 4 — Launch an EC2 Instance (Ubuntu)

Spin up a fresh Ubuntu EC2 instance. A `t2.medium` or larger is recommended given the size of the ML dependencies.

> ⚠️ **Don't forget:** Open port 3000 in your EC2 security group's inbound rules so users can reach your chatbot.

## Step 5 — Install Docker on the EC2 Instance

SSH into your EC2 and run these commands.

**Optional — update system packages first:**

```bash
sudo apt-get update -y
sudo apt-get upgrade
```

**Required — install Docker:**

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

## Step 6 — Register EC2 as a Self-Hosted GitHub Runner

This lets GitHub Actions execute deployment steps directly on your EC2 box.

Navigate to:
```
Settings > Actions > Runners > New self-hosted runner
```

Pick your OS (Linux for Ubuntu EC2), then copy and run the commands GitHub provides one at a time on your EC2 instance.

## Step 7 — Add GitHub Secrets

Go to your repo → **Settings → Secrets and variables → Actions** → **New repository secret**.

Add the following secrets:

- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_DEFAULT_REGION`
- `ECR_REPO`
- `PINECONE_API_KEY`
- `OPENAI_API_KEY`

Once these are in place, push any change to `main` and watch the **Actions** tab — your pipeline should kick off automatically.



---

## 📂 Project Structure

```
Medical-Chatbot/
├── .github/
│   └── workflows/
│       └── cicd.yaml          # CI/CD pipeline definition
├── data/                      # Source medical documents
├── research/                  # Notebooks for experimentation
├── src/                       # Helper modules
├── static/                    # CSS / JS for frontend
├── templates/                 # HTML templates
├── app.py                     # Flask application entry point
├── store_index.py             # Pinecone indexing script
├── dockerfile                 # Container build instructions
├── requirements.txt           # Python dependencies
└── README.md
```


---

## 📜 License

This project is open source. See `LICENSE` for details.

---

## 👤 Author

Built by **Aryan** as part of an end-to-end ML deployment learning journey.

If this project helped you, consider giving it a ⭐ on GitHub!

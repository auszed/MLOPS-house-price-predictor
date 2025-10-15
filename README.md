# 🏠 House Price Predictor – An MLOps Learning Project

Welcome to the **House Price Predictor** project! This is a real-world, end-to-end MLOps use case designed to help you master the art of building and operationalizing machine learning pipelines.

You'll start from raw data and move through data preprocessing, feature engineering, experimentation, model tracking with MLflow, and optionally using Jupyter for exploration – all while applying industry-grade tooling.

> 🚀 **Want to master MLOps from scratch?**  
Check out the [MLOps Bootcamp at School of DevOps](https://schoolofdevops.com) to level up your skills.

---

## 📦 Project Structure

```
house-price-predictor/
├── configs/                # YAML-based configuration for models
├── data/                   # Raw and processed datasets
├── deployment/
│   └── mlflow/             # Docker Compose setup for MLflow
├── models/                 # Trained models and preprocessors
├── notebooks/              # Optional Jupyter notebooks for experimentation
├── src/
│   ├── data/               # Data cleaning and preprocessing scripts
│   ├── features/           # Feature engineering pipeline
│   ├── models/             # Model training and evaluation
├── requirements.txt        # Python dependencies
└── README.md               # You’re here!
```

---

## 🚀 Preparing Your Environment

1. **Clone your forked copy:**

   ```bash
   git clone https://github.com/auszed/MLOPS-house-price-predictor.git
   cd house-price-predictor
   ```

2. **Setup Python Virtual Environment using UV:**

   ```bash
   uv venv --python python3.11
   .venv\Scripts\activate
   ```

3. **Install dependencies:**

   ```bash
   uv pip install -r requirements.txt
   ```

---

## 📊 Setup MLflow for Experiment Tracking

To track experiments and model runs:

```bash
docker compose -f deployment/mlflow/mlflow-docker-compose.yml up -d
```

Access the MLflow UI at [http://localhost:5555](http://localhost:5555)

---

## 🔁 Model Workflow

### 🧹 Step 1: Data Processing

Clean and preprocess the raw housing dataset:

```bash
python src/data/run_processing.py   --input data/raw/house_data.csv   --output data/processed/cleaned_house_data.csv
```

---

### 🧠 Step 2: Feature Engineering

Apply transformations and generate features:

```bash
python src/features/engineer.py   --input data/processed/cleaned_house_data.csv   --output data/processed/featured_house_data.csv   --preprocessor models/trained/preprocessor.pkl
```

---

### 📈 Step 3: Modeling & Experimentation

Train your model and log everything to MLflow:

```bash
python src/models/train_model.py   --config configs/model_config.yaml   --data data/processed/featured_house_data.csv   --models-dir models   --mlflow-tracking-uri http://localhost:5555
```

---


## Building FastAPI and Streamlit 

The code for both the apps are available in `src/api` and `streamlit_app` already. To build and launch these apps 

  * Add a  `Dockerfile` in the root of the source code for building FastAPI  
  * Add `streamlit_app/Dockerfile` to package and build the Streamlit app  
  * Add `docker-compose.yaml` in the root path to launch both these apps. be sure to provide `API_URL=http://fastapi:8000` in the streamlit app's environment. 


Once you have launched both the apps, you should be able to access streamlit web ui and make predictions. 

You could also test predictions with FastAPI directly using 

```
curl -X POST "http://localhost:8000/predict" \
-H "Content-Type: application/json" \
-d '{
  "sqft": 1500,
  "bedrooms": 3,
  "bathrooms": 2,
  "location": "suburban",
  "year_built": 2000,
  "condition": fair
}'

```

Be sure to replace `http://localhost:8000/predict` with actual endpoint based on where its running. 


### build fast api app
create the image
```
docker image build -t fastapi-app:latest -f Dockerfile.fastapi .
```

create the container with a name 
```
docker run -idt -p 8888:8000 --name api fastapi-app:latest
```

### build stramlit app
build the image
```
docker run -idt -p 8501:8501 --name streamlit-ui streamlit-app:latest
```

running the container
```
docker run -idtP NAMEIMAGE:VERSION
```

### DOCKERHUB IMAGE CLOUD

login into dockerhub
```
docker login
```

PUSH the image into a hub to upload the image to my personal hub i need to tag the image with the correct format so the image name should had my tagname
```
docker image push DOCKERUSERNAME/NAMEIMAGE:VERSION
```


### troubushooting the container 

logs of the container if something fails
```
docker logs NAMECONTAINER 
```

run the container using bash
```
docker run --rm -it fastapi bash
```


## docker compose 
with this we will build the 2 services ready for production
```
docker compose build
```

then we need to validate it and to get it up and run it
```
docker compose up -d
```

## pipeline
For the pipeline enviroment variables we need to go in github to 

secrets and variables > actions 
add the variables in the repository so the updates get directly when we update


steps to update the pipeline
```
git add .github/workflows/NAMEOFFILE.yaml
git commit -am "COMMENTS"
git push origin main
```

## 🧠 Learn More About MLOps

This project is part of the [**MLOps Bootcamp**](https://schoolofdevops.com) at School of DevOps, where you'll learn how to:

- Build and track ML pipelines
- Containerize and deploy models
- Automate training workflows using GitHub Actions or Argo Workflows
- Apply DevOps principles to Machine Learning systems

🔗 [Get Started with MLOps →](https://schoolofdevops.com)

---

## 🤝 Contributing

We welcome contributions, issues, and suggestions to make this project even better. Feel free to fork, explore, and raise PRs!

---

Happy Learning!  
— Team **School of DevOps**

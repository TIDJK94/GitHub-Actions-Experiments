# GitHub Actions Experiments

This repository demonstrates a complete CI/CD pipeline using GitHub Actions for a machine learning application. It includes training a linear regression model on house price data, building a FastAPI web API for predictions, and deploying it via Docker to a remote VM. Additionally, it features a Streamlit app for interactive predictions and a Jupyter notebook exploring neural network quantization.

## Features

- **Model Training**: Trains a simple linear regression model using scikit-learn on synthetic house data (size, number of rooms, garden presence).
- **API Deployment**: FastAPI application for real-time house price predictions.
- **Streamlit App**: Interactive web interface for model predictions.
- **CI/CD Pipeline**: Automated build, Docker image push to Docker Hub, and SSH-based deployment to a VM.
- **Quantization Notebook**: Educational content on quantizing neural networks (not integrated into the main pipeline).

## Project Structure

- `train_model.py`: Script to train and save the linear regression model (`regression.joblib`).
- `main.py`: FastAPI app for serving predictions.
- `model_app.py`: Streamlit app for interactive predictions.
- `houses.csv`: Sample dataset with house features and prices.
- `Dockerfile`: Multi-stage Docker build for the API.
- `.github/workflows/ci-cd.yml`: GitHub Actions workflow for CI/CD.
- `quantization_II.ipynb`: Jupyter notebook on quantization techniques.
- `requirements.txt`: Python dependencies (referenced in Dockerfile).

## Prerequisites

- Docker Hub account with repository access.
- Remote VM with SSH access and Docker installed.
- GitHub repository secrets configured:
  - `DOCKERHUB_USERNAME`
  - `DOCKERHUB_TOKEN`
  - `SSH_HOST`
  - `SSH_USER`
  - `SSH_PASSWORD`
  - `SSH_PORT` (optional, defaults to 22)

## Setup and Usage

1. **Clone the Repository**:
   ```
   git clone https://github.com/TIDJK94/GitHub-Actions-Experiments.git
   cd GitHub-Actions-Experiments
   ```

2. **Train the Model Locally** (optional, as it's done in Docker):
   ```
   pip install -r requirements.txt
   python train_model.py
   ```

3. **Run the FastAPI App Locally**:
   ```
   uvicorn main:app --host 0.0.0.0 --port 5094
   ```
   Access at `http://localhost:5094`. Use `/predict` endpoint with JSON: `{"size": 150.0, "nbRooms": 3, "hasGarden": true}`.

4. **Run the Streamlit App Locally**:
   ```
   streamlit run model_app.py
   ```

5. **Build and Run Docker Image**:
   ```
   docker build -t your-image .
   docker run -p 5094:5094 your-image
   ```

## CI/CD Pipeline

The workflow (`ci-cd.yml`) triggers on pushes to the `main` branch:

- **Build and Push**: Builds Docker image and pushes to Docker Hub with `latest` and commit SHA tags.
- **Deploy**: SSHs into the VM, pulls the image, stops any existing container, and runs the new one on port 5094.

Monitor runs in the Actions tab. Ensure the `mlops-course` environment is set up in repository settings.

## Quantization Notebook

`quantization_II.ipynb` covers implementing int8 quantization for tensors and neural networks, including matrix multiplication and dequantization. It's a standalone educational resource.

## Contributing

Feel free to open issues or pull requests for improvements, such as adding more features to the model or enhancing the CI/CD pipeline.

## License

This project is open-source. Use at your own discretion.

\# Exercise 1.15: Publish an Application to Docker Hub



\## Project: Fashion-MNIST MLOps



This exercise publishes the FastAPI inference service from an original Fashion-MNIST machine-learning operations project.



The project implements a containerized machine-learning workflow using PyTorch, FastAPI, Streamlit, MLflow, MinIO, and Docker Compose. The published image contains the FastAPI service responsible for exposing model inference endpoints.



\## Project Links



\- \*\*Source repository:\*\* https://github.com/dbhavesh/fashion-mnist-mlops

\- \*\*Dockerfile:\*\* https://github.com/dbhavesh/fashion-mnist-mlops/blob/main/docker/api/Dockerfile

\- \*\*Docker Hub:\*\* https://hub.docker.com/r/dbr007/fashion-mnist-api

\- \*\*Published image:\*\* `dbr007/fashion-mnist-api:v1`



\## Requirements



\- Docker Engine or Docker Desktop

\- Port 8000 available on the host

\- Internet access to pull the published image



No local Python installation is required to run the container.



\## Pull the Docker Image



```bash

docker pull dbr007/fashion-mnist-api:v1

```



\## Run the API



Start the container:



```bash

docker run -d --rm \\

&#x20; --name fashion-mnist-api \\

&#x20; -p 127.0.0.1:8000:8000 \\

&#x20; dbr007/fashion-mnist-api:v1

```



The FastAPI service listens on port 8000.



\## Verify the Application



Check the health endpoint:



```bash

curl http://127.0.0.1:8000/health

```



Expected output:



```json

{"status":"ok"}

```



An HTTP 200 response confirms that the API process is running.



You can also access the interactive API documentation, if enabled, at:



http://127.0.0.1:8000/docs



\### Model readiness



```bash

curl -i http://127.0.0.1:8000/ready

```



When running the API independently without a configured model artifact, an HTTP 503 response is expected:



```json

{

&#x20; "detail": "The selected prediction model is unavailable.",

&#x20; "code": "MODEL\_UNAVAILABLE"

}

```



The API container can start without a model, but the prediction endpoint requires a valid model checkpoint and the appropriate storage configuration. The full MLOps deployment, including MinIO and related components, is documented in the source repository.



\## Build the Image from Source



Clone the source repository:



```bash

git clone https://github.com/dbhavesh/fashion-mnist-mlops.git

cd fashion-mnist-mlops

```



Build the API image from the repository root:



```bash

docker buildx build --load \\

&#x20; -t fashion-mnist-api:v1 \\

&#x20; -f docker/api/Dockerfile .

```



The repository root is required as the Docker build context because the Dockerfile copies application files from the `src/` and `docker/api/` directories.



\## View Logs and Stop the Container



View logs:



```bash

docker logs fashion-mnist-api

```



Stop the container:



```bash

docker stop fashion-mnist-api

```



The container is automatically removed after stopping because it was started with `--rm`.



\## Verification



The image was built and tested locally, with the following results:



| Test | Result |

|---|---|

| API startup | Successful |

| `/health` | HTTP 200 |

| `/ready` without model configuration | HTTP 503, expected |

| Docker Hub authentication | Successful |

| Image push | Successful |



Published image digest:



`sha256:314e3098874eb50f76231ab93cd740f56aac6c80a859a71a289079c20000ba47`



\## Limitations



The published image packages the FastAPI service, not the complete multi-container MLOps platform. Full prediction functionality requires the model artifacts and associated infrastructure described in the source repository.


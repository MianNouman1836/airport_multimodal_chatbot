# Dockerfile for Smart Airport Multimodal Chatbot
FROM python:3.10-slim

WORKDIR /app
COPY . /app

RUN pip install --no-cache-dir -r docker/requirements.txt

EXPOSE 7860
ENV GRADIO_SERVER_NAME="0.0.0.0"

CMD ["python", "notebooks/task6_deployment.py"]

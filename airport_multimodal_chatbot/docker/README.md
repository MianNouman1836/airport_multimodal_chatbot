# Docker Deployment Instructions

1. Build the Docker Image:
   docker build -t airport-chatbot -f docker/Dockerfile .

2. Run the Container locally:
   docker run -p 7860:7860 airport-chatbot

3. Access the prototype at: http://localhost:7860

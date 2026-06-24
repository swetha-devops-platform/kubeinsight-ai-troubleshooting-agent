
services:
  investigation-service:
    build:
      context: ./investigation-service
    container_name: ai-k8s-investigation-service
    network_mode: host
    env_file:
      - ./investigation-service/.env.example
    volumes:
      - ./investigation-service:/app
      - ${HOME}/.kube/config-flat:/root/.kube/config:ro
      - ${HOME}/.minikube:/home/swetha_ravi/.minikube:ro
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

  frontend:
    build:
      context: ./frontend
    container_name: ai-k8s-frontend
    environment:
      NEXT_PUBLIC_API_BASE_URL: http://localhost:8000
      NEXT_PUBLIC_INSFORGE_BASE_URL: https://42iq9eih.us-east.insforge.app
      NEXT_PUBLIC_INSFORGE_ANON_KEY: anon_2d91f0995b159f6226febd0556c48c74e4497573f3527bb097684461e0239a2c
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app
      - /app/node_modules
      - /app/.next
    depends_on:
      - investigation-service
    command: npm run dev

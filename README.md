# Project: validation-server
Deploy simple GO webserver that has health endpoints

# Docker commands
You can manually test the application being deployed by running the docker image.
- docker run --publish 8071:8071 zimmy71/sidecar:20231228.142335

# Get readiness state
- curl --location 'localhost:8071/ready?full=1' --header 'Accept: application/json'

# Post data to validation endpoint
- curl --location 'localhost:8071/validate' --header 'Accept: application/json' --header 'Content-Type: application/json' --data '{"rego": "some rego"}'

# Helm commands
- with in project root directory run: helm install sidecar-helm .
- Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services sidecar-helm)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
- Use the ip address in the curl commands.
  curl --location $NODE_IP:$NODE_PORT/ready?full=1 --header 'Accept: application/json'
  curl --location $NODE_IP:$NODE_PORT/validate --header 'Accept: application/json' --header 'Content-Type: application/json' --data '{"rego": "some rego"}'


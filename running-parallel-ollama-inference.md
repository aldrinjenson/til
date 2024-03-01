---
description: >-
  Easy way to parallely infer with multiple ollama instances all running in the
  same machine
---

# Running parallel Ollama inference

### Problem

Ollama runs based on a single backend concept.

Even if you have good specs in your system, every new request is processed in a queue manner to make maximum resource  usage for a single query.

This is useful for personal laptop usage. But if you have a powerful maching with 10s of GPU RAM, and multiple users inferring at same time, it would be a problem

### Solution

Run multiple ollama instances as isolated container using docker.

### References:

* [Ollama blog ](https://ollama.com/blog/ollama-is-now-available-as-an-official-docker-image)announcing docker containers

### Approach 1

Use docker swarm

```bash
docker swarm init
```

```bash
docker service create --replicas 10 \
--name ollama \
--constraint 'node.labels.gpu'==true \
--mount type=volume,source=ollama,target=/root/.ollama \
--publish published=11434,target=11434 \
ollama/ollama
```

This creates 10 replicas managed by docker swarm.

The base url is same as that of a single instance which you can connect to just like connecting to a regular standalone instance of ollama

**Problem:** This one doesn't have GPU support



### Approach 2

Manage everything ourselves with a tiny bit of help from litellm

### Steps:

1. Write a script to create the specified number(n) of ollama container from port 11434 to 11434+n
2. Ensure that all of them uses the same volume to store the models so as to save up space
3. Create litellm proxy server using a custom config file(generated automatically from create script)
4. Connect to this litellm proxy just as connecting to a single ollama instance.





```bash
#!/bin/bash

# create.sh - to create a given set of docker containers running llama2


CONFIG_FILE="config.yml"
VOLUME_NAME="ollama_volume"
MODEL="llama2"

echo "model_list:" > $CONFIG_FILE

# Define function to create and manage ollama containers
create_ollama_container() {
    local container_name="ollama$1"
    local port=$2

    # Check if the container with the same name exists, if yes, remove it
    if docker ps -a --format '{{.Names}}' | grep -q "^$container_name$"; then
        echo "Removing existing container: $container_name"
        docker rm -f $container_name
    fi

    # Check if the container on the specified port exists, if yes, remove it
    if docker ps -a --format '{{.Ports}}' | grep -q "0.0.0.0:$port->"; then
        echo "Removing existing container on port $port"
        docker rm -f $(docker ps -a --format '{{if (index (split .Ports "/") 0) | (index (split . "->") 0) | (eq "'"0.0.0.0:$port"'")}}{{.Names}}{{end}}')
    fi

        # Create and run the new container
    docker run -d --gpus=all \
        -v $VOLUME_NAME:/root/.ollama \
        -p $port:11434 \
        --name $container_name \
        ollama/ollama

    echo "Created container: $container_name on port $port"


    # Add entry to the config file
    echo "  - model_name: $MODEL" >> $CONFIG_FILE
    echo "    litellm_params:" >> $CONFIG_FILE
    echo "        model: ollama/$MODEL" >> $CONFIG_FILE
    echo "        api_base: http://localhost:$port" >> $CONFIG_FILE

}

# Create n different ollama containers with unique names and ports
for i in {1..6}; do
    port=$((11434 + i))
    create_ollama_container $i $port
done
```



destroy.sh

```bash
#!/bin/bash
# Script to destroy running docker containers

docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
```



test.sh

```bash
#!/bin/bash
# test.sh - to make request to a given  number of llama2 instances

rm file.txt && echo "file.txt cleare"

# Define the command to be executed
curl_command="curl --location 'http://0.0.0.0:8000/chat/completions' --header 'Content-Type: application/json' --data ' {
  \"model\": \"llama2\",
  \"messages\": [
    {
      \"role\": \"user\",
      \"content\": \"tell a long story about unicorns\"
    }
  ]
}' | jq .choices[0].message.content  >> file.txt"

# Run the command n times in parallel
for i in {1..3}; do
    eval "$curl_command" && echo "Command $i succeeded." &  # Run the command in the background
done

# Wait for all background processes to finish
wait

echo "All commands completed."
```



Sample config.yaml

```yaml
model_list:
  - model_name: llama2
    litellm_params:
        model: ollama/llama2
        api_base: http://localhost:11435
  - model_name: llama2
    litellm_params:
        model: ollama/llama2
        api_base: http://localhost:11436
  - model_name: llama2
    litellm_params:
        model: ollama/llama2
        api_base: http://localhost:11437
  - model_name: llama2
    litellm_params:
        model: ollama/llama2
        api_base: http://localhost:11438
```






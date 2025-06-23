Set up OpenWebUI and Ollama using Docker:

## Prerequisites

*   Ensure that your system meets the system requirements for running OpenWebUI and Ollama.
    

## Steps to Setup

### Step 1: Docker Installation

Install Docker and Docker Compose on your system by running:

```bash
sudo apt install docker.io docker-compose -y
```

Enable and start the Docker service using:

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

### Step 2: Run OpenWebUI in a Docker container

Pull the OpenWebUI image from the official GitHub Container Registry and run it:

```bash
docker run -d \
  --name openwebui \
  -p 3000:8080 \
  -v openwebui-data:/app/backend/data \
  -e OLLAMA_BASE_URL=http://172.17.0.1:11434 \
  ghcr.io/open-webui/open-webui:main


```

Note that we are setting the `OLLAMA_BASE_URL` environment variable to the Ollama engine's URL (http://172.17.0.1:11434). This is necessary for OpenWebUI to communicate with Ollama.

### Step 3: Install Ollama

Download and install Ollama using the script from their official website:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Verify that the installation was successful by running:

```bash
ollama ls
```

### Step 4: Verify Docker and Ollama Link

List all currently running Docker containers using:

```bash
docker ps -a
```

Look for the OpenWebUI container named "openwebui".

### Step 5: Configure Ollama Service

Edit the Ollama systemd service file:

```bash
sudo nano /etc/systemd/system/ollama.service
```

Add the following line to set the `OLLAMA_HOST` environment variable within the `[Service]` section:

```ini
Environment="OLLAMA_HOST=172.17.0.1"
```

Save and exit the file using:

*   Press `Ctrl+x`, then `y`, then `Enter`.
    

### Step 6: Reload Systemd Daemon

Reload the systemd daemon to apply the changes made in the Ollama service configuration:

```bash
sudo systemctl daemon-reload
```

### Step 7: Restart Ollama Service

Restart the Ollama service to implement the updated environment variable:

```bash
sudo systemctl restart ollama
```

### Step 8: Start OpenWebUI and Access

Start the OpenWebUI container using:

```bash
docker start openwebui
```

Open OpenWebUI in your preferred browser by navigating to:

```bash
localhost:3000
```

### Step 9: OpenWebUI Configuration

After starting OpenWebUI, complete the following steps in the admin panel to configure Ollama API connections:

1.  Go to the Admin Panel.
    
2.  Navigate to the Settings page.
    
3.  Select the Connections tab.
    
4.  Manage your API connections and create a new connection for Ollama by setting the API key to `http://172.17.0.1:11434`.
    
5.  Verify that the connection has been established successfully.
    

*   If the connection status is Red, recheck the provided steps to ensure everything is set up correctly and try again.
    
*   If the connection status is Green, you have successfully configured OpenWebUI and Ollama for communication!

# 🚀 Complete Guide to Autonomys Node

## 📋 Preparation

Before starting the installation, make sure you have:
- A substrate address (you can use Subwallet)
- Activate the Gemini 3h network in your wallet

## 🛠️ Installation Options

There are two installation options available:

1. **CLI (Command Line Interface)** - Requires VPS (recommended)

2. **GUI (Graphical User Interface)** - Can be used without VPS - For more detailed illustrated guidance, please visit https://docs.autonomys.xyz/docs/farming-&-staking/farming/space-acres/space-acres-install/

### 🖥️ Option 1: CLI Installation (with VPS)

#### Step 1: Install Docker and Docker Compose
```bash
sudo apt update
sudo apt install docker.io docker-compose -y
```

#### Step 2: Create a New Screen
```bash
screen -Rd autonomys
```

#### Step 3: Create a New Folder
```bash
mkdir autonomys && cd autonomys
```

#### Step 4: Create a Docker Compose File
```bash
nano docker-compose.yml
```

#### Step 5: Insert the Docker Compose Code

Copy and paste the following code into the docker-compose.yml file. Make sure to replace the "--reward-address" part with your Subwallet address starting with "st".

```bash
services:
  node:
    image: ghcr.io/autonomys/node:gemini-3h-2024-sep-03
    volumes:
      - node-data:/var/subspace:rw
    ports:
      - "0.0.0.0:30333:30333/tcp"
      - "0.0.0.0:30433:30433/tcp"
    restart: unless-stopped
    command:
      [
        "run",
        "--chain", "gemini-3h",
        "--base-path", "/var/subspace",
        "--listen-on", "/ip4/0.0.0.0/tcp/30333",
        "--dsn-listen-on", "/ip4/0.0.0.0/tcp/30433",
        "--rpc-cors", "all",
        "--rpc-methods", "unsafe",
        "--rpc-listen-on", "0.0.0.0:9944",
        "--farmer",
        "--name", "subspace"
      ]
    healthcheck:
      timeout: 5s
      interval: 30s
      retries: 60
  farmer:
    depends_on:
      node:
        condition: service_healthy
    image: ghcr.io/autonomys/farmer:gemini-3h-2024-sep-03
    volumes:
      - farmer-data:/var/subspace:rw
    ports:
      - "0.0.0.0:30533:30533/tcp"
    restart: unless-stopped
    command:
      [
        "farm",
        "--node-rpc-url", "ws://node:9944",
        "--listen-on", "/ip4/0.0.0.0/tcp/30533",
        "--reward-address", "st8aWPBicPJRnvc9Wg8fxCwaaEbHGhYdq8FY5kZEREQtbPtUG",
        "path=/var/subspace,size=100G"
      ]
volumes:
  node-data:
  farmer-data:
```

Save the file by pressing Ctrl + X, then Y, and Enter.

#### Step 6: Run the Node

```bash
docker-compose up -d
```

#### Step 7: View Logs

```bash
docker compose logs --tail=500 -f
````

#### Step 8: Submit Feedback Form
Visit: https://autonomys.typeform.com/to/tRe4Z0uI

## 🎉 Congratulations! 🎉

You have successfully installed and run the Autonomys Node. If you encounter any difficulties or have questions, feel free to ask in the official Autonomys community.

  ### Discord - https://autonomys.xyz/discord
  ### Telegram - https://t.me/subspace_network
  ### Github - https://github.com/autonomys

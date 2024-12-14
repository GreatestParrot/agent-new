# Development Diary 14.12.2024

#### Default Jobs for CLI

Now CLI have two default jobs ready to run at any time:
1. Docker ```hello-world:linux``` to check if you set up successully
```bash

rn jobs add hello.job <ROBOT_NAME>
```

2. Container with access to sh terminal inside of ```alpine:3``` docker image
```bash

rn jobs add hello.job <ROBOT_NAME>
```

Sources of this examples could be found [here](https://github.com/Smehnov/rn/blob/main/mb/job_examples.py).

### Robot Config are now in YAML

The CLI now uses .yaml for storing configuration files, as it is more readable and widely used in DevOps tools. Example configuration file:
```yaml
robots:
- name: Spot
  robot_id: ''
  robot_peer_id: 12D3KooWFgRQ9bF2Xnu8UmVURssJLgL9gAdMdYbbs4g7fKGxkTf3
  robot_public_key: Vx0g5l4VGapu89LIo0dB7q6q1HLSj+5cj/Mgl0OE7U4=
  tags: []
users:
- public_key: ''
  tags: []
  username: new_user
version: 1
```

Much better than previously used unformatted .json, isn't it?
```json
{"version": 1, "robots": [{"robot_id": "", "robot_peer_id": "12D3KooWFgRQ9bF2Xnu8UmVURssJLgL9gAdMdYbbs4g7fKGxkTf3", "robot_public_key": "Vx0g5l4VGapu89LIo0dB7q6q1HLSj+5cj/Mgl0OE7U4=", "name": "Spot", "tags": []}], "users": [{"public_key": "", "username": "new_user", "tags": []}]}
```

### Agent store job history
Previously, as only job execution reached it's end, we've got no way to get info about it. Now, agent stores history in jobs.json files locally.

### CLI Job Info and Logs
Based on feature from above, CLI now can get much more than only simple status. To get detailed info about execution use
```bash
rn jobs info <ROBOT_NAME> <JOB_ID>
```
The output is gonna be something like this:
```
{
    'type': 'MessageResponse',
    'response_type': 'JobInfo',
    'job_info': {
        'job_type': 'DockerJobInfo',
        'container_id': 'ad46ad157dc1ff1af776595c834eeb5fa855d76ae497c3bbec570699a88f8caa',
        'image': 'hello-world:linux',
        'docker_status': 'removed',
        'last_logs': '\nHello from Docker!\nThis message shows that your installation appears to be working correctly.\n\nTo generate 
this message, Docker took the following steps:\n 1. The Docker client contacted the Docker daemon.\n 2. The Docker daemon pulled the 
"hello-world" image from the Docker Hub.\n    (arm64v8)\n 3. The Docker daemon created a new container from that image which runs the\n
executable that produces the output you are currently reading.\n 4. The Docker daemon streamed that output to the Docker client, which 
sent it\n    to your terminal.\n\nTo try something more ambitious, you can run an Ubuntu container with:\n $ docker run -it ubuntu 
bash\n\nShare images, automate workflows, and more with a free Docker ID:\n https://hub.docker.com/\n\nFor more examples and ideas, 
visit:\n https://docs.docker.com/get-started/\n\n'
    }
}
```

### Encryption
Although libp2p guarantees secure channel between agents, it's impossible to trust over agents, especially the public ones. Now, agent supports encryption in `ChaCha20-Poly1305` format for messages. 
Unfortunatelly, for CLI and Web-app (check next development dairy) to work, the same encryption/decryption function should be implemented on their side, so it's temporary disabled.

### Wireguard setup
Experiments with manual setup of VPN were sucessfully conducted. Next stop is automated deployment

### Data Storage
Data node - agent in storage mode which stores accepted files. It may be used for storing logs, camera frames, etc. The API already has mock version. It active development to share in release.

## Next steps
1. Implement encryption for CLI, so nobody could read your agent's messages without you letting them.
2. Make Wireguard network deployed automatic with only a few commands.
3. Convert mock API for Data Nodes into real one.
4. Create logs module to store each agent action in readable way.

---

# Development Diary 14.12.2024

Welcome to another update on our development journey! We've been hard at work and have some exciting new features and improvements to share with you. Let's dive in!

***

### Default Jobs for CLI

We're thrilled to introduce two default jobs that are ready to run at any time:

1. Docker's hello-world:linux - This job is perfect for checking if everything is set up correctly. Running it will give you that warm, fuzzy feeling of success.
```bash
rn jobs add hello.job <ROBOT_NAME>
```

2. Access to SH Terminal in alpine:3 - This job provides terminal access inside an Alpine Docker container. It's like having a tiny, powerful shell right at your fingertips.

```bash
rn jobs add terminal.job <ROBOT_NAME>
```
For the curious minds out there, you can find the sources for these examples [here](https://github.com/Smehnov/rn/blob/main/mb/job_examples.py).

### Robot Configs Now in YAML

In our quest for better readability and usability, we’ve switched to `.yaml` files for storing configuration data. This change makes your configs not only more readable but also aligns with common DevOps practices. Here's a sneak peek:

```yaml
robots:
- name: Spot
  robot_id: ''
  robot_peer_id: 12D3KooWFgRQ9bF2Xnu8UmVURssJLgL9gAdMdYbbs4g7fKGxkTf3
  robot_public_key: Vx0g5l4VGapu89LIo0dB7q6q1HLSj+5cj/Mgl0OE7U4=
  tags: []
users:
- public_key: ''
  tags: []
  username: new_user
version: 1
```

Compare this to the old, unformatted `.json` files:

```json
{"version": 1, "robots": [{"robot_id": "", "robot_peer_id": "12D3KooWFgRQ9bF2Xnu8UmVURssJLgL9gAdMdYbbs4g7fKGxkTf3", "robot_public_key": "Vx0g5l4VGapu89LIo0dB7q6q1HLSj+5cj/Mgl0OE7U4=", "name": "Spot", "tags": []}], "users": [{"public_key": "", "username": "new_user", "tags": []}]}
```
Much better, right?

### Agent Stores Job History
Previously, as only job execution reached it's end, we've got no way to get info about it. From now on, agent stores history in jobs.json files locally.


CLI Job Info and Logs
Building on our new job history feature, the CLI can now provide detailed information about job executions. This is like giving you a backstage pass to see what’s happening behind the scenes. To get this info, use:

```bash
rn jobs info <ROBOT_NAME> <JOB_ID>
```

The output will look something like this:
```
{
    'type': 'MessageResponse',
    'response_type': 'JobInfo',
    'job_info': {
        'job_type': 'DockerJobInfo',
        'container_id': 'ad46ad157dc1ff1af776595c834eeb5fa855d76ae497c3bbec570699a88f8caa',
        'image': 'hello-world:linux',
        'docker_status': 'removed',
        'last_logs': '\nHello from Docker!\nThis message shows that your installation appears to be working correctly.\n\nTo generate 
this message, Docker took the following steps:\n 1. The Docker client contacted the Docker daemon.\n 2. The Docker daemon pulled the 
"hello-world" image from the Docker Hub.\n    (arm64v8)\n 3. The Docker daemon created a new container from that image which runs the\n
executable that produces the output you are currently reading.\n 4. The Docker daemon streamed that output to the Docker client, which 
sent it\n    to your terminal.\n\nTo try something more ambitious, you can run an Ubuntu container with:\n $ docker run -it ubuntu 
bash\n\nShare images, automate workflows, and more with a free Docker ID:\n https://hub.docker.com/\n\nFor more examples and ideas, 
visit:\n https://docs.docker.com/get-started/\n\n'
    }
}
```

### Encryption
Security is a top priority. Even though libp2p guarantees a secure channel between agents, we can't always trust other agents, especially the public ones. Now, our agent supports message encryption using `ChaCha20-Poly1305`. However, to make this work seamlessly, we need to implement the same encryption/decryption functions on the CLI and web app (check the next diary, we did a crazy thing). So, encryption is temporarily disabled.

### Wireguard Setup
We've successfully conducted experiments with manual VPN setup. The next exciting step is to automate the deployment. Soon, setting up a VPN will be as easy as pie, with just a few commands.

### Data Storage
Introducing the Data Node - an agent in storage mode that stores accepted files. Imagine it as your personal data vault, storing logs, camera frames, and more. The API already has a mock version, and we're actively developing it for the next release.


## Next Steps
1. Implement Encryption for CLI - Ensuring that your agent's messages remain private.
2. Automate Wireguard Deployment - Making VPN setup simple and hassle-free.
3. Develop Data Node API - Turning the mock version into a fully functional reality.
4. Create Logs Module - Storing each agent's actions in a readable format.

---
That's all for today's update! We're making fantastic progress. Stay tuned!













# Azure DevOps agent

### How to use this image
Agent must be started with account connection information, this is supplied via two environment variables:

- `VSTS_ACCOUNT`: name of the Azure DevOps account
- `VSTS_TOKEN`: a personal access token (PAT) that have scope **Agent Pools (read, manage)**

Extra environment variables are suppored for extra customization:
- `VSTS_AGENT`: Agent name (default: `${hostname}`)
- `VSTS_POOL`: Agent pool name (default: `Default`)
- `VSTS_WORK`: Agent work folder (default: `_work`)

#### Example
```
docker run \
    -e VSTS_ACCOUNT=<name> \
    -e VSTS_TOKEN=<PAT> \
    -e VSTS_AGENT='$(hostname)-agent' \
    -e VSTS_POOL=DockerPool \
    -it mrmurb/azure-devops-agent:latest

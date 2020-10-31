# Development Tutorials

Help for developers: Setting up tools, Procedures, Docker, Kubernetes, Helm...

- [Development Tutorials](#development-tutorials)
  - [Docker and Github Container Registry](#docker-and-github-container-registry)
    - [Github Container Registry Authentication](#github-container-registry-authentication)
    - [Building and Publishing an Image to a Github Repository](#building-and-publishing-an-image-to-a-github-repository)
      - [Configuration](#configuration)
      - [Building and Publishing](#building-and-publishing)

## Docker and Github Container Registry

_Assumption: You have docker set up already._

### Github Container Registry Authentication

> To begin we are going to set up a connection to the Github Container Registry.
>
> This will allow you to push newly tagged images to github for use in kubernetes/helm.
>
> **If you lose access to docker at any point, follow this guide again.**

1. Navigate to [Settings / Developer settings / Personal access tokens](https://github.com/settings/tokens)
2. Click on "Generate new token.
3. Name your token _docker_.
4. Select:
   - write:packages
   - read:packages
   - delete:packages
5. Deselect:
   - repo (if it was automatically selected)
6. Click on "Generate token" at the bottom of the page.
7. Remain on this page until you are finished with this tutorial.

```bash
# Replace ACCESS_TOKEN with the access token you acquired above.
export CR_PAT=ACCESS_TOKEN

# Replace USERNAME with your github username.
echo $CR_PAT | docker login ghcr.io -u USERNAME --password-stdin
```

### Building and Publishing an Image to a Github Repository

#### Configuration

> If you are building and publishing an image in a repository that has previously had no image, add the following to your Dockerfile.

```bash
# Replace REPOSITORY with the name of the repository.
LABEL org.opencontainers.image.source https://github.com/flashflashrevolution/REPOSITORY
```

#### Building and Publishing

```bash
# Locate the SHA of the commit you will be basing the image off of.
git log --oneline

# Use the 7 digit SHA as the tag of your new release. Never use latest.
# Replace REPOSITORY with the name of your repository.
docker build -t ghcr.io/flashflashrevolution/REPOSITORY/web:SHA .

# Double check that you've named it and tagged it correctly.
docker images

# Push the new image using the exact string as you had before.
docker push ghcr.io/flashflashrevolution/REPOSITORY/web:sha-SHA
```

# LHM Actions

This repository contains and provides actions to

- Build Maven projects and deploy them to Maven Central
- Build npm projects and deploy them to npmjs
- Build Docker images and push them to GitHub Container Registry
- Create GitHub Releases
- Build vitepress Documentation and deploy it to GitHub Pages  

## Usage
### action-build-docs

Executes the following steps:

1. Enables GitHub Pages
2. Build vitepress docs project
3. Uploads the build output as an artifact.  
   The uploaded artifact can be used to deploy the docs to a web page (see [action-deploy-docs](#action-deploy-docs))

```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-build-docs@v1.0.0
  with:
      # Path to vitepress docs project
      # Default: ./docs
      docs-path: "./docs"
      
      # Node version
      # Default: 22
      node-version: "22"
      
      # You can change build command, e.g. if using vuepress
      # Default: build
      build-cmd: "build"
      
      # Vitepress output path that will be uploaded as artifact
      # Default: .vitepress/dist
      dist-path: ".vitepress/dist"
```

### action-build-image

Executes the following steps:

1. Checkout code
2. Login to Registry
3. Extract metadata (tags, labels) for Docker
4. Build and push image

```yaml
- uses: it-at-m/lhm_actions/action-templates/actions/action-build-docs@v1.0.0
  with:
      # Image registry to push image to
      # Default: ghcr.io
      registry: "ghcr.io"

      # Username to authenticate against image registry
      registry-username: ${{ github.actor }}
    
      # Password to authenticate against image registry
      registry-password: ${{ secrets.GITHUB_TOKEN }}
    
      # Tags to tag image with
      # Default: type=raw,value=latest
      image-tags: type=raw,value=latest

      # Labels to add to image  
      # Default: org.opencontainers.image.description=See ${{ github.server_url }}/${{ github.repository }}
      # Optional
      image-labels: |
          org.opencontainers.image.description=See ${{ github.server_url }}/${{ github.repository }}
    
      # Path to the Dockerfile to build image from
      path: ${{ github.event.inputs.app-path }}
    
      # Name to give the image
      image-name: ${{ github.event.inputs.app-path }}

      # Name of the artifact to download
      artifact-name: ${{ needs.release-maven.outputs.ARTIFACT_NAME }}
```

### action-checkout

### action-codeql

### action-create-github-release

### action-deploy-docs

### action-maven-build

### action-maven-release

### action-npm-build

### action-npm-release

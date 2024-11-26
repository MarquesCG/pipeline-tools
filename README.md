# pipeline-tools

This repository houses several reusable github actions. The included actions are build, deploy, init, lint, and test. Sample usage:

```
 test-backend:
    uses: MarquesCG/pipeline-tools/.github/workflows/test.yaml@v1
    secrets: inherit
    with:
      service-project-dir: %variables as needed%
      test-dir: %variables as needed%
      uses-mongo: %variables as needed%
```

## Build

Build a docker image within the specified context and pushes it to the MarquesCG packages registry. The tag of the image is automatically the `github.sha` of the commit that triggered the workflow.

- `image-name`: The name of the image to create
- `context-path`: The Dockerfile context within the repo
- `service-name`: The name of the service for logging purposes
- `build-args`: A list or arguments to provide to the build step


## Test

Uses poetry runs pytest in a specific directory. Allows a MongoDB to be created and provides the uri as an environment variable.

- `test-dir`: The directory to run the tests from
- `service-project-dir`: The directory that houses the pyproject.toml file for poetry install
- `uses-mongo`: If true, creates a mongodb image *(Default: false)* 
- `mongo-version`: The mongodb version tag to use *(Default: latest)* 
- `mongo-port`: The port for mongodb to use *(Default: 27017)* 
- `mongo-env-var`: The environment variable key that will be set to the mongodb URL *(Default: MONGODB_URI)*

## Init

Copies all the manifests for a service into the tylermarques/homelab-infra repo. This action should be run once, and later updates to the manifests should use the [Deploy action](#deploy). **This action requires that the HOMELAB_TOKEN secret is defined. Contact Tyler to arrange this.**

- `service-name`: The name of the service to initialize
- `manifest-dir`: The directory within the repo that stores the manfiests to be copied over.
- `branch-name`: The name of the branch to pull from *(Default: main)*


## Deploy

Updates the manifests in the tylermarques/homelab-infra repo. This action requires that the manifests already exists within the homelab-infra repo. The [Init action](#init) should therefore be run before this is run. **This action requires that the HOMELAB_TOKEN secret is defined. Contact Tyler to arrange this.**

- `service-name`: The name of the service within homelab-infra
- `deploy-env`: The name of the environemnt to update (dev|production)
- `branch-name`: The name of the branch to pull from
- `deploy-src`: The path to the deploy directory that contains all environments *(Default: deploy)*



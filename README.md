# Continuous Machine Learning

![logo](imgs/dark_logo.png)


## Introduction
Continuous Machine Learning (**CML**) is a tool for implementing continuous integration & delivery (CI/CD) in 
machine learning projects. Use it to automate parts of your development workflow, including
model training and evaluation, comparing ML experiments across your project history, and 
monitoring changing datasets. 

CML uses DVC and Git to provide: 

- **Reproducibility.** When you automate your workflow, it becomes reproducible. 
- **Observability.** CML creates human-readable reports to compare user-defined metrics like model performance across commits. Compare experiments like pull requests. 
- **Release-readiness.**: With CML, every experiment is a release candidate. 
- **Team efficacy.** Review your teammate's models and datasets like code. Checkin and checkout each other's workspaces. 

## Usage
CML extends the CI/CD workflow to ML projects. When a pull or push to your project repository is detected, CML coordinates cloud resources to reproduce a user-defined pipeline and return a CML Report to your repository. 

To begin, you'll need a GitHub or GitLab account. Users may wish to familiarize themselves with 
[Github Actions](https://help.github.com/en/actions) or [GitLab CI/CD](https://about.gitlab.com/stages-devops-lifecycle/continuous-integration/).

`.github/workflows/cml.yml`

```yaml
name: your-workflow-name

on: [push, pull_request]

jobs:
  run:
    runs-on: [ubuntu-latest]
    container: docker://dvcorg/dvc-cml:latest

    steps:
      - uses: actions/checkout@v2

      - name: dvc_cml_run
      env:
        repo_token: ${{ secrets.GITHUB_TOKEN }}
        repro_targets: your_dvc_target.dvc
      run: |
        # Run report:
        dvc_cml_run
```
_LIST OF FUNCTIONS THAT CAN BE RUN INSIDE?_

### Required input and output arguments
_ELLE: I don't believe any arguments are strictly required. Need confirmation._

### Environmental variables

_Here: do we introduce secrets here too???

### Secrets
#### Required
`GITHUB_TOKEN` - _description_

#### Optional
DVC support different kinds of remote
[storage](https://dvc.org/doc/command-reference/remote/add). To setup them
properly you have to setup credentials (if needed) as Github
[secrets](https://help.github.com/es/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets).

<details>
  <summary>
  S3 and S3 compatible storage (Minio, DigitalOcean Spaces, IBM Cloud Object Storage...)
  </summary>

```yaml
# Github
env:
  AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
  AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
  AWS_SESSION_TOKEN: ${{ secrets.AWS_SESSION_TOKEN }}
```

> :point_right: AWS_SESSION_TOKEN is optional.

</details>

<details>
  <summary>
  Azure
  </summary>

```yaml
env:
  AZURE_STORAGE_CONNECTION_STRING:
    ${{ secrets.AZURE_STORAGE_CONNECTION_STRING }}
  AZURE_STORAGE_CONTAINER_NAME: ${{ secrets.AZURE_STORAGE_CONTAINER_NAME }}
```

</details>

<details>
  <summary>
  Aliyn
  </summary>

```yaml
env:
  OSS_BUCKET: ${{ secrets.OSS_BUCKET }}
  OSS_ACCESS_KEY_ID: ${{ secrets.OSS_ACCESS_KEY_ID }}
  OSS_ACCESS_KEY_SECRET: ${{ secrets.OSS_ACCESS_KEY_SECRET }}
  OSS_ENDPOINT: ${{ secrets.OSS_ENDPOINT }}
```

</details>

<details>
  <summary>
  Google Storage
  </summary>

> :warning: Normally, GOOGLE_APPLICATION_CREDENTIALS points to the path of the
> json file that contains the credentials. However in the action this variable
> CONTAINS the content of the file. Copy that json and add it as a secret.

```yaml
env:
  GOOGLE_APPLICATION_CREDENTIALS: ${{ secrets.GOOGLE_APPLICATION_CREDENTIALS }}
```

</details>

<details>
  <summary>
  Google Drive
  </summary>

> :warning: After configuring your
> [Google Drive credentials](https://dvc.org/doc/command-reference/remote/add)
> you will find a json file at
> `your_project_path/.dvc/tmp/gdrive-user-credentials.json`. Copy that json and
> add it as a secret.

```yaml
env:
  GDRIVE_CREDENTIALS_DATA: ${{ secrets.GDRIVE_CREDENTIALS_DATA }}
```


## Example use case

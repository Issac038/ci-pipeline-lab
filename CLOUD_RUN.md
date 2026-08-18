# Cloud Run mapping note

This repository is configured for Docker Hub as the image registry, but the same deployment pattern maps cleanly to Google Cloud Run:

- Push stage: build the image and push it to Artifact Registry
- Auth stage: authenticate with Google Cloud using a service account JSON or workload identity
- Deploy stage: run `gcloud run deploy <service-name> --image <region>-docker.pkg.dev/<project>/<repository>/<image>:<tag>`

Example flow:

```bash
gcloud auth activate-service-account --key-file service-account.json

gcloud builds submit --tag us-central1-docker.pkg.dev/PROJECT_ID/REPO_NAME/app:${GITHUB_SHA}

gcloud run deploy app \
  --image us-central1-docker.pkg.dev/PROJECT_ID/REPO_NAME/app:${GITHUB_SHA} \
  --region us-central1 \
  --platform managed
```

This note is documentation only and does not deploy anything in this repository.

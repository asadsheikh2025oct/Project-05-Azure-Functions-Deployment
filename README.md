# Azure Functions CI/CD: Python V2 & DevOps Pipeline

A professional implementation of a serverless **Azure Function App** (Python 3.11) deployed via a multi-stage **Azure DevOps YAML Pipeline**. This project demonstrates the transition from local development to a fully automated, immutable cloud deployment.

## 🏗️ Architecture

* **Source:** GitHub repository.
* **CI/CD Platform:** Azure DevOps.
* **Infrastructure:** Azure Function App on a **Linux Consumption (Serverless)** plan.
* **Programming Model:** Python V2 (Decorator-based).

## 🛠️ Project Components

* `function_app.py`: The core logic using the `azure.functions` V2 library.
* `host.json`: Metadata for the Azure Function host, including extension bundle configurations.
* `azure-pipelines.yml`: A dual-stage pipeline (Build & Deploy) that manages dependencies, zips the code, and pushes to Azure.

## 🚀 The Pipeline (CI/CD)

The pipeline is split into two distinct stages to ensure a clean "Build Once, Deploy Many" workflow:

1. **Build Stage:**
* Sets up Python 3.11.
* Installs dependencies from `requirements.txt`.
* Archives the project into a ZIP file, ensuring `host.json` stays at the root.
* Publishes the ZIP as a Build Artifact.


2. **Deploy Stage:**
* Downloads the artifact from the build stage.
* Uses `AzureFunctionApp@2` with `zipDeploy` for a reliable deployment.
* Enables `remoteBuild` to allow Azure to handle Linux-specific dependencies.



## 🧠 Lessons Learned & Troubleshooting

Deploying Python V2 functions involves specific nuances that I successfully navigated:

* **Worker Indexing:** Discovered that Python V2 functions require the `AzureWebJobsFeatureFlags` environment variable to be set to `EnableWorkerIndexing` for Azure to discover the decorators.
* **Immutable Deployments:** Learned that using `WEBSITE_RUN_FROM_PACKAGE` puts the Azure Portal into **Read-Only** mode. This enforces DevOps best practices by making the pipeline the single source of truth.
* **Trigger Syncing:** Resolved issues with trigger synchronization by auditing environment variables and removing unnecessary `enableSyncTrigger` flags to maintain a lean configuration.
* **Pathing Precision:** Mastered the `ArchiveFiles` task to prevent "double-folder" nesting, ensuring the host could find the entry-point script.

## 🔧 How to Run

1. **Push** changes to the `main` branch.
2. The **Azure DevOps Pipeline** triggers automatically.
3. Access the live endpoint at: `https://<your-app-name>.azurewebsites.net/api/hello`

## 📊 Key Takeaways

* **Infrastructure as Code:** Moving from manual portal clicks to YAML-defined pipelines ensures consistency.
* **Linux Compatibility:** Using `remoteBuild` is essential for Python apps on Linux to avoid OS-level library mismatches.
* **Monitoring:** Leveraged Azure Log Stream to debug "Cold Start" and "Host Draining" events during deployment.

---

**Author:** Asad Sheikh
**Project:** DevOps Practice Project 05
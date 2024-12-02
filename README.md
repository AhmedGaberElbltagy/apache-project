# Fortify SAST Scan Pipeline Template

This template is designed for performing **Static Application Security Testing (SAST)** using Fortify ScanCentral. It handles the installation, configuration, and execution of Fortify tools to scan your codebase and upload results to the Fortify SSC.

---

## Parameters

The template uses the following parameters for dynamic customization:

| Parameter         | Default Value         | Description                                                                 |
|-------------------|-----------------------|-----------------------------------------------------------------------------|
| `ssc_url`         | `$(SSC_URL)`          | The URL of the Fortify Software Security Center (SSC).                     |
| `ssc_token`       | `$(SSC_TOKEN)`        | The authentication token for SSC to upload scan results.                   |
| `controller_url`  | `$(CONTROLLER_URL)`   | The URL of the Fortify ScanCentral Controller.                              |
| `client_auth_token` | `$(CLIENT_AUTH_TOKEN)` | The authentication token for the ScanCentral client.                      |
| `worker_auth_token` | `$(WORKER_AUTH_TOKEN)` | The authentication token for ScanCentral worker.                          |
| `path`            | `java_services`       | The relative path to the directory containing the source code to be scanned.|
| `buildTool`       | `none`                | The build tool used in the project (e.g., Maven, Gradle). Default is `none`.|

---

## Pipeline Stages

### 1. **Deployment Stage**
This stage contains a single job that executes the steps required to perform SAST using Fortify.

#### **Job: Fortify_SAST_Scan**

| Task                          | Description                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| **Install SCA**               | Installs the Fortify Security Code Analyzer if not already installed.                        |
| **Configure Auth Tokens**     | Configures authentication tokens for the ScanCentral worker and client.                      |
| **Initiate Sensor**           | Initializes the ScanCentral sensor to connect with the Fortify controller.                   |
| **Start Scanning**            | Scans the specified source code and uploads the results to the Fortify SSC.                  |
| **Check Scan Status**         | Monitors the scan job status until it is completed.                                          |
| **Clean Up**                  | Removes temporary files such as logs created during the scan.                                |

---

## Installation and Execution Details

### Step 1: Install Fortify Security Code Analyzer
- Checks if Fortify SCA is already installed in `/opt/fortify`. If not, installs it silently using the installer located at `/tmp/Fortify/Fortify_SCA_24.2.0_linux_x64.run`.

### Step 2: Configure Authentication Tokens
- Updates the configuration files (`worker.properties` and `client.properties`) with the provided tokens for secure communication with ScanCentral.

### Step 3: Initiate Sensor
- Starts the ScanCentral sensor with the provided `controller_url`.

### Step 4: Start Scanning
- Navigates to the specified `path` and starts scanning the project using the provided `buildTool`.
- The scan results are automatically uploaded to the Fortify SSC.

### Step 5: Monitor Scan Status
- Extracts the scan job token and checks its status in a loop until it completes.

### Step 6: Clean Up
- Removes temporary files such as the `output.log` created during the scan process.

---

## Usage

1. Update the parameters in your pipeline with the correct values for your environment.
2. Ensure the necessary Fortify tools and license files are available in the specified locations.
3. Trigger the pipeline to execute the scan process.

---

## Notes

- Make sure the agent pool (`Fortify`) has sufficient permissions and access to the required files and tools.
- Ensure the `path` parameter correctly points to the directory containing your source code.
- Monitor the logs for potential issues during the scanning process.
- For detailed Fortify documentation, visit the [Fortify website](https://www.microfocus.com/en-us/cyberres/application-security/fortify).

---

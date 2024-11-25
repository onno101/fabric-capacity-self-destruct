# Fabric Capacity Self-Destruct

## Overview
This repository provides guidance on how to automatically pause your Fabric Capacity to reduce costs, particularly useful for demo tenants. The process involves using a Fabric Notebook that "self-destructs" by pausing the capacity while running. You will need to manually resume the capacity when required. This solution is ideal for those who may forget to turn off their Fabric capacity, leading to unnecessary expenses.

Note that this method does not support resuming the capacity. A Fabric Notebook cannot start the capacity as it does not run when its capacity is paused. For resuming the capacity, consider using API requests as described [here](https://learn.microsoft.com/en-us/rest/api/microsoftfabric/fabric-capacities/suspend?view=rest-microsoftfabric-2023-11-01&tabs=HTTP) and orchestrating them through external tools like scheduling GitHub Actions pipelines.

## Getting Started

### Prerequisites
- Azure subscription
- Fabric license
- Azure Key Vault (with permissions to create and retrieve secrets)

### Configuration
1. Start by [creating a service principal (app registration) in Azure Entra ID](https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal). The following permissions on the Fabric Capacity resource are required to allow pausing, and are included in the `Fabric Administrator` role:
    - Microsoft.Fabric/capacities/read
    - Microsoft.Fabric/capacities/write
    - microsoft.fabric/capacities/suspend/action
    - microsoft.fabric/capacities/resume/action
1. Create a [client secret for the service principal](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app?tabs=certificate#add-credentials). Then, create three Azure Key Vault Secrets:
    - client secret (just generated)
    - client id
    - tenant id
1. Create a new Fabric notebook in a workspace using the same capacity you want to pause. Import the notebook from the `src/notebooks` folder. Ensure all required variables are filled in. All `# comments` in the code indicate where you need to change your variables.
1. Run the notebook to test, ensuring no essential processes are running in Fabric as it will pause the entire capacity.
1. Schedule the notebook using the (Scheduling functionality in Fabric)[https://learn.microsoft.com/en-us/fabric/data-engineering/how-to-use-notebook#security-context-of-running-notebook]. It could be wise to schedule it after each work day.

## Project Structure
- `src/`: Contains the code for the Self-Destruct notebook
- `README.md`: Project documentation.

## Contributing
Contributions are welcome! Please read the contributing guidelines for more details.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

## Contact
For any questions or suggestions, please use the Issues section.

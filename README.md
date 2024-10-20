# NPM_Package_Automation

**Objective**:
The task was to simulate the development and automation process for an internal React UI library, ensuring other tools and applications can seamlessly integrate with the latest version of the library.

### Approach:

1. **Library Simulation**:
    - Used the [**`react-tiny-popover`**](https://github.com/alexkatz/react-tiny-popover) React **TypeScript** library to simulate the internal UI library.
2. **Enhancements**:
    - Added **linting** in the `package.json` to enforce coding standards.
    - Developed a **test case** using **Jest** to simulate unit testing for the `PopoverPortal` component.
    - Added .npmrc to be able publish the library to the private Github packages.
3. **CI/CD Pipeline**:
    - Created a **GitHub Actions workflow** for continuous integration and deployment (CI/CD), which will be triggered on pushing or merging a new code to the main branch, hence a new version of the library will be published.
    - The pipeline (react-tiny-popover-cicd/.github/workflows/cicd.yaml) includes:
        - Checking out the repository.
        - Setting up Node.js version `18.6.0`.
        - Installing dependencies.
        - Running linting and testing.
        - Building the library.
        - Publishing the package to **private** **GitHub Packages (GitHub secret needs to be added either to the repository or to the organization)**.
        - To add a GitHub secret to the repository got to Settings → Secrets and variables → Actions → New repository secret
            <img width="643" alt="Screenshot 2024-10-20 at 4 37 24 PM" src="https://github.com/user-attachments/assets/b3a31689-0026-4591-b819-b1fbd3177b3c">

            
        - After the pipeline execution a new version of the library will be published to the GitHub private packages
            <img width="643" alt="Screenshot 2024-10-20 at 4 38 06 PM" src="https://github.com/user-attachments/assets/c05b41ab-1edf-4d6f-8796-7298e3b61ad3">

            
    - Alternatives for package publishing include **JFrog Artifactory**, **npm**, or **Azure Artifacts**.
4. **Automating Dependency Updates with Dependabot**:
    - To automate dependency updates and avoid missing important updates, I will be using Dependabot.
    - Configured **Dependabot** to automate dependency updates across projects that use this library (for demo I will be using**`NPM_Dependencies_Automation`**).
    - Configured Dependabot scans to check for updates on a **weekly basis,** and it will automatically create PRs with new versions.
    
    **Steps to simulate automatic dependency handling**:
    
    - Created a new repository named **`NPM_Dependencies_Automation`**.
    - Configured the `.github/dependabot.yml` file for automatic dependency updates as follows:
        <img width="669" alt="Screenshot 2024-10-20 at 4 38 54 PM" src="https://github.com/user-attachments/assets/034b9e4b-3759-41f1-a32f-eeeaea16d122">
        
        - configured the scan to run once each week; by default, this is on Monday. This can can also be configured to run daily or monthly.
        - Added the URL of my private registery.
        - Added the GitHub token to Dependabot secrets as follows:
            - from settings → Secrets and variables → Dependabot → New repository secret
                <img width="617" alt="Screenshot 2024-10-20 at 4 39 16 PM" src="https://github.com/user-attachments/assets/52d256ba-9d12-4e66-9d8c-671dbe0643b9">
                
    - Added a CI pipeline (NPM_Dependencies_Automation/.github/workflows/ci.yaml) to install dependencies, which includes the **`react-tiny-popover`** package published earlier.
        <img width="670" alt="Screenshot 2024-10-20 at 4 39 38 PM" src="https://github.com/user-attachments/assets/5fdfbd78-ad3a-4f14-ab77-02e7e1bf8954">
        
        package.json
        <img width="669" alt="Screenshot 2024-10-20 at 4 39 59 PM" src="https://github.com/user-attachments/assets/04b3968b-62f9-477f-b30b-b324e16132f0">
        
        - Added .npmrc to be able to install the @nourhankhalifa/react-tiny-popover package from the private GitHub packages.
        <img width="670" alt="Screenshot 2024-10-20 at 4 40 20 PM" src="https://github.com/user-attachments/assets/f26faadc-9ffc-4685-84e7-be6c69b13868">

        

### GitHub Repositories

https://github.com/nourhankhalifa/react-tiny-popover-cicd

https://github.com/nourhankhalifa/NPM_Dependencies_Automation

### Demo Video

In the attached video, I demonstrate a workflow for the React library development process. A developer will create a new branch for a feature, and once complete, they will open a pull request to the main branch. This triggers a CI pipeline that runs scans. Upon merging the feature (simulated by directly pushing to the main branch for demo purposes), the publish pipeline is triggered to release the new version to GitHub Packages.

In the **NPM_Dependencies_Automation** repository, which simulates a project that uses the react library as a dependency, I manually triggered Dependabot, simulating its automatic weekly scan. Dependabot will automatically create a pull request with the new version if an update is detected, awaiting team review and approval.

### Notes and Alternative Solutions

- Other CI/CD tools such as **Jenkins**, **GitLab CI**, or **Azure DevOps** can be used, depending on the organization's stack.
- **Renovate** can be used as an alternative to **Dependabot** for dependency updates.
- Additional tools for **security scanning** can be integrated into the pipeline before publishing to the registry.
- Instead of GitHub Packages, a different registry like **JFrog** can be used, based on the organization's stack.
- For **RedHat** restrictions, a **self-hosted runner** on a RedHat image or running pipelines on RedHat-based agents (e.g., in Jenkins or other tools) can be configured.

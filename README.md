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
            
            ![Screenshot 2024-09-27 at 8.00.11 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bfaea251-05d7-46b2-a023-766af5b6ee7c/f6b57964-2826-480e-bfe9-c644cb1cd66f/Screenshot_2024-09-27_at_8.00.11_PM.png)
            
        - After the pipeline execution a new version of the library will be published to the GitHub private packages
            
            ![Screenshot 2024-09-27 at 8.05.59 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bfaea251-05d7-46b2-a023-766af5b6ee7c/199e5202-c0eb-47ab-9a51-1f2412e5bfe7/Screenshot_2024-09-27_at_8.05.59_PM.png)
            
    - Alternatives for package publishing include **JFrog Artifactory**, **npm**, or **Azure Artifacts**.
4. **Automating Dependency Updates with Dependabot**:
    - To automate dependency updates and avoid missing important updates, I will be using Dependabot.
    - Configured **Dependabot** to automate dependency updates across projects that use this library (for demo I will be using**`NPM_Dependencies_Automation`**).
    - Configured Dependabot scans to check for updates on a **weekly basis,** and it will automatically create PRs with new versions.
    
    **Steps to simulate automatic dependency handling**:
    
    - Created a new repository named **`NPM_Dependencies_Automation`**.
    - Configured the `.github/dependabot.yml` file for automatic dependency updates as follows:
        
        ![Screenshot 2024-09-27 at 8.14.21 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bfaea251-05d7-46b2-a023-766af5b6ee7c/e8e32bc3-a873-412e-8f29-7417c7d22475/Screenshot_2024-09-27_at_8.14.21_PM.png)
        
        - configured the scan to run once each week; by default, this is on Monday. This can can also be configured to run daily or monthly.
        - Added the URL of my private registery.
        - Added the GitHub token to Dependabot secrets as follows:
            - from settings → Secrets and variables → Dependabot → New repository secret
                
                ![Screenshot 2024-09-27 at 8.19.40 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bfaea251-05d7-46b2-a023-766af5b6ee7c/2a76dc91-e0a3-4f79-9d8f-cb3071b4055f/Screenshot_2024-09-27_at_8.19.40_PM.png)
                
    - Added a CI pipeline (NPM_Dependencies_Automation/.github/workflows/ci.yaml) to install dependencies, which includes the **`react-tiny-popover`** package published earlier.
        
        ![Screenshot 2024-09-27 at 8.25.43 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bfaea251-05d7-46b2-a023-766af5b6ee7c/6ae4c2f7-facb-4ebe-929a-d6aec68e306f/Screenshot_2024-09-27_at_8.25.43_PM.png)
        
        package.json
        
        ![Screenshot 2024-09-27 at 8.27.42 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bfaea251-05d7-46b2-a023-766af5b6ee7c/2cc49c4e-5df3-4e01-a1d7-5143c6c39ae4/Screenshot_2024-09-27_at_8.27.42_PM.png)
        
        - Added .npmrc to be able to install the @nourhankhalifa/react-tiny-popover package from the private GitHub packages.
        
        ![Screenshot 2024-09-27 at 8.30.55 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bfaea251-05d7-46b2-a023-766af5b6ee7c/147bedcc-a998-4a7c-a6bc-da75ec8be40b/Screenshot_2024-09-27_at_8.30.55_PM.png)
        

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

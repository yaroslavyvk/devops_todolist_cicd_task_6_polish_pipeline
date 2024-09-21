
---

# Django ToDo List

This is a to-do list web application with the basic features of most web apps, such as accounts/login, API, and interactive UI.

To complete this task, you will need:

- CSS | [Skeleton](http://getskeleton.com/)
- JS  | [jQuery](https://jquery.com/)

## Table of Contents

- [Explore](#explore)
- [Task](#task)
- [Solution](#solution)
  - [Docker Integration](#docker-integration)
  - [Matrix Testing](#matrix-testing)
  - [Input Variables](#input-variables)
  - [Branch Protection](#branch-protection)
  - [Manual Approval](#manual-approval)
  - [Workflow Optimization](#workflow-optimization)
  - [Pull Request](#pull-request)
- [Conclusion](#conclusion)

## Explore

Follow these steps to get the application up and running on your local machine (requires Python 3.8 or higher due to compatibility with Django 4):

```bash
pip install -r requirements.txt
```

Create a database schema:

```bash
python manage.py migrate
```

And then start the server (default is <http://localhost:8000>):

```bash
python manage.py runserver
```

You can now browse the [API](http://localhost:8000/api/) or start on the [landing page](http://localhost:8000/).

## Task

Extend the project's GitHub Actions workflow by integrating Docker to build and push images to DockerHub.

This CI/CD enhancement involves several key tasks:

1. **Update your forked repository with your DockerHub username and password.**
   - Add corresponding secrets to the repository.
2. **Update `DockerImageName` with the DockerHub image repository name.**
3. **Use Matrix to run unit tests on different Python versions (3.8, 3.9).**
4. **Use Matrix to run unit tests on different OS types: Ubuntu and Windows.**
5. **Add input variables:**
   - Input variable to choose which artifact from the matrix to deploy (windows-3.8, ubuntu-3.9, etc).
6. **Add branch protection to the main branch in your fork.**
7. **Add mandatory pull requests and Python CI job status checks for PRs.**
8. **Add Manual Approval for the STG environment.**
9. **Allow to run only one workflow per pull request.**
10. **New runs should cancel the previous runs.**
11. **Create a Pull Request with the changes.**
12. **Pull Requests description should also contain a reference to a workflow run with successful workflow execution.**

## Solution

I have successfully extended the project's GitHub Actions workflow by implementing the tasks outlined above. Below is a detailed description of how I executed each task and the reasoning behind each decision.

### Docker Integration

- **Updated the workflow to build Docker images and push them to DockerHub:**
  - Modified the existing GitHub Actions workflow to include steps for building a Docker image of the Django application.
  - Used the `docker/build-push-action` to streamline the build and push process.
- **Added DockerHub username and password as secrets in the repository:**
  - Stored `DOCKERHUB_USERNAME` and `DOCKERHUB_PASSWORD` securely in GitHub Secrets to avoid exposing sensitive information.
- **Updated `DockerImageName` with the DockerHub image repository name:**
  - Ensured the Docker image is correctly tagged with the repository name for proper versioning and distribution.

*Reasoning:* Integrating Docker allows for consistent deployment environments and eases the distribution of the application across different platforms.

### Matrix Testing

- **Configured the workflow to use a matrix for running unit tests on different Python versions (3.8, 3.9):**
  - Defined a matrix strategy in the workflow YAML file to run tests concurrently on specified Python versions.
- **Added matrix configurations to test on different operating systems: Ubuntu and Windows:**
  - Expanded the matrix to include both `ubuntu-latest` and `windows-latest` runners to ensure cross-platform compatibility.

*Reasoning:* Using a matrix strategy enhances test coverage across different environments, increasing the reliability of the application.

### Input Variables

- **Added input variables to choose which artifact from the matrix to deploy:**
  - Implemented workflow inputs that allow specifying the target environment (e.g., `os: ubuntu`, `python-version: 3.9`).
  - Adjusted the deployment steps to use these inputs, making the workflow more flexible and customizable.

*Reasoning:* Input variables provide greater control over the deployment process, allowing selective deployment of artifacts based on specific criteria.

### Branch Protection

- **Added branch protection rules to the main branch:**
  - Configured GitHub repository settings to prevent direct pushes to the `main` branch.
- **Made pull requests and passing CI checks mandatory for merging:**
  - Set required status checks on the CI workflow to enforce that all tests pass before code can be merged.

*Reasoning:* Branch protection ensures code integrity by enforcing code reviews and successful CI checks, maintaining a high-quality codebase.

### Manual Approval

- **Implemented manual approval steps for deploying to the staging (STG) environment:**
  - Added `environment` protection rules in the workflow that require manual approval before deploying to STG.
- **Used GitHub Environments feature:**
  - Leveraged GitHub's Environments to manage deployment targets and approval processes.

*Reasoning:* Manual approval adds a safety layer before deploying changes to critical environments, allowing for last-minute reviews.

### Workflow Optimization

- **Configured the workflow to allow only one run per pull request:**
  - Used the `concurrency` feature with a unique `group` identifier based on the pull request.
- **New runs automatically cancel previous ones to optimize resources:**
  - Set `cancel-in-progress: true` within the concurrency configuration.

*Reasoning:* Optimizing workflow runs conserves CI/CD resources and reduces unnecessary load, ensuring faster feedback cycles.

### Pull Request

- **Created a pull request with all the changes:**
  - Opened a PR summarizing all enhancements made to the CI/CD pipeline.
- **Included a reference to a successful workflow execution in the PR description:**
  - Attached a link to the successful GitHub Actions run for reviewers to verify the workflow execution.

*Reasoning:* Providing references to successful runs increases transparency and helps reviewers validate the implemented changes.

## Conclusion

By completing these tasks, I have enhanced the CI/CD pipeline to be more robust, flexible, and secure. The integration of Docker and matrix testing ensures that the application is reliable across different environments. Branch protection and workflow optimizations contribute to maintaining code quality and efficient development practices.

---

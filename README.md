# Setting Up Continuous Integration with Jenkins and GitHub
## Objective:
​This lab's focus is on partially building a CICD pipeline by integrating Github and Jenkins. I have included steps to and troubleshooting common issues faced during setup, and ensuring a smooth continuous integration workflow. You'll learn to set up Jenkins, connect it to a GitHub repository, and resolve authentication, branch configuration challenges.
### How to submit:

Please show me the successful build on your Jenkins either during this lab or next week’s lab. I prefer an in-person demo over a recorded one, unless you have a valid reason for not being able to attend next week’s lab.

## Lab Overview
- Part 1: Setting up the environment
- Part 2: Pushing a simple project to GitHub
- Part 3: Configuring Jenkins to build the project
- Part 4: Troubleshooting common issues
- Part 5: Additional tasks for Bonus marks only. 



## Part 1: Setting Up the Environment
- 1. Install Prerequisites:
    - Jenkins: Install Jenkins from https://www.jenkins.io/download/.
    - Java: Ensure you have Java JDK installed. Verify using:
    ```
    Java -version
    ```
- 2. Start Jenkins:
    - Launch Jenkins on your local machine using .war file:
    ```
    java -jar jenkins.war
    ```
    - As a service(Linux):
    ```
    sudo systemctl start jenkins
    ```
- 3. Access Jenkins Web Interface
    - Open your web browser and go to http://localhost:8080.
- 4. Unlock Jenkins

    - During the initial startup, Jenkins will ask for an admin password.

    -  The password is located in:

        - Windows: ```C:\Program Files\Jenkins\secrets\initialAdminPassword```
        - macOS/Linux: ```/Users/yourusername/.jenkins/secrets/initialAdminPassword```
    - Enter the password in the Jenkins interface.

- 5. Install Suggested Plugins
    - Choose Install suggested plugins when prompted.

- 6.  Create an Admin User
        - Fill in the required fields to create your admin user.
- 7. Install Necessary Plugins:
        - Git Plugin (if not already installed):
            - Go to Manage Jenkins > Manage Plugins > Available.
            - Search for Git Plugin and install it.
            - Restart Jenkins if prompted.
        - Python Plugin:
            - Search for ShiningPanda Plugin and install it.
            - This plugin helps manage Python installations and virtual environments.
- 8. Configure Python in Jenkins
        - Go to Manage Jenkins > Global Tool Configuration.
        - Scroll to Python.
        - Click Add Python.
            - Name: Python3 (or any name you prefer)
            - Python home: Provide the path to your Python installation (e.g., C:\Python39).
            - Alternatively, select Install automatically to let Jenkins manage the installation.
- 9. Create a New Jenkins Job
        - From the Jenkins dashboard, click on New Item.
    - Configure the Job
        - Enter an item name: SamplePythonApp_Build
        - Select: Freestyle project
        - Click OK.
- 10. Configure Source Code Management
    - In the Configure page of your job, scroll down to Source Code Management.
    - Set Up Git Repository
        - Select: Git
        - Repository URL: https://github.com/<your-username>/CICD-Jenkins.git
        - Add Credentials:
            - Click Add > Jenkins.
            - Select Username with password and enter your GitHub username and Personal Access Token (PAT).
        - Branch Specifier: Leave as */main unless using a different branch.
- 11.  Configure Build Triggers
    - Check Poll SCM.
    - Set the schedule to how often you want Jenkins to check for changes.
        - example Every 5 minutes: H/5 * * * *
- 12. Add a Build Step:
    - Under Build > Add build step, select Execute shell (or Windows Batch Command).
    - Add the command to run the Python script
    ```
    python app.py
    ```
    - if you are on windows: you could do this 
    Execute Windows batch command
13. Save and Build:
    - Save the job and click Build Now to trigger the first build.

## Part 3: Troubleshooting Common Issues
### Problem 1: GitHub Authentication Error
- Error:
    ```
    fatal: Support for password authentication was removed on August 13, 2021.

    ```
- Solution:
    - Generate a Personal Access Token (PAT) with the following scopes:
        - repo (for private repositories).
        - repo:status, repo:public_repo (for public repositories).
    - Add the PAT to Jenkins credentials and use it for the repository connection.

### Problem 2: Branch Not Found
- Error:
    ```
    Couldn't find any revision to build. Verify the repository and branch configuration for this job.

    ```
### Problem 3: Repository Empty
- Error:
```
fatal: Could not read from remote repository.

```
- Solution:

    - Ensure the repository has at least one commit.
    - Push code to GitHub or initialize the repository with a README.md file.

### Problem 5: Build Fails with Empty Workspace
- Error:
    ```
    ERROR: Could not check out code from repository.
    ```
- Solution:

    - Ensure the repository URL and credentials are correct.
    - Verify the repository permissions.
    - Test the repository manually:
    ```
    git clone https://<username>:<pat>@github.com/<your-username>/SampleApp.git
    ```

###  Part 5: Enhancement for bonus

- Enhance the Pipeline:

    - Automatically triggers a build  after each push to the repo via the webhook in github
    - Add test scripts and run them in the Jenkins build and integrate tools like pytest or unittest for Python projects.
    - Automate deployment steps (e.g., Docker or cloud deployment).

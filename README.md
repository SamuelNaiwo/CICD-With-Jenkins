# CI/CD With Jenkins

![Alt text](img/CICD_CICD.webp)

## What is Jenkins?

- An open source automation server which enables developers around the world to reliably build, test, and deploy their software.

### Other Tools Available

- Circle CI
- GitLab
- Bamboo
- Team City
- Buddy
- Travis CI

## What is CI?

- The practice of automating the integration of code changes from multiple contributors into a single software project.

## Difference between CD & CDE?

What Continuous Delivery (CD) Does
- CD automatically deploys releases to a testing or staging environment. It requires human intervention to deploy a release from staging to production.

What Continuous Deployment (CDE) Does
- CDE automatically deploys releases from building through testing and into production. It simply checks to see all tests have past and releases it to customers.

# Running Test On Jenkins Through GitHub

## Set Up SSH Key to GitHub

1. Open your terminal window.

2. Change directory into your .ssh folder.
3. Generate a new key using the command below.
    ```
    ssh-keygen -t rsa -b 4096 -C <email address>
    ```
4. Name the key to your preference. I named mine `samuel-jenkins-key`.
5. Open your public ssh key with the command below.
    ```
    sudo cat <key-name>.pub
    ```
6. Copy your ssh key to paste into your GitHub repo.
7. Navigate to your GitHub repo and click on the settings tab.
8. Click deploy keys under security.
9. Click add deploy key.
10. Title your new key and paste your ssh key into the box below.
11. Click add key in the green box and you should see your new kwy.

## Set Up Jenkins

1. Open Jenkins in your browser and log in.

2. Click `New Item` on the left hand side to create a new job.

    ![Alt text](img/jen_new_item.png)

3. Enter the name you would like to name your item. I used `samuel-CI` since we are using it for continuous intergration (CI)
4. Select `Freestyle project`
5. Click OK to create. 
6. In the `Description` box write a few lines to explain what this job is for. I entered `Building a CI for automating testing`.
7. Select `Discard old builds` box and set max number of builds to `3`

    ![Alt text](img/Discard%20old%20builds.png)

8. Select `GitHub project` and in the project URL and paste your GitHub repo URL using HTTPS.
    - Navigate to your GitHub repo.

    - Click `<> Code` in the green box.
    - Select HTTPS and copy the URL.

    ![Alt text](img/GitHub%20HTTPS%20URL.png)

    - Paste the URL and paste on Jenkins.

9. Under Office 365 Connecter, select `restrict where this project can be run`. We had one set up already named `sparta-ubuntu-node`

    ![Alt text](img/Office%20365%20connector.png)

10. Under source code management select git.
11. Copy your SSH URL from your GitHub repo and paste it into the `Repository URL` box.
12. Click add key and select Jenkins.
13. For the kind tab select `SSH Username with private key`.
14.	Enter the username for what you would like to name this key. (Should be the same as your ssh key you created)
15.	Select enter directly next to private key and click add.
16.	Open your terminal and open your private ssh key with the command below.
    ```
    sudo cat <key-name>
    ```
17. Copy your private ssh key and open Jenkins again. Then, paste your private ssh key in the box.
17.	Click add at the bottom to add ssh key.
18. Sele
18.	Change branches to build name from master to `main` as this is the branch on GitHub.
19.	Under `Build Environment` select `Provide Node & npm bin/ folder to PATH`. We had one created for us already.
20.	Under Build click `Add build step` and select `Execute shell`.

    ![Alt text](img/build_step.png)

21. Enter commands to navigate to where your app is saved and run tests.
    ```
    cd app
    npm install
    npm test
    ```
22. Click `Build Now` and wait for the build history to update. When the new build shows up, click the dropdown as show below to access `Console Output` and click it.
    
    ![Alt text](img/build_now.png)

23. Once you've clicked it, it will show everything that has run in the background and we can check to see if all tests have passed.

    ![Alt text](img/success.png)

# Automate Build Proccess On Jenkins With Webhook

## Create Webhook On Github

1.	Open git hub and navigate to your repository where your app is stored.

2.	Click on the settings tab.
3.	Click on webhook and click add webhook
4.	Copy IP address from Jenkins and paste into payload url `http://35.178.11.196:8080/github-webhook/`. We used `github-webhook` at the end to show to connection we wanted to use.
5.	Select `application/json`
6.	Click `Create webhook`
7.	Open Jenkins in browser and find your job you created. Hover over your job and you will see an arrow next to it. Click the arrow and click `configure`.
8.	Scroll down to `Build triggers` and select `GitHub hook trigger`. Save the changes after.
9.	Open GitHub and make changes to your README.md on GitHub where you previously made the webhook.
10.	Name the commit change and commit the changes.
11.	Check Jenkins to see if it builds automatically.

## Check Connection From Local Machine To Jenkins With Webhook

1.	Open your IDE and navigate to folder where you made changes on GitHub.

2.	Pull the changes you made on GitHub using `git pull <origin> <branch>`
3.	Make a change to your README.md file.
4.	Add the changes with `git add`. Commit the changes with `git commit -m “”`. Push the changes to GitHub with `git push <origin> <branch>`.
5.	Open Jenkins and it should be built automatically for you.

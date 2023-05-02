# CI/CD With Jenkins

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

- 

## Difference between CD & CDE?

- Continuous delivery automates deployment of a release to a staging or testing environment, while continuous deployment automatically deploys every release to production.

What Continuous Delivery (CD) Does and Doesn't Do.
- CD automatically deploys releases to a testing or staging environment.
- CD does not automatically deploy code changes to production.
- CD does require human intervention to deploy a release from staging to production.

What Continuous Deployment (CDE) Does and Doesn't Do.
- CDE automatically deploys releases from building through testing and into production.
- CDE doesn't require human intervention.
- CDE doesn't ensure your testing culture and protocol is up to snuff. It simply looks for a checked box and, if it finds the box is checked, deploys the release to production and beyond.

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

1. Open Jenkins in your browser.

2. Click `New Item` on the left hand side to create a new job.
3. Enter the name you would like to name your item. I used `samuel-CI` since we are using it for continuous intergration (CI)
4. Select `Freestyle project`
5. Click OK to create. 
6. In the `Description` box write a few lines to explain what this job is for. I entered `Building a CI for automating testing`.
7. Select `Discard old builds` box and set max number of builds to `3`
8. Select `GitHub project` and in the project URL and paste your GitHub repo URL using HTTPS.
    - Navigate to your GitHub repo.

    - Click `<> Code` in the green box.
    - Select HTTPS and copy the URL.
    - Paste the URL and paste on Jenkins.

9. Under Office 365 Connecter, select `restrict where this project can be run`. We had one set up already named `sparta-ubuntu-node`
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
18.	Change branches to build name from master to `main` as this is the branch on GitHub.
19.	Under `Build Environment` select `Provide Node & npm bin/ folder to PATH`. We had one created for us already.
20.	Under Build click `Add build step` and select `Execute shell`.
21. Enter commands to navigate to where your app is saved and run tests.
    ```
    cd app
    npm install
    npm test
    ```
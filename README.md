# cypressAPICircleCi
To set up Circle CI via GitHub for cypress please follow the steps in: https://circleci.com/blog/api-testing-with-cypress/


### Initialize the directory and set up Cypress

We need to create a directory and initialize an empty node project.

mkdir CypressE2EPractice && cd CypressE2EPractice

npm init -y 

npm init -y creates node project and creates package.json file.

### Install the Cypress framework in the same folder using the following command.

npm install cypress --save-dev

Run the following command to initialize the cypress for the first time and creates default directories and files.

npx cypress open

Cypress will open. Please select E2E and click continue. It will create a cypress configuration file. Select Browser and start writing the cypress e2e script.

### Initializing git and pushing to circleci

Initialize git using the following command.

git init

Add the .gitignore file in the root directory and add node_modules in the file. This will prevent node_module (npm-generated modules) whenever we push our changes to Git Hub (remote).

push the changes to remote using git. The details can be found in: https://gist.github.com/mindplace/b4b094157d7a3be6afd2c96370d39fad

### Now, it is time to write the CI pipeline.

Create a .circleci folder in the root directory and write a config.yml file with the following code.

version: '2.1'
orbs:
  cypress: cypress-io/cypress@3
workflows:
  use-my-orb:
    jobs:
      - cypress/run:
          cypress-cache-key: custom-cypress-cache-v1-{{ arch }}-{{ checksum "package.json" }}
          cypress-cache-path: ~/.cache/custom-dir/Cypress
          cypress-command: npx cypress run
          node-cache-version: v2

In this configuration, CircleCI pulls Cypress orbs and uses the configuration to run Cypress tests. Commit changes and push it to remote. Login to CircleCI and navigate to the projects. Find the corresponding repo and click setup project.

***We may get the following error:*** 

If we get such an error then we need to update security settings at the organizational level. Please change options to:

Yes
Allow all members of my organization to use uncertified orbs (partner and community) in project configuration, and publish development versions of orbs.

Now we can write a cypress test script, whenever we push the new change to remote then the build will be triggered. The practice script can be found in the git repo: https://github.com/untoldstory69/cypressE2EPractice/tree/main.

We will explore both examples pass and fail.

Once we push the changes to remote, it will show running in circleci.

Since we intentionally failed this script, the result will show failed. We can see the reason why it failed when we dig into the details.

Now we will fix the error and push the code to remote. The build will be automatically triggered and this time the status will show success.

By following the above steps, we have successfully integrated the cypress script with Circleci and the build is trig

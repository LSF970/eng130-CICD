# Jenkins Jobs guide

## CI - Continuous Integration
Step 1 - General settings
- Keep the number of saved builds to 3, we don't want to store too many
- Link to your repo's project url (HTTPS url)

![Alt text](/images/ci-1.png "General settings")

Step 2 - Make the code run on an agent node

![Alt text](/images/ci-2.png "365 connector")

Step 3 - Connect Jenkins to GitHub repo (dev branch not main)
- Repo url here is SSH url not HTTPS
- Note the use of a new SSH key pair, between Github and Jenkins

![Alt text](/images/ci-3.png "Source code management")

Step 4 - Build triggers and environment
- Tick 'GitHub hook trigger' so that the job will run when a ppush is made to dev branch
- Make sure the environment has NodeJS installed

![Alt text](/images/ci-4.png "Build triggers and environment")

Step 5 - Build
- Add an execute shell
- cd into the correct directory and run `npm install` and `npm test`

![Alt text](/images/ci-5.png "Build settings")

Step 6 - The test result should be documented in Jenkins console output

## CI Automate Git merge - Dev to Main after successful test
Step 1 - General settings
- Keep the number of saved builds to 3, we don't want to store too many
- Link to your repo's project url (HTTPS url)

![Alt text](/images/merge-1.png "General settings")

Step 2 - This can be run anywhere so node doesn't need to specified

![Alt text](/images/merge-2.png "365 connector")

Step 3 - Connect Jenkins to GitHub repo,  make sure it is set to dev branch
- Repo url here is SSH url not HTTPS
- Same SSH key as last time
- Here we need to add an additional behaviour. Select 'Merge before build'
- Set name of repo to 'origin'
- Set branch to merge to to 'main'
- Set merge strategy to 'default'
- FF mode is up to you
- Username and email is also up to you, will show on GitHub when merge occurs.

![Alt text](/images/merge-3.png "Source code management")

Step 4 - Build triggers and environment
- Tick 'build after other projeccts are built' and select the previous job (ci)
- make sure to only trigger is build is stable
- Build environment does not matter, the instance is just pushing code to GitHub

![Alt text](/images/merge-4.png "Build triggers and environment")

Step 5 - Build
- No shell needed
- Insted add a Post Build Action. Git Publisher is the one we want.
- Tick 'Push only if build succeeds' and 'Merge Results'

![Alt text](/images/merge-5.png "Build settings")

Step 6 - Check results
- Make a basic change to the code in dev branch
- Push that code
- CI job should trigger and test new code
- It should pass and trigger the merge job
- Merge job should pass and merge dev with main
- Check GitHub to see if code was changed

## CD - Continuous Deployment
Step 1 - General settings
- Keep the number of saved builds to 3, we don't want to store too many
- Link to your repo's project url (HTTPS url)

![Alt text](/images/cd-1.png "General settings")

Step 2 - Run the deployment on a ubuntu node

![Alt text](/images/cd-2.png "365 connector")

Step 3 - Connect Jenkins to GitHub repo,  make sure it is set to dev branch
- Repo url here is SSH url not HTTPS
- Same SSH key as last time
- Make sure the branch is now set to 'main'

![Alt text](/images/cd-3.png "Build settings")

Step 4 - Build triggers
- Set the job to start after a successful merge (previous job)

![Alt text](/images/cd-4.png "Build settings")

Step 5 - Build Environment
- Make sure Nodejs is installed
- Give Jenkins SSH agent with the .pem file

![Alt text](/images/cd-5.png "Build")

Step 6 - Build
- Add an execute shell
- Use rsync to get the new files across
- cd into correct directory
- Start the app in the background 

![Alt text](/images/cd-6.png "Build")
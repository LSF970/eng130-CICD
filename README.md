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


# Content Queue Workflow template repository

This template serves as a starting point to use the Content Queue in your own repositories.

## Create a new repo

Use the green "Use this template" button at the top to create a new repository using this template. This will copy
all necessary files to the new repository and you do not have to copy anything yourself. Of course you could
also create an empty repository and transfer files over there manually, up to you!

## Set up actions to your liking

### Adding new issues to the project board

The `add-issue.yml` workflow runs every time a new issue gets created in your repository. The action then adds
the issue to the project in the right column. You can define which project and column to use in your repository
by specifying `project` and `column` respectively.

Currently this needs a GitHub Personal Access Token to work. See below for more info on naming.

### Remove issue

Sometimes issues get closed without being tweeted. This could for example be if an idea gets rejected, or an old
suggestion is now longer relevant. The `remove-issue.yml` workflow runs every time an issue gets closed and removes
all existing closed issues from the board. You can define columns to not be considered by specifying them in
the `ignoredColumns` input. This can for example be helpful to keep the list of already published issues around in the
board. Do not forget to adjust the `project` name accordingly as well to reflect your project name.

### Tweeting

There are several parts to the `tweet.yml` workflow, such as parsing the tweet, validation and the actual publishing.
Some of these steps will need configuration to reflect which project column to use for published issues and the
Twitter credentials. The credentials are taken from the GitHub Actions secrets. We will set these up further down.

### Create Board

Currently we do not automatically create a board for you. Create a new board through the GitHub Projects UI. You can
choose your own name, but you will need to make sure that the workflow inputs (see above) are correctly reflecting your
project board name. Then create the necessary columns in the board: `Backlog`, `To Tweet` and `Tweeted`. You can rename
those to your liking as well, and again these names will need to be adjusted in the workflow inputs as needed. Additional
columns can be created as well for better organization depending on your workflow. You could for example add a "Proofread"
column for a 4-eye principle review of the content. These additional columns are not automated.

## Create a Twitter App

You need to create a Twitter App to get the credentials to use their API. This will require a developer account. You will
need to sign up for a developer account with the account you intend to use with the `content-queue`. You can find more
information on how to sign up and how to create a new app in [their documentation](https://developer.twitter.com/en/docs/twitter-api/getting-started/getting-access-to-the-twitter-api).

## Add credentials to GitHub Actions Secrets

As you now have set up the Twitter App, you can get the credentials to use. Go to your repository settings, click on
"Secrets" in the sidebar and then choose "Actions". Now you can add new secrets through the "New repository secret"
button on the top right. You will need the following secrets. You can find all of these in the "Keys and tokens"
tab of your Twitter App in the Twitter Developer Portal. **Make sure that the Authentication Tokens have "Read and Write" permissions!**

| Secret Name                  | Secret value                                  | Required |
|------------------------------|-----------------------------------------------|----------|
| TWITTER_CONSUMER_KEY         | Consumer Keys -> API Key                      | Yes      |
| TWITTER_CONSUMER_SECRET      | Consumer Keys -> API Secret                   | Yes      |
| TWITTER_ACCESS_TOKEN_KEY     | Authentication Tokens -> Access Token         | Yes      |
| TWITTER_ACCESS_TOKEN_SECRET  | Authentication Tokens -> Access Token Secret  | Yes      |

Additionally to this, you need the following secrets:

| Secret Name       | Secret value                                                                                                             | Required |
|-------------------|--------------------------------------------------------------------------------------------------------------------------|----------|
| GHPROJECT_TOKEN   | GitHub Personal Access Token, created through GitHub Settings -> Developer Settings. This token needs the "repo" scope.  | Yes      |


## Test it

Now that everything is set up, we can test it!

* Create a new issue using the "New issue" button in the "Issues" tab in your repository
* Choose the template you want to test
* Fill out the necessary information
* Once the issue is created, wait a bit and verify that the new issue does show up on your project board
* Review the content of your issue and make sure this is something you want to tweet
* If all is good, move the card to the respective "To Tweet" column
* Go to the "Actions" tab and see the action start, once the action is marked as green, go to your Twitter account and see your new tweet
* Verify that the issue got closed and moved to the "Tweeted" column
* Invite others to suggest post ideas by filing an issue themselves!

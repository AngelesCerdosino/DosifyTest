# Post Purchase Context API

![technology Go](https://img.shields.io/badge/technology-go-blue.svg)

This is a wrapper API for Post Purchase context information about MELI entities.

## Decision Tree

The Use Cases of the application are defined by a Decision Tree that runs taking a context as input. Check out the diagram of the current Decision Tree below:

![Decision tree](/.github/docs/post-purchase-context_decision-tree.png)

Link: <https://drive.google.com/file/d/1GFTVpHlIWlMbIaS1WA1KaziEncM55nUf/view?usp=sharing>

ATTENTION: _Whenever you make changes to the Decision Tree always keep the diagram updated._

## Dependency management

Dependencies are managed using Go Modules. It is recommended that you add the following env var to your local environment (usually in your ~/.profile or equivalent):

```bash
export GOPRIVATE="github.com/mercadolibre"

# If using Go 1.13+ you can run once:
go env -w GOPRIVATE="github.com/mercadolibre"
```

Go modules clones using `https` by default, in order to authenticate with Github and be able to clone private repositories you usually need to use `ssh`. In order to force `git` to use `ssh` on Github, you need to set the following lines in your `~/.gitconfig`:

```bash
[url "ssh://git@github.com/"]
    insteadOf = https://github.com/
```

## Linting

For linting we use `golangci-lint` that is run by the Fury CI. To run it locally you should:

- Install [golangci-lint locally](https://golangci-lint.run/usage/install/#local-installation) or [integrate](https://golangci-lint.run/usage/integrations/) with your favorite editor:

```bash
go get -u github.com/golangci/golangci-lint/cmd/golangci-lint
```

- Then [run locally](https://github.com/golangci/golangci-lint#local-installation) using `golangci-lint run`. Output should be something like (or empty in case of success):

```bash
$ golangci-lint run

domain/claim.go:110:3: S1008: should use 'return <expr>' instead of 'if <expr> { return <bool> }; return <bool>' (gosimple)
                if c.Resolution.ClosedBy == playerRole {
                ^
```

## Development Environment

In order to be able to run the project locally you must set the kvs_environment variable first:

MacOS/ Linux:

```bash
  export GO_ENVIRONMENT="gokvsclient_test"
```

Windows:

```bash
  set GO_ENVIROMENT="gokvsclient_test"
```

## Generating mocks

First of all you should install the `mockgen` tool following the steps in [`GoMock`](https://github.com/golang/mock).

Once you have installed it you can mock any **interfaces** in the project going to the __interface__ file directory and executing:

```bash
mockgen -source=[file].go -destination=[path to mocks package]/[mock_file].go -package mocks
```

## [Babel](https://github.com/mercadolibre/go-meli-toolkit/tree/master/babel)

### Library Usage

```go
import (
  "github.com/mercadolibre/go-meli-toolkit/babel"
  "golang.org/x/text/language"
)

locale := language.MustParse("es-MX")
babel.Tr(locale, "Translation Key")
babel.Trn(locale, count, "Singular Key", "Plural Key")

```

### Installing the client

```bash
$ go get github.com/mercadolibre/go-meli-toolkit/babel/babel
no output

$ babel
babel is a command line tool to help managing tasks like update, upload and download translations from Babel.
```

### Scanning your code for translations keys

Just run `babel scan` on your project root.
This will update a file called `.../config/translations/messages.po` which should contain all the translation keys found in your code.

### Upload messages to Babel

Run `babel upload` and start translating your messages inside Babel.

### Download the translation bundle from Babel

Run `babel download`.
And the translation files will be updated on the `.../config/translations` folder.

## Configurations - Enable or Disable

To activate or deactivate configurations, you must use the [configuration API](https://web.furycloud.io/#/post-purchase-context/configurations)



## Dashboard

To see the metrics and performance of the production environment, go to the [Post Purchase Context](https://app.datadoghq.com/dashboard/p7i-uas-ni5/post-purchase-context?from_ts=1585067224443&to_ts=1585153624443&live=true&tile_size=m) Datadog dashboard.

## GitHub to Slack

To connect Github to Slack visit this link:
[Github to Slack](https://slack.com/intl/pt-br/help/articles/232289568-GitHub-para-Slack#como-instalar-o-app)

[Incoming WebHooks](https://meli.slack.com/services/B0115BXQX1B)

[workflow-syntax-for-github-actions](https://help.github.com/pt/actions/reference/workflow-syntax-for-github-actions)

[slack-pr-open-notification-action](https://github.com/jun3453/slack-pr-open-notification-action)

### post-purchase-context.yml

```yml
name: Slack pull request notification

on:
  pull_request:
    branches:
      - develop
      - '!release/**'
  push:
    branches:
      - develop
      - '!release/**'
    types: [opened, reopened]
  pull_request_review_comment:
   types: [created, deleted, edited]
  issue_comment:
    types: [created, deleted]
  issues:
    types: [created, deleted]
  deployment:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Notify slack
        env:
          SLACK_WEBHOOK_URL : https://hooks.slack.com/services/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
          PULL_REQUEST_NUMBER : ${{ github.event.pull_request.number }}
          PULL_REQUEST_TITLE : ${{ github.event.pull_request.title }}
          PULL_REQUEST_AUTHOR_NAME : ${{ github.event.pull_request.user.login }}
          PULL_REQUEST_AUTHOR_ICON_URL : ${{ github.event.pull_request.user.avatar_url }}
          PULL_REQUEST_URL : ${{ github.event.pull_request.html_url }}
          PULL_REQUEST_COMPARE_BRANCH_NAME : ${{ github.event.pull_request.head.ref }}
          PULL_REQUEST_BASE_BRANCH_NAME : ${{ github.event.pull_request.base.ref }}
          PULL_REQUEST_BODY : ${{ github.event.pull_request.body }}
          IS_SEND_HERE_MENTION : true
          PULL_REQUEST_COMENT: ${{ github.event.pull_request. }}
        uses: jun3453/slack-pr-open-notification-action@v1.0.1

```

## Devex Information

- [business diagram](https://docs.google.com/drawings/d/1BiVgdwmqFgIuA-gCRT-sMhCavVQ2COaP34YByncBFtE)

- [decision tree](https://docs.google.com/drawings/d/1KJ3jWzdUd1zxWo4GoniYMuidpx5s8ThxVy4NbvSKeOo)

- [architecture diagram](https://docs.google.com/drawings/d/1UWWKS1yyoqiIgjWg-Q2bTpIuAI0St0VeGQ0UuJsFQZE)

- [process flow](https://docs.google.com/drawings/d/12N29PFj-iDSM-_GVzci1PhYiEgRmwgrInNAoIskazzg)

- [consumer architecture](https://docs.google.com/drawings/d/1FFhsS5odq9mYvji0atdIntkaIAZxye9KCH435RMCbOc)

- [TTL catalog](https://docs.google.com/drawings/d/1CiyKtMUeu-Blo3Ulr_speB4LksxlMPqXrobxLebIT6k)


## Troubleshooting

- Can't do a GET in localhost

Response example:
 ```
{
    "message": "Can't get return: Message: Error executing GET;Error Code: rest_client_error;Status: 400;Cause: [MockUp nil!]",
    "error": "internal_server_error",
    "status": 500,
    "cause": [
        "Can't get return: Message: Error executing GET;Error Code: rest_client_error;Status: 400;Cause: [MockUp nil!]"
    ]
}
```
FIX: Add this env variable to your system GO_ENVIRONMENT=gokvsclient_test
## KYC Information

- [decision tree](https://docs.google.com/drawings/d/1DB77oHJKSkmpHURzmXDlDiiViDzA2BG9uQHCZ_E5acI)

- [process flow](https://docs.google.com/drawings/d/1CMAoAmJQv5PwlCKG3KfmsJp31UrThosaX4lPQeqrnFE)

## Questions

- [ci-cd@mercadolibre.com](ci-cd@mercadolibre.com)
- [fury@mercadolibre.com](fury@mercadolibre.com)

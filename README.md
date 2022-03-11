# oneconnect-build-and-restore-action
This is a GitHub Action meant to be used as a composite action within an existing workflow. This action encapsulates setting up a checkout, build, test and Sonarcloud quality gate check in one step.

The action encapsulates the following other actions:

- actions/checkout
- actions/setup-dotnet
- dorny/test-reporter


## Inputs

### `ghp-username`

**Required** Github user name to restore nuget package

### `ghp-password`

**Required** Github access token to restore nuget package

### `solution-name`

**Required** Solution name to build

### `dotnet-version`

**Required** Dotnet version to use

### `initialise-sonar-cloud`

**Required** Falg to set if Sonarcloud analysis needs to be performed or not

### `sonarcloud-project-id`

**Optional** Sonarcloud project id

### `sonarcloud-token`

**Optional** Sonar token

### `github-token`

**Optional** Github token


## Usage
You can use this composite Action in your own workflow by adding:

```yml
build-dependabot:
    name: Build (dependabot)
    if: ${{ github.actor != 'dependabot[bot]' }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout, Build and Restore
        uses: awazevr/oneconnect-build-and-restore@v1.0.4
        with:
          ghp-username: ${{ secrets.GHP_USERNAME }}
          ghp-password: ${{ secrets.GHP_PASSWORD }}
          solution-name: "OneConnect.PricingSync.sln"
          dotnet-version: "6.0.x"
          initialise-sonar-cloud: false

  build:
    name: Build
    if: ${{ github.actor != 'dependabot[bot]' }}
    runs-on: ubuntu-latest
    permissions:
      actions: write
      checks: write
      contents: write
      deployments: write
      id-token: write
      issues: write
      discussions: write
      packages: write
      pages: write
      pull-requests: write
      repository-projects: write
      security-events: write
      statuses: write
    steps:

      - name: Checkout, Build and Restore
        uses: awazevr/oneconnect-build-and-restore@v1.0.4
        with:
          ghp-username: ${{ secrets.GHP_USERNAME }}
          ghp-password: ${{ secrets.GHP_PASSWORD }}
          solution-name: "OneConnect.PricingSync.sln"
          dotnet-version: "6.0.x"
          sonarcloud-project-id: "awazevr_ocint-pricing-sync-services"
          sonarcloud-token: ${{ secrets.SONAR_TOKEN }}
          initialise-sonar-cloud: true
          github-token: ${{ secrets.GITHUB_TOKEN }}

```


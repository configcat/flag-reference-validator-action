# ConfigCat Feature Flag Cleanup for GitHub Actions [archived]

**This repository has been archived**. The project became obsolate after the release of [ConfigCat's Scan Repository GitHub Action](https://github.com/configcat/scan-repository).

This GitHub Action is a utility that discovers [ConfigCat feature flag](https://configcat.com) usages in your source
code and validates them against your own feature flags on the [ConfigCat Dashboard](https://app.configcat.com).
Documentation: https://github.com/configcat/feature-flag-reference-validator

## Installation
1. Get your SDK Key from [ConfigCat Dashboard](https://app.configcat.com/sdkkey) and store it as a [GitHub secret](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets) under the name `CONFIG_CAT_SDK_KEY`.

2. Create a new Actions workflow in your GitHub repo.

   - **If you already have an `action.yml` file:** Copy and paste the `ConfigCatFeatureFlagCleanup` job declaration below into the jobs section in your `action.yml` file.
   - **If you don't already have a workflow file:** Create a new file titled `action.yml` in the `.github/workflows` directory of your repository. Copy and paste the following code to `action.yml`.

   ```yaml
   on: push
   name: Example Workflow
   jobs:
     ConfigCatFeatureFlagCleanup:
       name: ConfigCat Feature Flag Cleanup
       runs-on: ubuntu-latest
       steps:
       - uses: actions/checkout@v1
       - name: ConfigCat Feature Flag Cleanup
         uses: configcat/github-action-feature-flag-cleanup@2.0.1
         with:
           configcat-sdk-key: ${{ secrets.CONFIG_CAT_SDK_KEY }}
           fail-on-warnings: false
   ```

  > We strongly recommend that you update the second uses attribute value to reference the latest tag in the [configcat/github-action-feature-flag-cleanup](https://github.com/configcat/github-action-feature-flag-cleanup) repository. This pins your workflow to a latest version of the action.

3. Commit & push `action.yml`.

## Usage

Feature Flag Cleanup Action will run on any push event.

> Will not block PR approvals until you set `fail-on-warnings: true`.

## Configuration options

Add these to the `with` section to enable more functionality.

| Parameter              | Description                                                                        |      Default       |
| ---------------------- | ---------------------------------------------------------------------------------- | :----------------: |
| `configcat-sdk-key`    | The [SDK Key](https://app.configcat.com/sdkkey) for your feature flags & settings. | CONFIG_CAT_SDK_KEY |
| `scan-directory`       | The directory to run flag validations on.                                          |         .          |
| `configcat-cdn-server` | To set a custom ConfigCat CDN server.                                              | cdn.configcat.com  |
| `fail-on-warnings`     | Show warnings or stop on a build error when validation fails.                      |       false        |
| `debug`                | More verbose logging.                                                              |       false        |

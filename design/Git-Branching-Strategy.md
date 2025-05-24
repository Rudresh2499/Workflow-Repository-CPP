# Branching Strategy

## Pre-requisite

- Automate branch creation using workflows for any __feature/__, __dev/__, __stablize/__, __hotfix/__ branches with initial tag push.
- Assuming the initial tag `v0.0.0` on __master__
    - __feature/__ - Create a feature branch from latest __master/__
    - __release/__ - Create a release branch from the latest __master/__
    - __dev/__ - Create a dev branch from the provided __feature/__
    - __stabilize/__ - Create a stabilization branch from the provided __feature/__
    - __prerelease/__ - Create a pre-release branch from the provided __feature/__
    - __hotfix/__ - Create a hotfix branch from the latest __master/__ or __release/__
    - __finalization/__ - Create a finalization branch for a release from the provided __release/__

## Branch level workflows

> [!NOTE]
> To check if any provision is there to fetch the name of the source branch from which a merge took place to any branch during a workflow run. If so a cleanup job can be triggered post successful completion of the CI workflow to delete the source branch.

### master

- _CI_ - The root level CI workflow that is triggered when a pull request is merged to __master__.

### release

- _PR_ - The release level PR workflow that is triggered when any PR is opened, synchronized or re-opened on any __release__
- _CI_ - The release level CI that is triggered when a pull request is merged to __release__

### feature

- _PR_ - The feature level PR workflow that is triggered when any PR is opened, synchronized or re-opened on any __feature__
- _CI_ - The feature level CI workflow that is triggered when a pull request is merged to any __feature__. Equivalent to __master__ level _PR_ workflow.

### dev | stabilize | prerelease | hotfix | finalization

- _DEV_ - The developer build workflow triggered on push to any __dev__, __stabilize__, __hotfix__, __finalization__ branch. Objective is to have a quick code build and UT execution.

> [!IMPORTANT]
> The hotfix branch should have only the DEV workflow enabled as PR workflow for the branch would mean the enabling of PR workflow at master branch. As hotfix branches are merged directly to master it would result in the trigger of a master level CI workflow.

### Repository level workflows

- __BRANCH-CREATION__ - Manually triggered workflow that will be creating the specified level branch from the specified source, conforming to the branch definitions

- __PROMOTION__ - Manually triggered workflow that will be responsible for build promotion and pushing of initial tag onto the specified __feature__, __release__ branches.

- __RELEASE__ - Manually triggered workflow that will be responsible for handling the final promotion to release taking care of the build and distributions

- __NIGHTLY__ - Scheduled triggered workflow that will be responsible to fetch the latest build post the _beta.0_ stage to be tested against the automated test cases marking successful builds with the tag `nightly`
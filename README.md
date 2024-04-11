# Template repo for C# SDK

This template helps developers get started with publishing the C# SDK to NuGet Gellery package repository.

## Prerequisites

The user will need the following:

- A [NuGet Gallery](https://www.nuget.org/) account with an [NuGet API key](https://www.nuget.org/account/apikeys) that allows uploading packages into the desired project

## Contents

This repository contains the following:

- A `README` that contains the instructions
- A GitHub Actions workflow to publish the C# SDK to NuGet Gallery package repository.

## Instructions

1. Create a new target C# SDK Repo by clicking the **Use this template** button at the top of this repository.

1. Set the `NUGET_TOKEN` actions secret in the target SDK repo.

1. If you already have a Control Repo:

    1. Update your `LIBLAB_GITHUB_TOKEN` actions secret to a new token that has access to all your existing SDK repos, as well as this new one.

    1. Update the [`githubRepoName` field in the `csharp` section](https://developers.liblab.com/cli/config-file-overview-language/#githubreponame) of your liblab config file to the name of your new repo.

1. Run the GitHub Action `Generate SDKs using liblab` in the Control Repo that builds the SDK, and raises a PR against this target SDK Repo. This will be triggered automatically when you commit and push the update to the liblab config file.

1. Review and merge the PR.

1. Create a release in the target SDK Repo.

1. The GitHub Action `Publish to NuGet Gallery package repository` in the target SDK Repo will be triggered by the release, and publish the package to NuGet.

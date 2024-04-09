# Template repo for C# SDK  
This template helps developers get started with publishing the C# SDK to NuGet Gellery package repository.

## Prerequisites
The user will need the following:

- A [NuGet Gallery](https://www.nuget.org/) account with an [NuGet API key](https://www.nuget.org/account/apikeys) that allows uploading packages into the desired project

## Contents
This repository contains the following:

- A `README` that contains the instructions
- A GitHub Action workflow to publish the C# SDK to NuGet Gallery package repository.


## Instructions

1. Create a new target C# SDK Repo by clicking the **Use this template** button at the top of this repository.

2. Set the `NUGET_TOKEN` actions secret in the target SDK repo.

3. Run the GitHub Action `Generate SDKs using liblab` in the Control Repo that builds the SDK, and raises a PR against this target SDK Repo.

4. Review and merge the PR.

5. Create a release in the target SDK Repo.

6. The GitHub Action `Publish to NuGet Gallery package repository` in the target SDK Repo publishes the package to the registry.

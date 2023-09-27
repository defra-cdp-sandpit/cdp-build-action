# CDP Terraform Build Action

The action is designed to run Terraform Plan and Apply for a given target.

## What does it do?

* Setup the build environment with AWS role credentials
* Set up terraform CLI
* Access Secrets manager in AWS for relevant required secrets
* Run Terraform init, format, validate, plan and apply

### Example usage

```yaml
steps:
  - name: Terraform Build
      uses: defra-cdp-sandpit/cdp-build-action/tf-build@main
      with:
       environment: infra-dev
       aws_account_id: "12345678910"

```

If any steps are required before building the image, they can be inserted as steps between checking the code out and calling the build action.

## Inputs

`version`

If you wish to use your own versioning systems for whatever reason (maybe a custom one that bumps the package.json version etc), you can override the version number via the version input.

`github-token`

This should always be set to ${{ secrets.GITHUB_TOKEN }}
It’s used to tag the repository when the version is incremented

`image-name`

Overrides the name of the docker image. By default it will use the name of the github repository, but in some use cases (repositories being renamed, multi-project repos etc) it may be required to override this value.

`push`

Sets whether the image will be published to the docker registries. Defaults to true, setting this value to false allows the build action to be used as a dry run/build test.

## Major and Patch releases

By default, each build will bump the minor version (e.g. 0.1.0 -> 0.2.0). To do a major or patch releases simply include `#major` or `#patch` in the commit message.

`git commit -m “bump to #major”`

Would result in 0.55.0 -> 1.0.0, while

```
git commit -a -m “possible bug fix #patch”
```

Would result in 0.55.0 -> 0.55.1

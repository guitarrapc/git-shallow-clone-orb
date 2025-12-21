[![CircleCI](https://dl.circleci.com/status-badge/img/gh/guitarrapc/git-shallow-clone-orb/tree/main.svg?style=svg)](https://dl.circleci.com/status-badge/redirect/gh/guitarrapc/git-shallow-clone-orb/tree/main)
![Orb Version Badge](https://badges.circleci.com/orbs/guitarrapc/git-shallow-clone.svg)

## Git-shallow-clone-orb

## Deprecation Notice

This repository will be deprecated and archived on 2025-12-31 09:00 JST (UTC+9).
After that date, no further updates or maintenance will be provided.

Please migrate your checkout step from this repository’s shallow clone implementation to CircleCI’s official `blobless clone` feature.

**Background**

On September 18, 2025, CircleCI officially released a new
[faster checkout option](https://circleci.com/changelog/introducing-a-faster-checkout-option/) that supports blobless clone.

With this official support in place, this repository’s shallow clone functionality is no longer necessary.

**How to use Blobless Clone on CircleCI**

You can enable blobless clone by setting the `method` key in your `checkout` step.

```yaml
jobs:
  build:
    steps:
      - checkout:
          method: blobless # <- here!
```

**What is a Blobless Clone?**

A blobless clone fetches the entire commit history while deferring file contents (blobs) until they are actually needed.　Because blobless clone provides better flexibility and efficiency, we strongly recommend switching from shallow clone to blobless clone.

Blobless clone

- Keeps full commit history
- Downloads file contents only on demand
- Minimizes initial network transfer

Shallow clone

- Limits commit history
- Downloads all file contents upfront
- Can restrict Git operations

Thank you to everyone who used, tested, and contributed to this repository.

## Usage

See the [orb registry listing](http://circleci.com/orbs/registry/orb/guitarrapc/git-shallow-clone) for usage guidelines.

## Contributing

We welcome [issues](https://github.com/guitarrapc/git-shallow-clone-orb/issues) to and [pull requests](https://github.com/guitarrapc/git-shallow-clone-orb/pulls) against this repository!

## Publish

To publish orb, push new tag to remote.

## Add Test

Add test job in .circleci/config.yml.

```yaml
jobs:
  # Define one or more jobs which will utilize your orb's commands and parameters to validate your changes.
  integration-test-checkout:
    docker:
      - image: cimg/base:stable
    steps:
      - git-shallow-clone/checkout
```

Call it from integration-test_deploy job, and add as orb-tools/dev-promote-prod-from-commit-subject required job.

```yaml
  integration-test_deploy:
    when: << pipeline.parameters.run-integration-tests >>
    jobs:
      - integration-test-checkout # <-- this line!

      - orb-tools/dev-promote-prod-from-commit-subject:
          orb-name: guitarrapc/git-shallow-clone
          add-pr-comment: false
          fail-if-semver-not-indicated: true
          publish-version-tag: false
          requires:
            - integration-test-checkout # <-- this line!
          filters:
            branches:
              only:
                - master
                - main
```

## Basic orb setup

setup orb account and namespace.

```shell
# require perconal api tokens
$ circleci setup
$ circleci namespace create guitarrapc github guitarrapc
$ circleci orb create guitarrapc/git-shallow-clone
```

validate before publish.

```
$ cd ./src
$ circleci orb validate orb.yml
```

publish orb to the alpha.

```
$ cd ./src
$ circleci orb publish orb.yml guitarrapc/git-shallow-clone@dev:alpha
```


publish orb to the dev.

```
$ cd ./src
$ circleci orb publish orb.yml guitarrapc/git-shallow-clone@dev:0.x.0
```

publish orb to the production.

```
$ cd ./src
$ circleci orb publish promote guitarrapc/git-shallow-clone@0.x.0
```

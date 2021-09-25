## git-shallow-clone-orb
[![CircleCI](https://circleci.com/gh/guitarrapc/git-shallow-clone-orb.svg?style=svg)](https://circleci.com/gh/guitarrapc/git-shallow-clone-orb) [![CircleCI Orb Version](https://img.shields.io/badge/endpoint.svg?url=https://badges.circleci.io/orb/guitarrapc/git-shallow-clone)](https://circleci.com/orbs/registry/orb/guitarrapc/git-shallow-clone)

## Usage

See the [orb registry listing](http://circleci.com/orbs/registry/orb/guitarrapc/git-shallow-clone) for usage guidelines.

## Contributing

We welcome [issues](https://github.com/guitarrapc/git-shallow-clone-orb/issues) to and [pull requests](https://github.com/guitarrapc/git-shallow-clone-orb/pulls) against this repository!

## MEMO

* normal push without specific pattern of a tag, dev build will run.
* push tag for production release.
    * master-major.v1.0.0
    * master-minor.v1.1.0
    * master-patch.v1.1.1

> NOTE: `orb-tools/dev-promote-prod` cleanup-tags is set to true to remove `master.*` tags on publish orb to the production.

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
      - integration-test-checkout

      - orb-tools/dev-promote-prod-from-commit-subject:
          orb-name: guitarrapc/git-shallow-clone
          add-pr-comment: false
          fail-if-semver-not-indicated: true
          publish-version-tag: false
          requires:
            - integration-test-checkout
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

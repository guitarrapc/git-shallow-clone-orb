## git-shallow-clone-orb
[![CircleCI](https://circleci.com/gh/guitarrapc/git-shallow-clone-orb.svg?style=svg)](https://circleci.com/gh/guitarrapc/git-shallow-clone-orb) [![CircleCI Orb Version](https://img.shields.io/badge/endpoint.svg?url=https://badges.circleci.io/orb/guitarrapc/git-shallow-clone)](https://circleci.com/orbs/registry/orb/guitarrapc/git-shallow-clone)

## Usage

See the [orb registry listing](http://circleci.com/orbs/registry/orb/mafuyuk/terraform) for usage guidelines.

## Contributing

We welcome [issues](https://github.com/mafuyuk/terraform-orb/issues) to and [pull requests](https://github.com/mafuyuk/terraform-orb/pulls) against this repository!

## MEMO

* normal push without specific pattern of a tag, dev build will run.
* Don't need push tag for production release, but merge to master is enough.
    * master-major.v1.0.0
    * master-minor.v1.1.0
    * master-patch.v1.1.1

> NOTE: `orb-tools/dev-promote-prod` cleanup-tags is set to true to remove `master.*` tags on publish orb to the production.

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
$ circleci orb validate orb.yml
```

publish orb to the dev.

```
$ circleci orb publish orb.yml guitarrapc/git-shallow-clone@dev:0.x.0
```

public orb to the production.

```
$ circleci orb publish promote guitarrapc/git-shallow-clone@0.x.0
```

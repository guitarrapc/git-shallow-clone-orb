## git-shallow-clone-orb


setup orb account and namespace.

```shell
# require perconal api tokens
$ circleci setup
$ circleci namespace create guitarrapc github guitarrapc
$ circleci orb create guitarrapc/git-shallow-clone
```

validate before publish
```
$ circleci orb validate orb.yml
```

publish

```
$ circleci orb publish orb.yml guitarrapc/git-shallow-clone@dev:first
```

push to production.

```
$ circleci orb publish promote guitarrapc/git-shallow-clone@0.0.1
```


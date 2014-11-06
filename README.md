# lein-auto-release

A Leiningen plugin to handle automatic merging of branches in the
lein release process. Includes other git utils.

## Usage

Put `[com.andrewmcveigh/lein-auto-release "0.1.7"]` into the
`:plugins` vector of your project.clj.

```clojure
(defproject
  ...

  :plugins [[com.andrewmcveigh/lein-auto-release "0.1.7"]]

  :release-tasks [["auto-release" "checkout" "master"]
                  ["auto-release" "merge-no-ff" "develop"]
                  ["vcs" "assert-committed"]
                  ["change" "version"
                   "leiningen.release/bump-version" "release"]
                  ["auto-release" "update-release-notes"]
                  ["auto-release" "update-readme-version"]
                  ["vcs" "commit"]
                  ["vcs" "tag" "v"]
                  ["deploy" "clojars"]
                  ["vcs" "push"]
                  ["auto-release" "checkout" "develop"]
                  ["auto-release" "merge" "master"]
                  ["change" "version" "leiningen.release/bump-version"]
                  ["vcs" "commit"]
                  ["vcs" "push"]])
```

## Available Tasks

### #'checkout

Checkout branch/tag/etc.

### #'checkout-latest-tag

Checkout the latest (largest version number) tag.

### #'merge

Git merge

### #'merge-no-ff

Git merge --no-ff

### #'tag

Git tag, no signing

### #'update-readme-version

Updates `README.md` with the current (new) version number, provided the latest-tag
matches with the version in the `README.md`.

### #'update-release-notes

Compiles and prepends the commit log from the latest tag to now, filtering
key words/phrases. E.G., "Merge branch ..."

### #'update-marginalia-gh-pages

Takes the `./docs` folder generated by `lein marg`, and moves it to the
gh-pages branch. If gh-pages doesn't exist, it creates it, and sets it up.
You should have run `lein marg` prior to this task, maybe as a `:release-task`.


## License

Copyright © 2014 Andrew Mcveigh

Distributed under the Eclipse Public License, the same as Clojure uses. See
the file LICENSE.

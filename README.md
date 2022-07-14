# @kjots/snyk-issue-peer-dependencies

## Snyk issue: peer dependencies

This project demonstrates an issue where Snyk will not report issues in transitive peer dependencies that have been
installed automatically by npm v7 and above when testing against `package-lock.json`.  It also shows that Snyk _will_
report the issues when testing against `package.json` after installing `node_modules`.

This project has a dependency (`@kjots/test-dependency`) that itself has a peer dependency (`angucomplete-alt@3.0.0`) that
contains a cross-site scripting issue.  This peer dependency is installed automatically when using npm v7 and above and
thus is present in both `node_modules` and `package-lock.json`.

Note that the peer dependency is _not_ included as a direct dependency by this project.

This issue has been raised on the [Snyk CLI](https://github.com/snyk/cli) project: https://github.com/snyk/cli/issues/3458

### Scenario 1: snyk test --file=package-lock.json (not working as expected)

This scenario tests against `package-lock.json` and shows that the cross-site scripting issue in `angucomplete-alt@3.0.0`
is _not_ reported.

The results of this scenario are the same regardless if `node_modules` are installed or not.

```
$ snyk test --file=package-lock.json

Testing /Users/kjots/Development/Git/github.com/kjots/snyk-issue-peer-dependencies...

Organization:      [redacted]
Package manager:   npm
Target file:       package-lock.json
Project name:      @kjots/snyk-issue-peer-dependencies
Open source:       no
Project path:      /Users/kjots/Development/Git/github.com/kjots/snyk-issue-peer-dependencies
Licenses:          enabled

✔ Tested 1 dependencies for known issues, no vulnerable paths found.
```

### Scenario 2: snyk test --file=package.json (working as expected)

This scenario tests against `package.json` and shows that the cross-site scripting issue in `angucomplete-alt@3.0.0` 
_is_ reported.

This scenario requires `node_modules` to be installed.

```
$ snyk test --file=package.json

Testing /Users/kjots/Development/Git/github.com/kjots/snyk-issue-peer-dependencies...

Tested 2 dependencies for known issues, found 1 issue, 1 vulnerable path.


Issues with no direct upgrade or patch:
  ✗ Cross-site Scripting (XSS) [Medium Severity][https://snyk.io/vuln/SNYK-JS-ANGUCOMPLETEALT-73622] in angucomplete-alt@3.0.0
    introduced by angucomplete-alt@3.0.0
  No upgrade or patch available



Organization:      [redacted]
Package manager:   npm
Target file:       package.json
Project name:      @kjots/snyk-issue-peer-dependencies
Open source:       no
Project path:      /Users/kjots/Development/Git/github.com/kjots/snyk-issue-peer-dependencies
Licenses:          enabled
```

# FACT-Finder Web Components Feature Lab

This repository is used to flesh out new features by defining their specifications.

## Branches

There are several permanent branches with certain tasks.

### master

The _master_ branch reflects all feature specifications that have been implemented.

### feature-template

The _feature-template_ branch holds the latest template from which all feature branches must start.

### feature branches

Feature branches start from the _feature-template_ branch. Specifications are written and reviewed here. For each feature a new directory in `/specs` shall be created and the `feature-template.md` file shall be copied into the new feature directory.
```
/
- specs/
    feature-template.md
    - feature1/
        feature1.md (based on feature-template.md)
```

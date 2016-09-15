## Categorizing GitHub Issues Using Milestones

After review of each github issue, the issue should either be marked with a milestone or closed if we decide not to address the issue further. We will have three milestones:

1. _NextRelease_: Issues which are to be included in the next stable release. Whenever a release is made, this milestone will be renamed with the version number of the new release and a new empty _NextRelease_ milestone will be created.
2. _FutureRelease_: Issues which are planned for some future release. Whenever a release is completed, issues will be selected from this milestone and moved into the new _NextRelease_ milestone.
3. _IdealRelease_: Issues which contain interesting ideas that someone may wish to take on in the future, but will not normally be reviewed for possible inclusion into a near-term release.

## Workflow for GitHub Issues

**Create a new branch for each issue using the issue number** and a brief mnemonic description using dashes as word separators (e.g. 500-crypto-api). If the branch is being used for several tightly related issues, select the most appropriate issue and link the other issues to that issue in GitHub.

When the new branch has been adequately tested it (or if it is not possible to test it on its own), merge it into the _integration_ branch to allow it be tested against other code commits for possible interactions.

## Creation and Testing of a Release

The release manager will merge all the branches associated with issues marked with the _NextRelease_ milestone to the master branch to create a stable release of Graphene. The release manager will will use a label to tag each issue after he has merged the associated branch to master. Barring an emergency, all such branches should have been previously tested in the **integration** branch and should be marked as closed. This version will then be tested on the real network prior to making an official announcement of the new release.

## Assignment of GitHub Issues (Avoiding Duplication of Work)

Anyone can assign themselves to an unassigned issue. If you wish to work on a issue assigned to someone else, you must first discuss the issue with the currently assigned person before beginning work on it.

## GitHub Labels

GitHub labels are very useful for categorizing issues in terms of severity, task difficulty, impact (e.g. causes a hardfork), etc. However, they become less useful as categorizations if new labels with overlapping meanings are created, so please be sure there are no [similar labels](https://github.com/Rsteem/rsteemit.com/wiki/Labels) before creating new ones.

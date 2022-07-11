# Forge issue with recursive update
When updating a dependency using `forge update`, forge updates all nested  dependencies to the latest version (if these nested dependencies are also managed using foundry / git submodule).

This can result in the installed library being in a different state then it is in it's own source repository. In case the latest version of the nested dependency is not backwards compatible this can lead to the installed version of the package breaking.


## The example

This example project imports the package [outdated-foundry-package](https://github.com/ckoopmann/outdated-foundry-package) which is implemented in v0.6 solidity and depends on an old version of the [openzeppelin-contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) repo.

When initially installing the package using `forge install`, everything works fine, but once the user calls `forge update lib/outdated-foundr-package` the nested oz dependency is updated to the latest version. This version will require a solidity version >0.8 and therefore the v.0.6 based package will not compile anymore.

## How to reproduce this issue

You can run the same steps that are run in the github-action locally to reproduce the issue:

1. Install dependencies `forge install`
2. Run `forge test` and observe test passing
3. Update package with `forge update lib/outdated-foundry-package`
4. Repeat step 2 and observe compilation failure


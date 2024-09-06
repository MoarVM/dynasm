MoarVM Mirror Of LuaJIT's DynAsm
===

This repository hosts the files from luajit's repository that relate to
DynAsm, including a one-file implementation of lua called minilua.

In September of 2024, we filtered the upstream repo using this command:

    mkdir dynasm_filtered
    cd dynasm_filtered
    git init
    cd ..
    git clone https://repo.or.cz/luajit-2.0.git
    cd luajit-2.0
    git filter-repo --path src/host/minilua.c --path dynasm/ --path-rename src/host/: --path-rename dynasm/: --source . --target ../dynasm_filtered/

This results in a repository with initially an empty branch checked out,
and usable branches in the "origin" remote.

Updating This Repository
----

If you want to update this repository, following the steps above is a
good starting point.

Afterwards, proceed as follows:

Run `git checkout` in the `dynasm_filtered` repository to choose the tag
you're interested in. After that, in this repository you can `git fetch` 
from `dynasm_filtered` and get a ref called `FETCH_HEAD` that points at
the commit at the tag or branch you had checked out in `dynasm_filtered`.

From there, since git filter-branch is (ideally) deterministic, the commit
hashes from previous "imports" should match up, and it should not be too
troublesome to merge the new commits into this repository.

Please make extra sure not to cause commits with hashes that were previously
set as git submodules in MoarVM's commit history to disappear.

For example, don't use `git rebase`.

Find a list of all commit hashes checked out by the dynasm submodule in the
moarvm repo with this command:

    git log --all -u -- 3rdparty/dynasm | grep '^[+-]Subproject' | cut -b 2- | sort | uniq | cut -d' ' -f 3

Here's the output from a run I just did:

    098d7251e6ee657df9f6dbfe88e34d1d23e4bcb0
    12ace5e3a50238f69a3616e65b3e9ca188c41ac5
    348ee123640393df01489173793c2f80dbda483e
    60ca088e7dd3fd0efa3690613001dd762913e03d
    7b16ea15c659f489d500a8310f5198f532a0099d
    7eaedfd5aa833374c91b5d544ded51b450f18e64
    88aab647ff739591de27159a6d70ad893a23f96a
    98a4093f3b6a05ebd41f73fdd3bb4b3caec897e9
    9a481e0397f6026152c981ffb99939398df91379
    a951a00e18a6861b97f7592c1f04d3ffef8bed38
    bdb0e057104fe255b8817dfb3e2166a269e2d3af
    be9c7a120851cb262a8ba3bb1e9bb8cb0d495efe
    d84e11714c9460bbb032de6cb1e06c3cb1c31f07
    deade9aa64b1b101d01ca69bd8e335da72e9dfc3
    e1a681416e4c4f4a8085e15f1d29b4e20b0a9739
    f29344d5c5aa8fb6d81b8c0d5477ae67cfb33b30
    f4ce3864e39aa07cb55ce437c26cd13b6de7ccd5

You can verify that the commits are available like this:

    git show -s --oneline $(cat)

and paste the block of hashes you got into the terminal, then press ctrl-d.

If one of the commits is not available, the output will look like this:

    fatal: bad object 9999999999999999999999999999999999999999

Otherwise you will get a list of short hashes and commit message titles.

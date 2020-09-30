# chef_base_plans_parent

This directory is an example [meta](https://github.com/mateodelnorte/meta#readme) 'parent' project that manages all 200+ chef base plan 'child' repositories.  All child repositories are listed in the included .meta file, ignored by the parent, and can be treated as independent git repositories.  There is no [git submodule or subtree](https://martowen.com/2016/05/01/git-submodules-vs-git-subtrees/) configuration required.

meta allows one command (or even a script) to be applied to each of the child repositories.  So, for example, ``meta git pull`` will do a git pull on each of the children and ``meta git tag -a v1.4 -m "my version 1.4"`` will apply the same annotated tag across all.

## Pre-requisites

Install [meta](https://github.com/mateodelnorte/meta#readme)

## Usage

Clone all the child repositories by running ``meta git update``, which will checkout all repositories in the structure defined in the .meta file.  See example output [below](#example-meta-clone).

Build and run inspec tests against one of the base-plan repositories.  See example inspec output [below](#example-inspec-test-result)

* cd to repos/chef-base-plans/base-plans
* enter ``hab studio enter``, which will load the ``repos/chef-base-plans/base-plans/.studiorc`` and make available some helper methods.
* cd into a base plan, e.g., zip
* Type ``plan build`` to build a fresh hab artifact or ``plan load`` to load an existing one
* Type ``plan verify`` to run inspec tests.  For example, the inspec output for zip looks something like:

Update all existing repositories:  ``meta git pull``

# Appendix

## Example meta clone

```bash
...
...
/Users/gavindidrichsen/Documents/@REFERENCE/habitat/repos/chef_base_plans_parent/repos/chef-base-plans/admin/support/examples/chef_base_plans_parent/repos/core-plans:
Cloning into 'repos/core-plans'...
remote: Enumerating objects: 38, done.
remote: Counting objects: 100% (38/38), done.
remote: Compressing objects: 100% (26/26), done.
remote: Total 29358 (delta 13), reused 24 (delta 10), pack-reused 29320
Receiving objects: 100% (29358/29358), 18.44 MiB | 2.17 MiB/s, done.
Resolving deltas: 100% (14290/14290), done.
/Users/gavindidrichsen/Documents/@REFERENCE/habitat/repos/chef_base_plans_parent/repos/chef-base-plans/admin/support/examples/chef_base_plans_parent/repos/core-plans ✓

/Users/gavindidrichsen/Documents/@REFERENCE/habitat/repos/chef_base_plans_parent/repos/chef-base-plans/admin/support/examples/chef_base_plans_parent/repos/chef-base-plans/base-plans/zlib-musl:
Cloning into 'repos/chef-base-plans/base-plans/zlib-musl'...
remote: Enumerating objects: 43, done.
remote: Counting objects: 100% (43/43), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 43 (delta 10), reused 24 (delta 6), pack-reused 0
Unpacking objects: 100% (43/43), 13.31 KiB | 290.00 KiB/s, done.
/Users/gavindidrichsen/Documents/@REFERENCE/habitat/repos/chef_base_plans_parent/repos/chef-base-plans/admin/support/examples/chef_base_plans_parent/repos/chef-base-plans/base-plans/zlib-musl ✓
...
...
```

## Example inspec test result

```bash
+ hab pkg exec chef/inspec inspec exec /src/zip/. --chef-license=accept --input-file /src/zip/attributes.yml
+---------------------------------------------+
✔ 1 product license accepted.
+---------------------------------------------+

Profile: Habitat Core Plan zip (zip)
Version: 0.1.0
Target:  local://

  ✔  core-plans-zip-exists: Ensure zip exists
     ✔  Command: `hab pkg path core/zip` exit_status is expected to eq 0
     ✔  Command: `hab pkg path core/zip` stdout is expected not to be empty
     ✔  File /hab/pkgs/core/zip/3.0/20200930145106/bin/zip is expected to exist
     ✔  File /hab/pkgs/core/zip/3.0/20200930145106/bin/zip is expected to be executable
     ✔  File /hab/pkgs/core/zip/3.0/20200930145106/bin/zipcloak is expected to exist
     ✔  File /hab/pkgs/core/zip/3.0/20200930145106/bin/zipcloak is expected to be executable
     ✔  File /hab/pkgs/core/zip/3.0/20200930145106/bin/zipnote is expected to exist
     ✔  File /hab/pkgs/core/zip/3.0/20200930145106/bin/zipnote is expected to be executable
     ✔  File /hab/pkgs/core/zip/3.0/20200930145106/bin/zipsplit is expected to exist
     ✔  File /hab/pkgs/core/zip/3.0/20200930145106/bin/zipsplit is expected to be executable
  ✔  core-plans-zip-works: Ensure zip works as expected
     ✔  Command: `hab pkg path core/zip` exit_status is expected to eq 0
     ✔  Command: `hab pkg path core/zip` stdout is expected not to be empty
     ✔  Command: `/hab/pkgs/core/zip/3.0/20200930145106/bin/zip -h` exit_status is expected to eq 0
     ✔  Command: `/hab/pkgs/core/zip/3.0/20200930145106/bin/zip -h` stdout is expected to match /Zip 3.0/
     ✔  Command: `/hab/pkgs/core/zip/3.0/20200930145106/bin/zipcloak -h` exit_status is expected to eq 0
     ✔  Command: `/hab/pkgs/core/zip/3.0/20200930145106/bin/zipcloak -h` stdout is expected to match /ZipCloak 3.0/
     ✔  Command: `/hab/pkgs/core/zip/3.0/20200930145106/bin/zipnote -h` exit_status is expected to eq 0
     ✔  Command: `/hab/pkgs/core/zip/3.0/20200930145106/bin/zipnote -h` stdout is expected to match /ZipNote 3.0/
     ✔  Command: `/hab/pkgs/core/zip/3.0/20200930145106/bin/zipsplit -h` exit_status is expected to eq 0
     ✔  Command: `/hab/pkgs/core/zip/3.0/20200930145106/bin/zipsplit -h` stdout is expected to match /ZipSplit 3.0/


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 20 successful, 0 failures, 0 skipped
```

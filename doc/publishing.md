# Publishing your projects in Alire

The process for submitting a new crate or release is currently manual, although
automated support through the `alr publish` command is in the roadmap. The
sequence of steps to take is described after some introductory concepts; you
can also ask for help in the [gitter channel](https://gitter.im/ada-lang/Alire)
of the project.

## General concepts

The community index is a collection of
[TOML](https://github.com/toml-lang/toml) files stored in the
[alire-index](https://github.com/alire-project/alire-index) repository, under
the [index](https://github.com/alire-project/alire-index/blob/master/index)
directory. Each file contains a release for a crate and is named after the
crate and version it contains. A crate contains the description of a release,
with other metadata.

The complete specification of such TOML files is available in this
[document](catalog-format-spec.md).

## New crates and releases

Publishing a new crate is achieved through a pull-request against the index
repository, in which the TOML file for the release must be provided.

### Index branches

The community index is supported through two kinds of branches:

- `stable-x.x.x` branches are used by stable versions of `alr`.
- `devel-x.x.x` branches are used to introduce breaking changes in the index
  format, during the development of `alr`.

Your `alr` version knows which branch to use, so you do not need to manually
select one. However, when submitting crates/releases as pull requests, you must
decide to which branch they will be added: selecting the latest stable branch
results in the release becoming immediately available to the latest stable
`alr`. Conversely, using the latest development branch will make the releases
available for testing by unstable clients, and will become generally available
with the next stable release of `alr`.

Note that, as of this writing, only development branches exist, until the first
stable release of `alr` is made.

## Checks on contributions

Each crate is "owned" by a list of maintainers, provided with the
`maintainers-logins` property of the crate file. After the initial submission,
which will be manually approved (see the [policies](policies.md) for details),
the maintainers of a crate are the only people allowed to submit new releases
or metadata modifications to the corresponding crate file.

## Detailed steps

These steps describe how to submit a new release. 

1. Prepare a source archive (zip, tar) with the release sources and project
   files.
    1. This archive **must not** contain the `alire` directory generated by
       `alr` in working directories. The `alire` directory is designed to be
       put in your VCS ignore list.
    1. The archive **must** contain a single directory (name not important)
       containing, in turn, the sources. This is the kind of archives
       automatically generated by GitHub, GitLab, Sourceforge... or through
       `git archive`.
1. Compute an integrity hash for the archive with `alr publish --hash <URL>`
    1. The URL can be remote or local (`file://`), although using the final
       online file is recommended.
    1. The final online file should be immutable. Note that online
       autogenerated archives can be found currently in the index, but should
       be avoided whenever possible (because of lack of immutability
       guarantees, see [this
       conversation](https://github.community/t5/How-to-use-Git-and-GitHub/Ensuring-identical-tarballs-from-releases/m-p/34196/highlight/true#M8997)).
       In the future, `alr publish` will offer support to generate and upload
       such files more conveniently.
1. Fill in the release manifest providing all the necessary information, 
   dependencies (`depends-on`) and possibly excluded platforms
   (`available`). See, for example, the [hello
   world](https://github.com/alire-project/alire-index/blob/master/index/he/hello/hello-1.0.1.toml)
   crate, or the [empty
   skeleton](https://github.com/alire-project/alire-index/blob/master/templates/skeleton.toml)
   template.
1. Place the manifest file in the appropriate directory of your locally checked
   out index repository, and create a new pull request. The detailed substeps
   are the usual for submitting contributions in GitHub:
    1. Fork the community index to your GitHub account.
    1. Clone your fork locally and make the changes locally.
    1. Commit and push the changes to your fork.
    1. Create the pull request from your fork against the community repository
       through the GitHub web interface (or the
       [`hub`](https://github.com/github/hub) tool).
        1. The base branch you select for the pull request will determine where
           your changes will become available; see the section on [index
           branches](#index-branches) for details.

Once the pull request is verified and merged, the new release will become
available for normal use. The open source Ada ecosystem needs all the help it
can get, so happy contributing!

# neve-ports

neve-ports is a collection of source package ports for Neve. The system has been
written specifically for the distribution using the Python scripting language.

From user standpoint, it works similarly to many distro packaging systems (users
of Void Linux `xbps-src` will most likely find it a little familiar) however it
is not based on any existing system and should not be considered a variant of any.

There are two authoritative documents on the system:

* [`Usage.md`](Usage.md) is the reference for users. It covers usage of `nbuild` and its
  basic and advanced options as well as concepts and requirements.
* [`Packaging.md`](Packaging.md) is the reference manual for packagers. It covers the API of the
  system and guidelines for creating and modifying templates, but not usage.

Most people looking to get involved with the project should read both.

To get started, read [`Usage.md`](Usage.md) first.

## Using neve-ports with Neve

You might want to test your built packages in an actual Neve system. Since
`nbuild` creates a regular `apk` repository for you, this is as simple as
adding the repositories in your system.

Consider path to `neve-ports` at `/home/user/neve-ports`. The default repository path
for `nbuild` is the `packages` directory directly in `neve-ports`. This is not
the actual repo yet, as there are multiple categories. The actual repositories
are those that have a directory named like your architecture (e.g. `x86_64`)
with the file `APKINDEX.tar.gz` in them.

Create a file `/etc/apk/repositories.d/00-neve-ports.list`. The file must have
the `.list` extension. Put something like this in there:

```
/home/user/neve-ports/packages/main
/home/user/neve-ports/packages/user
```

This will give `apk` access to the `main` and `user` packages of your local
repository. You might want to restrict this list to only the repositories that
you have.

If you want access to local `-dbg` packages, you will also want to add the `debug`
sub-repositories, e.g. `/home/user/neve-ports/packages/main/debug`.

You will also want to drop your signing public key in `/etc/apk/keys`. The key
can be located in `etc/keys` in the `neve-ports` directory, with the `.pub` extension
(do not put in the private key).

### Pinning the repositories

You might also want to pin the local repository. This will effectively make `apk`
prefer your pinned repository even if a newer version if available in remote
repos. This is done by adding a prefix such as `@neve-ports` before the repository
line, e.g. `@neve-ports /home/user/neve-ports/packages/main`. Then you can install things
from the repository like `apk add foo@neve-ports`. If you just `apk add foo`, the
tagged repositories will be ignored.

Note that dependencies of packages from pinned repositories will still be pulled
from unpinned repositories preferentially, but pinned repositories will be used
if necessary. This is not the case for dependencies of packages from unpinned
repositories, which will only ever be pulled from unpinned repositories.

## Bootstrapping installations from repositories

For instructions on how to bootstrap the system into a target root as well as
some more advanced tooling for e.g. creation of actual images, check out the
[chimera-live](https://github.com/chimera-linux/chimera-live) repository.

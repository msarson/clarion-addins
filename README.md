# Mark Sarson's Clarion Addins

Publisher registry for the [Clarion Addin Finder](https://github.com/msarson/ClarionAddinFinder).

`addins.json` lists the Clarion IDE addins published under the `msarson` publisher id. Addin Finder
reads it directly — this repo is the source of truth for these addins, and updating a version here
is all that a release requires.

## How this fits together

The [root registry](https://github.com/msarson/clarion-addin-registry) records **publishers**, not
addins. Each publisher keeps their own list in their own repo and owns what is in it, so a version
bump never needs a pull request against someone else's project.

```
clarion-addin-registry/registry.json     publishers: [ msarson -> clarion-addins, ... ]
                        |
                        +--> msarson/clarion-addins/addins.json      (this file)
                        +--> <other publisher>/.../addins.json
```

Addin Finder derives this file's URL from the publisher id and repo name recorded in the root
registry, so it cannot be repointed at another host after approval.

## Publishing a new version

Edit the addin's entry in `addins.json`, bump `version`, and point the download URLs at the new
release tag. That is the whole process — there is nothing to submit anywhere else.

```jsonc
{
  "id": "GitPane",                    // also the folder name under accessory\addins
  "name": "GitPane",
  "version": "1.0.9",                 // must match the release being linked
  "targetFramework": "net40",         // net40-net48; .NET 5+ will not load in Clarion
  "downloadUrls": [ "https://github.com/msarson/Clarion-GitPane/releases/download/v1.0.9/GitPane.dll" ],
  "addinFileUrl":  "https://github.com/msarson/Clarion-GitPane/releases/download/v1.0.9/GitPane.addin"
}
```

Use `downloadZipUrl` instead of `downloadUrls` where a release ships a zip.

## Two rules that are enforced, not conventions

**Download URLs must be under `github.com/msarson/`.** Addin Finder rejects entries that are not.
An approved publisher can only ship binaries from their own account.

**An addin's `<Identity name>` must be unique within a Clarion installation.** Clarion loads every
subfolder of `accessory\addins` at startup and refuses to start at all if two of them declare the
same Identity — the error is *"Identity name used by multiple addins"*. So do not publish an addin
whose Identity is already used by someone else's, and keep `id` matching the Identity. Addin Finder
checks before installing and will refuse rather than break the IDE, but the check exists to catch
mistakes, not to make duplicate names workable.

## Responsibility

These addins are mine and I am responsible for them. They run inside the Clarion IDE, in-process,
with the privileges of whoever is running it. Each is MIT licensed and, like any MIT software, comes
with no warranty. Report problems against the individual addin's repository, linked from its entry.

# HRD ERP - published releases

This repository is the update channel for HRD ERP. It holds nothing but
published builds and the manifest that every install polls.

**The source code lives in the private `hrd-erp` repository. This one is
public on purpose:** the app on a shop phone downloads the update without a
GitHub account, and it cannot authenticate to a private repository.

## How it works

`update_check.json` sits in the main branch of this repository. Every install
polls:

```
https://raw.githubusercontent.com/venomous654321/hrd-erp-releases/main/update_check.json
```

The manifest says which version is current, where to get it, and what its
SHA-256 is:

```json
{
  "latest_version": "6.0.1",
  "release_date": "2026-10-20",
  "notes": "Desktop correctness round 1.",
  "download_url": "https://github.com/venomous654321/hrd-erp-releases/releases/download/v6.0.1/HRD-ERP-6.0.1-portable.zip",
  "sha256": "…",
  "size_bytes": 71613833,
  "mandatory": false
}
```

An install compares `latest_version` with its own. If it is newer, the app
shows an update badge in the top bar, and the release can be downloaded and
installed from there.

### Why the manifest is not on the release

GitHub's CDN caches release assets for a long time. A manifest that sits on a
release can still be served from the cache after it has been replaced, and the
app would not see the new version. A file in the main branch has ordinary
cache rules, so a new release is picked up within minutes.

## Publishing a release

From the source repository:

```powershell
.\make_release.ps1 -Version 6.2.0 -Notes "What changed"
```

That builds, runs the tests, packs the portable build, writes the manifest,
commits and tags the source, publishes the build as a release here, and
updates `update_check.json` in this repository.

## The two repositories

| Repository | Visibility | Holds |
|---|---|---|
| `hrd-erp` | private | the source code |
| `hrd-erp-releases` | public | the built releases and the manifest |

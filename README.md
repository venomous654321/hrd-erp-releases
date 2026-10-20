# HRD ERP - published releases

This repository is the update channel for HRD ERP. It holds nothing but
published builds and the manifest that every install polls.

**The source code lives in the private `hrd-erp` repository. This one is
public on purpose:** the app on a shop phone downloads the update without a
GitHub account, and it cannot authenticate to a private repository.

## How it works

`update_check.json` sits on the newest release. Every install polls:

```
https://github.com/venomous654321/hrd-erp-releases/releases/latest/download/update_check.json
```

The manifest says which version is current, where to get it, and what its
SHA-256 is:

```json
{
  "latest_version": "6.0.1",
  "release_date": "2026-10-20",
  "notes": "Desktop correctness round 1.",
  "download_url": "https://github.com/venomous654321/hrd-erp-releases/releases/download/v6.0.1/HRD-ERP-6.0.1-portable.zip",
  "sha256": "â€¦",
  "size_bytes": 71500000,
  "mandatory": false
}
```

An install compares `latest_version` with its own. If it is newer, the app
shows an update badge in the top bar, and the release can be downloaded and
installed from there.

## Publishing a release

From the source repository:

```powershell
.\make_release.ps1 -Version 6.2.0 -Notes "What changed"
```

That builds, tests, packs the portable build, writes the manifest, commits and
tags the source, and publishes both files here as a new release.

# Repository Notes

## Nightly Manifest Process

- For `*-nightly.json` manifests in this bucket, treat GitHub prereleases from the upstream repo's `dev` branch as the source of truth.
- Prefer the GitHub Releases API over `/releases/latest` when the upstream only publishes prereleases, because `latest` can return `404`.
- Use `https://api.github.com/repos/<owner>/<repo>/releases` and verify that the selected release actually has Windows assets before updating the manifest.
- For nightly manifests, make `checkver.regex` match only `-dev-` style versions and require at least one expected Windows asset name in the same release payload. This avoids updating to a prerelease entry that exists before its assets finish uploading.
- Inspect one release archive before finalizing the manifest so the `bin` field matches the real packaged executable name.
- For GitHub release assets, keep `autoupdate` URLs pointed at the release download URLs. Scoop can resolve updated hashes from the GitHub release asset digests during `checkhashes`.
- After editing a manifest, run `.\bin\checkver.ps1 <manifest-name>` and `.\bin\checkhashes.ps1 <manifest-name>`.

## CI Workflow Process

- Keep `.github/workflows/*.yml` aligned with the current `ScoopInstaller/BucketTemplate` workflows.
- When comparing with `BucketTemplate`, use the raw GitHub files or GitHub contents API if cloning is unavailable.
- Prefer the pinned `actions/checkout` SHA and the current `potatoqualitee/psmodulecache` SHA from `BucketTemplate`; do not keep `@main` or old cache action versions in this repo.

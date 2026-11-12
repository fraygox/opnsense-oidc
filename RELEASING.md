# Releasing

A release is a tag `vX.Y.Z` on `main`. Pushing the tag runs
`.github/workflows/release.yml`, which:

1. checks that the tag equals `PLUGIN_VERSION` in the `Makefile` and that
   `CHANGELOG.md` has a section for that version,
2. runs the offline ID-token tests (`test/README.md`),
3. builds `os-oidc-devel-X.Y.Z.pkg` in a FreeBSD VM against the current
   `opnsense/plugins` tree,
4. publishes a GitHub release with the package, a `SHA256SUMS` file and the
   changelog section as the release notes.

If any step fails, nothing is published. Fix the problem on `main`, move the
tag (`git tag -f vX.Y.Z && git push -f origin vX.Y.Z`) and the workflow runs
again.

## Steps

1. Set `PLUGIN_VERSION` in the `Makefile`. `pkg` only upgrades to a higher
   version, so it must be higher than every version already installed
   anywhere, including packages that were patched in place.
2. Add a `## X.Y.Z` section at the top of `CHANGELOG.md`.
3. Commit both to `main`.
4. `git tag vX.Y.Z && git push origin main vX.Y.Z`

## Installing a release

On the firewall, as root:

```sh
fetch https://github.com/fraygox/opnsense-oidc/releases/download/vX.Y.Z/os-oidc-devel-X.Y.Z.pkg
sha256 os-oidc-devel-X.Y.Z.pkg      # compare with SHA256SUMS on the release page
pkg unlock -y os-oidc-devel         # only if the package was locked
pkg add -f os-oidc-devel-X.Y.Z.pkg
```

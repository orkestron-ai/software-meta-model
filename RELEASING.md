# Releasing AISMM

This is the maintainer checklist for cutting an AISMM release. AISMM follows
semantic versioning as defined in
[`aismm-versioning-and-conformance.md`](./aismm-versioning-and-conformance.md):
**MAJOR** for breaking changes, **MINOR** for additive changes, **PATCH** for
editorial/fix-only releases.

## Branch model

- `main` carries the current major line (currently **v3**).
- Each released major is preserved as a branch (e.g. `v2.0.0`) and a tag for
  historical reference.

## Release checklist

1. **Confirm `main` is green.** All CI checks pass on the latest commit.
2. **Finalize the changelog.** Move everything under `## [Unreleased]` in
   [`CHANGELOG.md`](./CHANGELOG.md) into a new version section with the release
   date. Keep the `Added / Changed / Deprecated / Removed / Fixed / Security`
   grouping.
3. **Bump the version** everywhere it appears:
   - `README.md` ("Current version")
   - `CITATION.cff` (`version` and `date-released`)
   - `aismm-versioning-and-conformance.md`
4. **Verify the layer inventory** in
   [`aismm-layer-inventory.md`](./aismm-layer-inventory.md) matches the actual
   layer count and README claims.
5. **Run the consistency checks** described in
   [`aismm-consistency-checks.md`](./aismm-consistency-checks.md).
6. **Commit** the release prep: `Release vX.Y.Z`.
7. **Tag and push:**

   ```bash
   git tag -a vX.Y.Z -m "AISMM vX.Y.Z"
   git push origin main --tags
   ```

8. **Create the GitHub Release** from the tag, pasting the new
   `CHANGELOG.md` section as the release notes. With the GitHub CLI:

   ```bash
   gh release create vX.Y.Z \
     --title "AISMM vX.Y.Z — <headline>" \
     --notes-file <(sed -n '/## AISMM X.Y.Z/,/## AISMM /p' CHANGELOG.md)
   ```

   (Or create it through the web UI: **Releases → Draft a new release →**
   choose the tag → paste notes.)
9. **For a MAJOR release**, preserve the previous major as a branch before it
   diverges:

   ```bash
   git branch vPREV.0.0 <commit-of-last-prev-release>
   git push origin vPREV.0.0
   ```

10. **Announce** in Discussions and update any downstream references.

## Pre-1.0 / pre-release tags

For previews use a suffix: `vX.Y.Z-rc.1`, `vX.Y.Z-beta.1`. Mark these as
"pre-release" on GitHub so they do not show up as the latest stable release.

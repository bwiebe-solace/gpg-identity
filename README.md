# GPG Identity — Benjamin Wiebe

This repository is the canonical source for my **work GPG key** (Solace). It
serves as a distribution point and a place for others to contribute signatures,
building a verifiable web of trust without relying on keyserver infrastructure.

Note: I maintain a separate key for personal communications. The two keys are
cross-signed with each other, so either can serve as a trust anchor for the
other.

## Key Details

| Field       | Value                                                       |
|-------------|-------------------------------------------------------------|
| Name        | Benjamin Wiebe (Solace Employee Key)                        |
| Email       | benjamin.wiebe@solace.com                                   |
| Key type    | Ed25519 / Curve25519                                        |
| Fingerprint | `1574 6FFA 864B 0DCB AE8D  4B0D ADEF C33E 229A 5A37`       |

## Retrieving the Key

Import directly from this repository:

```bash
curl -sL https://raw.githubusercontent.com/bwiebe-solace/gpg-identity/main/pubkey.asc | gpg --import
```

Or clone the repo and import locally:

```bash
gpg --import pubkey.asc
```

## Verifying the Fingerprint

After importing, verify the fingerprint matches exactly what is shown above:

```bash
gpg --fingerprint benjamin.wiebe@solace.com
```

Do not trust a key that does not produce this exact fingerprint. If in doubt,
verify out-of-band (in person, via a signed message, or through another
trusted channel).

## Contributing a Signature

If you have verified my identity and would like to add your signature to my
key, start by signing and exporting the updated key locally:

```bash
# 1. Import the current key from this repo
gpg --import pubkey.asc

# 2. Verify the fingerprint matches before signing (critical)
gpg --fingerprint benjamin.wiebe@solace.com

# 3. Sign the key with your own key
gpg --sign-key 15746FFA864B0DCBAE8D4B0DADEFC33E229A5A37

# 4. Export the updated key (now includes your signature)
gpg --armor --export 15746FFA864B0DCBAE8D4B0DADEFC33E229A5A37 > pubkey.asc
```

Then submit the updated `pubkey.asc` using one of the two methods below.

### Option 1 — Submit via GitHub issue (preferred)

Open a **[Submit Signature](../../issues/new?template=submit-signature.yml)**
issue, paste the contents of `pubkey.asc` into the form, and submit. A pull
request will be opened automatically on your behalf — no fork required.

### Option 2 — Open a pull request directly

Fork this repository, commit the updated `pubkey.asc` to your fork, and open a
pull request. The PR description template will guide you through the checklist.

---

PRs are merged manually by me on my local machine so that I can review the
incoming signature and update my own keyring at the same time.

## Maintainer: Merging a Signature PR

PRs are intentionally not merged via the GitHub UI. The steps below allow
validating the signature and updating the local keyring atomically.

```bash
# Fetch the PR branch without checking it out
git fetch origin pull/<PR_NUMBER>/head:pr-<PR_NUMBER>

# Inspect the diff — it must only modify pubkey.asc
git diff main pr-<PR_NUMBER>

# Import the key from the branch to update the local keyring and review sigs
git show pr-<PR_NUMBER>:pubkey.asc | gpg --import
gpg --list-sigs 15746FFA864B0DCBAE8D4B0DADEFC33E229A5A37

# If the signature looks legitimate, squash-merge and commit
# (squash is required — the bot's commit on the PR branch is unsigned)
git merge --squash pr-<PR_NUMBER>
git commit -m "merge: Accept signature from <SIGNER>"
git push origin main

# Clean up
git branch -d pr-<PR_NUMBER>
```

If the signature is not legitimate or the PR modifies anything other than
`pubkey.asc`, close the PR without merging. To remove a bad signature from the
local keyring:

```bash
gpg --edit-key 15746FFA864B0DCBAE8D4B0DADEFC33E229A5A37
# At the gpg> prompt: uid 1 (select the uid), then: delsig
```

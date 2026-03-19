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

PRs are intentionally not merged via the GitHub UI. Use the included script
to review, accept, or reject a signature PR interactively:

```bash
bash accept-signature.sh
```

The script will list open signature PRs, fetch the branch, import the
signer's public key from the issue (if provided), show the current signatures
on the key, verify them if possible, and prompt to accept or reject.

If the signer did not include their public key in the issue, their signature
can only be verified out-of-band. Only accept signatures from people whose
identity you can confirm.

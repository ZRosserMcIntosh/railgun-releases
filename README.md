# railgun-releases
Signed desktop installers for RAILGUN — macOS, Windows, and Linux. Binaries only; source is not published here.

Use of these files is governed by the [Railgun Desktop End-User License](LICENSE).

## Verifying a download

Each release published from November 2026 on carries `SHA256SUMS.txt` and `SHA256SUMS.txt.sig`,
an Ed25519 signature over the checksums made with Railgun's update signing key —
the same key the desktop app checks automatic updates against. The checksums
on their own only prove a file matches the release it came from; the signature
proves the release is Railgun's.

1. Get the public key, `railgun-update.pem`, from this repository (it is
   published here when the signing key is created, and is the same key built
   into the app as `UPDATE_PUBLIC_KEYS`).
2. Check the signature (OpenSSL 3):

   ```sh
   openssl pkeyutl -verify -pubin -inkey railgun-update.pem -rawin \
     -in SHA256SUMS.txt -sigfile SHA256SUMS.txt.sig
   ```

   It must print `Signature Verified Successfully`.
3. Check your installer against the signed list:

   ```sh
   sha256sum --check --ignore-missing SHA256SUMS.txt     # Linux
   shasum -a 256 --check --ignore-missing SHA256SUMS.txt # macOS
   ```

macOS installers are also signed and notarized by Apple, and Windows installers
are Authenticode-signed; the release pipeline refuses to publish a non-draft
release that is not.

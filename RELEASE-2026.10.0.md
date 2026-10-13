# 2026.10.0 Release Notes

2026.10.0 is a minor release that fixes Argon2i and Argon2id compatibility with RFC 9106, completes Ed25519 small-order key validation, and improves secret handling and failure recovery in ML-DSA and ML-KEM.

* **Argon2i and Argon2id** - Fix incorrect address-block indexing that produced non-standard hashes for affected parameter sets since 2025.11.0. Argon2d is unaffected. `ARGON2I_LEGACY` and `ARGON2ID_LEGACY` preserve the previous behaviour for migration. Reference vectors are now included in the default test suites (#114, #115).
* **Ed25519** - Reject all small-order public keys during validation, fixing an incomplete check from 2026.5.2. Public-key validation at level 2 or higher and private-key validation now use the Ed25519 small-order values. Signature verification and X25519 are unchanged (#112).
* **ML-DSA and ML-KEM** - Ensure secret stack buffers are wiped on normal exits and exceptions, and clear additional sensitive intermediates (#113).
* **ML-DSA** - Failed key generation or import now leaves existing keys unchanged and newly created keys unset, preventing inconsistent key state and exposure of uninitialised data (#110).
* **LM-OTS** - Bound hash lengths before buffer sizing to fix GCC `-Wstringop-overflow` warnings on ARM-32 builds with `-Werror` (#105, #106).
* **Apple builds** - Restrict the machine-info helper to PowerPC, fixing `-Werror=unused-function` failures on other Apple targets (#107).

**Argon2 upgrade note:** Argon2i and Argon2id hashes and derived keys produced by versions 2025.11.0 through 2026.9.1 may differ from RFC 9106-compliant implementations. Existing password hashes should be verified using the corresponding `LEGACY` variant and rehashed using the standard variant after successful verification. Encrypted data using affected derived keys must be decrypted using the legacy variant and re-encrypted using the corrected one. The legacy variants are intended for migration only. Argon2d is unchanged.

**Compatibility:** This release adds two legacy Argon2 variants without other public API or ABI changes. Ed25519 small-order public keys previously accepted at validation level 2 or higher are now rejected. The shared-library SONAME remains `libcryptopp.so.9`.

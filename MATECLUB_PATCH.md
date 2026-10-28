# Mate Club receipt synchronization guard

Based on official purchases-ios 5.99.0 (`338022faafce4675788e405fbcaa4246aae2c93b`). The suite owns receipt synchronization and selects its authenticated billing owner before an explicit purchase, sync or restore. Preserve the existing Mate Club fork behavior while updating the SDK: when `autoSyncPurchases` is false, do not start the StoreKit transaction-updates listener or foreground observer detection. Keep the transaction delegate and explicit purchase/sync/restore APIs. Public API and reusable deployment targets are unchanged.

Local validation on 28 October 2026 used Xcode 27.2 (27B48), iOS Simulator 27.2 (24B114), ARM64. The original StoreKit initialization preflight passed 1/1, followed by a separate-process run of the six new behavioral cases plus the existing automatic-listener control: 7/7 passed, zero skipped. These use the real local SKTestSession and mock receipt backend; no real purchases or customer accounts. The full SDK/API tester suite was not run.

SwiftLint ran strictly on the three changed Swift files. It exits 2 for 46 existing violations, including file length; comparison found zero violations on changed or added lines. This is not a clean repository-wide lint result.

The existing private purchase-intent delegate helper also retains the guard, but has no current caller; the registration test covers the reachable API, not execution of that private helper. Native app builds separately validate the SDK product. Native publishing and dependency pushes remain on hold for the owner's local review.

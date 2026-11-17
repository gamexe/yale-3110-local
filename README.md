# Yale Lock Bluefy controller

Static, offline-capable Web Bluetooth controller for the owner's registered
Yale lock. Open the GitHub Pages URL in Bluefy on iOS. This repository must
never contain `credentials.json`, captures, certificates, or prefilled lock
identifiers. Enter credentials only on the private phone.


## Remembered Bluetooth and automatic unlock

Select the lock once in Bluefy using Unlock. The device ID is remembered in
sessionStorage and localStorage; the browser retains Bluetooth permission.
On opening or returning to the visible page, the controller restores exactly
that device with getDevices(), connects without a chooser, and retries
out-of-range failures with backoff (up to 30 seconds). Browsers without
getDevices() need a user tap after reload; saved JSON cannot recreate a device.

Enable “Unlock automatically when this page opens” once to request one unlock
per future opening. The request waits for the saved lock to connect; it is
consumed before attempting authentication and is never retried automatically.
Automatic attempts within one minute of the previous attempt are skipped.
The separate “Unlock on next connection” button arms one attempt for the
current page. Cancel stops the current pending attempt; disable the checkbox
to stop automatic unlock on future openings.

Keep Bluefy foregrounded. BLE connection is not proof of arrival. This is not
background iPhone or geographic arrival automation. Reconnect alone never
unlocks, unless an automatic attempt was explicitly enabled or armed.
Fresh GATT characteristics and cryptographic session keys are used per unlock.
No credentials or authenticated Bluetooth sessions are serialized.

Validation: JavaScript syntax and mocked connection lifecycle checks cover
restoration, missing permissions, missing getDevices, explicit selection,
one-shot automatic unlock, hidden pages, concurrent connection guards,
invalid credentials cleanup, and out-of-range retries. Physical lock/Bluefy
acceptance remains required with the door open.

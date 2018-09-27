# Firmware change log

## Upgrading

> NOTE: The Upgrader tool only currently supports Windows at this time.

1. Download the LightWare Upgrader tool here: http://support.lightware.co.za/LightWareUpgrader-1.12.0.rar
2. Unzip the downloaded file to a location on your PC.
3. Run the file `LightWareUpgrader.exe` in the unzipped folder.
4. Connect your LightWare device via USB to your PC.
5. Click the COM port that appears.
6. If the device is not the latest version you can click the `Upgrade` button to begin the process.
7. Wait until the upgrade has completed successfully and click `OK`.

## 1.1.1

*Fixes*
- Version `1.1.0` had the LWNX protocol disabled by default. It is now active by default.

## 1.1.0

*Features*
- Modified `Distance [105]` command to accept a minimum range parameter.
- Modified `Distance [105]` command to output angle to closest distance measurement.
- Less aggressive power draw on the 5V line during start-up.
- Supports Pixhawk proximity detection when disabling LWNX mode.

*Fixes*
- The `Point start index` field of the `Distance output [48]` command now correctly takes the `Output rate [108]` into account.

## 1.0.1

*Notes*
- Initial release.
# Changelog

## [1.0.0-beta.1] - 2023-06-29
The latest release, version **1.0.0-beta.1**, enhances the authentication functionalities while also introducing two new features: Device Location and Support Archive.

#### Enhanced Authentication
A novel method is unveiled within this iteration, affording users the ability to institute a new password for their miners.

#### Device Location Functionality
The Locate Device feature debuts, offering users the capability to activate or deactivate LEDs on the device. This serves as a visual aid, allowing the client to precisely identify the targeted device.

#### Introduction of Support Archive
A new API has emerged, facilitating streamlined access to a comprehensive support archive. This archive acts as a valuable repository, housing crucial information for troubleshooting and problem resolution.

### Added
* Introduction of a new `braiins.bos.v1.AuthenticationService::SetPassword()` method that allows the user to set a new password on the miner,
* Introduction of a new `braiins.bos.v1.ActionsService::SetLocateDeviceStatus()` and `braiins.bos.v1.ActionsService::GetLocateDeviceStatus()` methods to turn on/off device location function,
* Introduction of a new `braiins.bos.v1.MinerService::GetSupportArchive()` streaming method that allows to download BOS support archive.
* Extension of the `braiins.bos.v1.GetMinerDetailsResponse` message with `sticker_hashrate` field that contains HR declared by miner vendor,

**Important:** This iteration bears resemblance to **1.0.0-alpha.1**, yet it is grounded in a licenses-integrated branch and encompasses a broader array of features.

---

## [1.0.0-beta] - 2023-05-11
The latest release, version 1.0.0-beta, introduces a significant addition: the all-new BOS+ License feature.

### Added
* Introduction of a new `braiins.bos.v1.LicenseService::GetLicenseState()` streaming method to fetch BOS Licence state.

**Important:** This version remained in a private state for a duration, attributed to the testing phase of the novel licenses approach.

---

## [1.0.0-alpha] - 2023-05-25
The first release for the new Braiins OS+ Public API, which introduces the first batch of features.

##### Actions
With actions from `ActionService`, user can start/stop/restart/pause/resume mining (BOS+). Reboot of the whole miner is supported as well.

##### Configuration
With `ConfigurationService` methods, user can read current miner configuration and configuration constraints.

##### Cooling
With `CoolingService`methods, user can read current state of fans or current temperatures, or configure immersion mode.

##### Miner
With `MinerService` methods, user can read miner HW details, overall miner or hashboards statistics.

##### Pools
With `PoolService` methods, user can read all currently used pool groups, as well as update a specific pool group.

##### Tuner
With `TunerService` methods, user can read current tuner state and configure tuner power target.

##### Other
Overall to use gRPC API, user must be authenticated. For this purpose, `AuthenticationService` with `Login` method is present.

The method in `VersionService` provides information about a particular version of the public Braiins OS+ API.

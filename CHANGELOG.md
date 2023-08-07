# Changelog

## [1.0.0-alpha.1] - 2023-06-26

New version 1.0.0-alpha.1 extends authentication options and introduces new features, **Locate Device** and **Support Archive**.
##### Authentication options
We introduce a new method that allows user to set new password on miner.

##### Locate Device
Locate device is a feature where user can turn on/off a LED on the device and client is able to visually detect, which one is it.

##### Support Archive
We introduce a new API that provides simple way how to download a support archive that contains useful information for problem investigation.

### Added
* Introduction of a new `braiins.bos.v1.AuthenticationService::SetPassword()` method that allows user to set a new password on the miner,
* Introduction of new `braiins.bos.v1.ActionsService::SetLocateDeviceStatus()` and `braiins.bos.v1.ActionsService::GetLocateDeviceStatus()` methods to turn on/off device location function,
* Introduction of a new `braiins.bos.v1.MinerService::GetSupportArchive()` streaming method that allows to download BOS support archive.
* Extension of the `braiins.bos.v1.GetMinerDetailsResponse` message with a `sticker_hashrate` field that contains HR declared by miner vendor.

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

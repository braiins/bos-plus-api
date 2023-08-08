# Changelog

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

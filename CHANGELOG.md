# Changelog

## [1.9.0] - yyyy-mm-dd
### Added
* Added `on_start_target_percent` field in the `braiins.bos.v1.DPSConfiguration`, `braiins.bos.v1.DPSConstraints`, `braiins.bos.v1.SetDPSRequest`, `braiins.bos.v1.SetDPSResponse`. This change allows to specify the target that the Dynamic Performance Scaling starts from.
* Introduced new `braiins.bos.v1.ActionsService::FactoryReset()` method to perform factory reset.

## [1.8.0] - 2025-11-25
Version **1.8.0** introduces new Upgrade service `braiins.bos.v1.UpgradeService` and relative power/hashrate target configuration.

### Added
* Introduced new field `uid` in the `braiins.bos.v1.PoolGroup` message to get group uid.
* Introduced new service `braiins.bos.v1.UpgradeService` with methods:
  * `UpdateAutoUpgradeConfig` - enables/disables AutoUpgrade feature and configures upgrade schedule
  * `GetAutoUpgradeStatus` - retrieves current AutoUpgrade configuration and execution status
* Introduced new messages for AutoUpgrade scheduling:
  * `units.DayOfWeek` enum and custom `upgrade.UpgradeTime` message for schedule configuration
  * `DailySchedule` - schedule upgrades to run daily at a specific time
  * `WeeklySchedule` - schedule upgrades to run weekly on a specific day of the week at a specific time
  * `MonthlySchedule` - schedule upgrades to run monthly on a specific day of the month (1-28) at a specific time
  * `AutoUpgradeSchedule` - oneof message that can contain any of the schedule types
  * `UpdateAutoUpgradeConfigRequest` - request message to update AutoUpgrade configuration
  * `UpdateAutoUpgradeConfigResponse` - response message containing enabled status and next execution timestamp
  * `GetAutoUpgradeStatusRequest` - request message to get AutoUpgrade status
  * `GetAutoUpgradeStatusResponse` - response message containing enabled status, schedule, next execution, and last execution timestamps
* Introduced new enumeration `RelativeTargetReference` with `NOMINAL`, `MIN`, `MAX` and `CURRENT` variants.
* Introduced new message `SetRelativeTargetRequest` with `save_action`, `percentage` and `reference` fields.
* Introduced new methods `SetRelativePowerTarget` and `SetRelativeHashrateTarget`. User can now set power and hashrate targets relative to a reference value

## [1.7.0] - 2025-08-10
Version **1.7.0** introduces changes to `braiins.bos.v1.PoolGroupConfiguration` and `braiins.bos.v1.PoolConfiguration`.

### Added
* Introduced new enumeration `braiins.bos.v1.FanPauseMode` with `FAN_PAUSE_MODE_AUTO` and `FAN_PAUSE_MODE_MANUAL` variants.
* Introduced new field `default_fan_pause_mode` in the `braiins.bos.v1.CoolingConstraints` message to get default fan pause mode.
* Introduced new messages `braiins.bos.v1.SetQuickRampingRequest`, `braiins.bos.v1.QuickRampingResponse` and `braiins.bos.v1.SetDefaultQuickRampingRequest`.
* Introduced new method `SetQuickRamping` in the `braiins.bos.v1.PerformanceService`. User can now set quick ramping time up and down values.
* Introduced new method `SetDefaultQuickRamping` in the `braiins.bos.v1.PerformanceService`. User can now set quick ramping time up and down to default.
* Introduced new fields `quick_ramping_time_up_s` and `quick_ramping_time_down_s` in the `braiins.bos.v1.HashboardPerformanceConfiguration` message to get quick ramping time up and down values.
* Introduced new field `quick_ramping_time_s` in the `braiins.bos.v1.HashboardConstraints` message to get quick ramping time constraints.

### Changed
* Mark `uid` for `braiins.bos.v1.PoolGroupConfiguration` and `braiins.bos.v1.PoolConfiguration` as optional. This change allows to create new pool group or pool without providing empty `uid` value. When new entity is created `uid` is generated automatically. When updating existing Pool Group with `UpdatePoolGroup` method, `uid` is required, otherwise error is returned back to the user.

## [1.6.0] - 2025-06-10
Version **1.6.0** introduces enhanced control of fan behavior during cooling mode curtailment and adds ability to apply Custom Contracts.

### Added
* Introduced new messages `PauseMode`, `AutoPauseMode` and `ManualPauseMode`.
* Introduced new field `pause_mode` in the `braiins.bos.v1.CoolingAutoMode` message to set and get pause cooldown fan speed for automatic cooling mode.
* Introduced new field `pause_mode` in the `braiins.bos.v1.CoolingManualMode` message to set and get pause cooldown fan speed for manual cooling mode.
* Introduced new field `pause_cooldown_fan_speed_ratio` in the `braiins.bos.v1.CoolingConstraints` message to get pause cooldown fan speed constraints.
* Introduced new method `ApplyContractKey` in the `braiins.bos.v1.LicenseService`. User can now apply contract key to miner and get licence.

## [1.5.0] - 2025-04-09
Version **1.5.0** extends Hashboard message to provide lowest inlet and highest outlet temperatures. And HW info including miner serial number from stock FW.

### Added
* Introduced new fields `lowest_inlet_temp`, `highest_outlet_temp` in the `braiins.bos.v1.Hashboard` message to get the lowest inlet and highest outlet temperature for a specific hashboard.
* Introduced new fields `serial_number`, `board_name` and `chip_type` in the `braiins.bos.v1.Hashboard` message.
* Introduced new `PsuInfo` message and added it to the `braiins.bos.v1.GetMinerDetailsResponse` message.
* Introduced new `ControlBoardSocFamily` enumeration and added it to the `braiins.bos.v1.GetMinerDetailsResponse` message.
* Introduced new field `serial_number` in the `braiins.bos.v1.GetMinerDetailsResponse` message.

## [1.4.0] - 2025-02-26
Version **1.4.0** introduces possibility to set DPS mode to `normal` or `boost`. It also extends cooling configuration with new values `immersion` and `hydro`.

### Added
* Introduced new enumeration `braiins.bos.v1.DPSMode` with `DPS_MODE_NORMAL` and `DPS_MODE_BOOST` variants.
* Introduced new field `mode` in the `braiins.bos.v1.DPSConfiguration`, `braiins.bos.v1.SetDPSRequest`, `braiins.bos.v1.SetDPSResponse` message to get or set DPS mode.
* Introduced new field `mode` in the `braiins.bos.v1.DPSConstraints` message to get the default value for DPS mode.
* Introduced new method `SetCoolingMode` in the `braiins.bos.v1.CoolingService`. User can now set cooling mode to `automatic`, `manual`, `hydro` or `immersion`. It gives user possibility to set specific temperature and fan settings for each mode.
* Introduced new field `min_fan_speed` in the `braiins.bos.v1.CoolingAutoMode` to set minimum fan speed for automatic cooling mode.
* Introduced new field `max_fan_speed` in the `braiins.bos.v1.CoolingAutoMode` to set maximum fan speed for automatic cooling mode.
* Introduced new field `min_fan_speed` in the `braiins.bos.v1.CoolingConstraints` to get default value for minimum fan speed for automatic cooling mode.
* Introduced new field `max_fan_speed` in the `braiins.bos.v1.CoolingConstraints` to get default value for maximum fan speed for automatic cooling mode.
* Introduced new field `target_temperature` in the `braiins.bos.v1.CoolingManualMode` to set target temperature for manual cooling mode.

### Changed
* Mark `SetImmersionMode` method as deprecated in `CoolingService`. Instead of this method, user should use `SetCoolingMode` with `immersion` mode.
* Extend `CoolingConfiguration` mode with new value `immersion` that represents immersion cooling mode.
* Extend `CoolingConfiguration` mode with new value `hydro` that represents hydro cooling mode.
* Mark `disabled` cooling mode as deprecated in `CoolingConfiguration` mode.
* Mark `minimum_required_fans` field as deprecated in `CoolingConfiguration`. Instead user should use `minimum_required_fans` field in `CoolingAutoMode` or `CoolingManualMode`.
* Extend `CoolingAutoMode` with new field `minimum_required_fans`.
* Extend `CoolingManualMode` with new field `minimum_required_fans`.

## [1.3.0] - 2024-10-22
Version **1.3.0** introduces few small improvements.

### Changed
* Extended `braiins.bos.v1.SupportArchiveFormat` enumeration with `SUPPORT_ARCHIVE_FORMAT_ZIP_ENCRYPTED` variant,
* Extended `braiins.bos.v1.Platform` enumeration with `PLATFORM_STM32MP157C_II2_BMM1` variant.

## [1.2.0] - 2024-07-17
Version **1.2.0** introduces the possibility to configure all pool groups at once and read Braiins OS errors.

### Added
* Introduced new method `SetPoolGroups` in the `braiins.bos.v1.PoolService` to set all Pool groups at once,
* Introduced new method `GetErrors` in the `braiins.bos.v1.MinerService` to get all miner errors,
* Introduced new field `model` in the `braiins.bos.v1.Hashboard` message that contains hashboard name.

## [1.1.0] - 2024-05-09
Version **1.1.0** introduce possibility to read network configuration, changes in authentication and few more changes.

### Added
* Introduced new field `last_share_time` in the `braiins.bos.v1.PoolStats` that provides info about last share time,
* Introduced new field `token` in the `braiins.bos.v1.LoginResponse` that provides info created authentication token that was till now available only in response header,
* Introduced new field `timeout_s` in the `braiins.bos.v1.LoginResponse` that provides info about authentication token expiration time,
* Introduced new method `GetNetworkInfo` in the `braiins.bos.v1.NetworkService` to get current network configuration for the default network interface,
* Introduced new field `kernel_version` in the `braiins.bos.v1.GetMinerDetailsResponse` that provides info about Kernel version.

## [1.0.0] - 2024-03-26
The first stable release of the Public API incorporates minor enhancements.

### Added
* Introduced new field `enabled` in the `braiins.bos.v1.DPSConfiguration` that provide info, if DPS is enabled by default or not.
* Introduced new field `enabled` in the `braiins.bos.v1.TunerConstraints` that provide info, if DPS is enabled by default or not.
* Introduced new field `default_mode` in the `braiins.bos.v1.TunerConstraints` that provide info about default tuner mode.
* Introduced new field `status` in the `braiins.bos.v1.GetMinerDetailsResponse` that provide info about current miner status.

## [1.0.0-beta.6] - 2024-03-05
Version **1.0.0-beta.6** contains one new feature Network configuration.

### Added
* New `braiins.bos.v1.NetworkService` with `GetNetworkConfiguration` and `SetNetworkConfiguration` methods

## [1.0.0-beta.5] - 2023-12-14
Version **1.0.0-beta.5** contains one small extension.

### Added
* Extension of the `braiins.bos.v1.Platform` enumeration with new value for Zynq.

## [1.0.0-beta.4] - 2023-11-23
Version 1.0.0-beta.4 contains one new feature and one breaking change.

### Added
* Introduced new field `system_uptime_s` in the `braiins.bos.v1.GetMinerDetailsResponse` that replace `system_uptime`(marked as deprecated) to keep best practise that field name should describe also unit.
* We added option to clean tuner profiles by adding `braiins.bos.v1.PerformanceService::RemoveTunedProfiles`

### Breaking Changes:
* We reverted removing `braiins.bos.v1.MinerModel` enumeration from previous release because this change cause troubles to our users. 
  Instead of dropping enumeration, we decided to mark it as deprecated and introduce new field `miner_model` for string representation.

## [1.0.0-beta.3] - 2023-10-30
Version 1.0.0-beta.3 contains a few minor improvements.

### Added
* Extension of the `braiins.bos.v1.GetMinerDetailsResponse` message with `bosminer_uptime_s` field that contains bosminer uptime,

### Breaking Changes:
* We removed `braiins.bos.v1.MinerModel` enumeration and changed type of `model` field in `braiins.bos.v1.MinerIdentity` to `string`. Replacing model enumeration with string eliminates the need to release a new version every time we add support for new model.
---

## [1.0.0-beta.2] - 2023-08-10
Version **1.0.0-beta.2** extends performance management and Pool Group management possibilities. It also introduces an API to get Mining Status.

#### Enhanced Performance Options
We have extended the alternatives for performance tuning in this release.

##### Hashrate Target Introduction
Formerly, users could only configure power target values. Now, we made it possible to define hashrate targets as well.

##### Dynamic Performance Scaling
This release adds the possibility to configure Dynamic Performance Scaling parameters. Users now can fully optimize their operations to their unique requirements using Public API.

##### Augmented Hashboard Administration
We have extended the hashboards management. Public API now lets users to activate or deactivate the individual hashboards.

#### Evolved Pool Group Management
Previously only single pool group modifications were possible. We have reworked the Public API to allow addition of new pool groups and removal of existing ones.

#### Introduction of Mining Status
An entirely new API makes its debut, providing concise insights into the mining status of individual devices. The mining status allows users to readily differentiate between paused operations and miners requiring attention or possible repair.

### Added
* Introduction of a new `braiins.bos.v1.PerformanceService::SetDefaultHashrateTarget()` method that allows the user to set the currently configured hashrate target value to default one,
* Introduction of a new `braiins.bos.v1.PerformanceService::SetHashrateTarget()` method that allows the user to set the currently configured hashrate target value to a specific one,
* Introduction of a new `braiins.bos.v1.PerformanceService::IncrementHashrateTarget()` method that allows the user to increment the currently configured hashrate target value,
* Introduction of a new `braiins.bos.v1.PerformanceService::DecrementHashrateTarget()` method that allows the user to decrement currently configured hashrate target value,
* Introduction of a new `braiins.bos.v1.MinerService::EnableHashboards()` method that allows user to enable hashboards,
* Introduction of a new `braiins.bos.v1.MinerService::DisableHashboards()` method that allows user to enable hashboards,
* Introduction of a new `braiins.bos.v1.PerformanceService::SetDPS()` method that allows user to configure dynamic performance scaling,
* Introduction of a new `braiins.bos.v1.PerformanceService::SetPerformanceMode()` method that allows the user to set performance mode to one of the modes: manual or tuner,
* Introduction of a new `braiins.bos.v1.PerformanceService::GetActivePerformanceMode()` method that allows the user to read the current performance mode,
* Introduction of a new `braiins.bos.v1.PerformanceService::ListTargetProfiles()` method that allows the user to read the currently used tuner profile,
* Introduction of a new `braiins.bos.v1.MinerService::GetMinerStatus()` method streaming actual miner status.
* Introduction of a new `braiins.bos.v1.PoolService::RemovePoolGroup()` method that allows to remove pools group,
* Introduction of a new `braiins.bos.v1.PoolService::CreatePoolGroup()`,
* Extension of the `braiins.bos.v1.UpdatePoolGroupResponse` message with `group` field that contains updated pool group configuration,
* Extension of the `braiins.bos.v1.TunerConstraints` message with new field `hashrate_target` that contains constraints for hashrate target value,
* Extension of the `braiins.bos.v1.GetMinerStatsResponse` message with new field `power_stats` that contains miner power stats like consumption or efficiency,
* Extension of the `braiins.bos.v1.GetTunerStateResponse` message with new fields:
  * `current_power_target` that contains the current power target,
  * `current_hashrate_target` that contains the current hashrate target,
  * `current_profile` that contains info about the currently used tuner profile,
* Extension of the `braiins.bos.v1.Hashboard` with the new field `enabled` that represents a flag if HB is enabled
* Extension of the `braiins.bos.v1.GetMinerConfigurationResponse` with new fields:
  * `dps` that contain Dynamic Performance Scaling configuration,
  * `hashboard_config` holds information regarding the presently configured hashboard frequency, voltage settings, or the enabled/disabled status of HB,
* Extension of the `braiins.bos.v1.GetConstraintsResponse` message with new fields:
  * `dps_constraints` that contains constraints for Dynamic Performance Scaling configuration,
  * `hashboards_constraints` that contains constraints for hashboards config,
* Extension of the `braiins.bos.v1.Platform` enumeration with new values for CVITEK and Braiins control boards.

### Breaking Changes:
* The service `braiins.v1.bos.TunerService` has been updated to `braiins.v1.bos.PerformanceService` to encompass methods relevant to this service.
* Furthermore, the method `braiins.v1.bos.PerformanceService::SetAbsolutePowerTarget()` has been modified to `braiins.v1.bos.PerformanceService::SetPowerTarget()`, removing the potentially confusing term "Absolute".
* In alignment with the adjusted method name, the request message previously labeled as `braiins.bos.v1.SetAbsolutePowerTargetRequest` has been changed to `braiins.bos.v1.SetPowerTargetRequest`.
* Standardization of modification requests has been enacted, with each request containing a designated `save_action` field located at index 1.
* In anticipation of potential hashboard identifiers incorporating non-numeric characters in the future, adjustments have been made to the hashboards identifier fields, shifting from `uint32` to `string` type.

### Migration Guide:
In order to transition to version **1.0.0-beta.2**, the integration requires implementation of the subsequent adjustments:
* Ensure incorporation of the updated moniker `PerformanceService`. The same applies to usage of the `SetAbsolutePowerTarget` method. 
* Verify the seamless functionality of your modification request with the modified index placements within the `save_action` fields. 
* Guarantee accurate parsing of the novel string-based hashboard identifiers.

---

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

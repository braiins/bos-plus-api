# Braiins OS+ Public API

This repository contains protocol buffers for the new Braiins OS+ Public API, which is based on gRPC API technology.


### Versions

| Public API Version    | BOS+ version |
|-----------------------|--------------|
| 1.0.0-beta.2 (latest) | 23.08        |
| 1.0.0-beta.1          | 23.05        |
| 1.0.0-beta            | 23.04        |
| 1.0.0-alpha.1         | 23.03.3      |
| 1.0.0-alpha           | 23.03        |

### Overview

The new Braiins OS+ Public API was released as an alpha version alongside Braiins OS+ version 23.03. This API aims to provide a unified standard for all current and future hardware variants, regardless of manufacturer. However, since it is an alpha version, there might still be some changes in the API in future releases.

The API does not cover the full scope of the Bosminer API and GraphQL API in the alpha version. But in the next releases, this gap will be closed.

## Getting Started

gRPC is a high-performance remote procedure call (RPC) framework developed by Google. It allows developers to create efficient and reliable communications between client and server applications running on different platforms and written in different programming languages. Read more about gRPC at **[grpc.io](https://grpc.io)**


### Prerequisites
* Miner must have port 50051 enabled. As of BOS+ version 23.03.1, it should be enabled by default.

There is no specific prerequisite for using BOS+ Public API. It is possible to directly access it using standard GUI tools like **[Postman](https://www.postman.com)**, **[Kreya](https://kreya.app)** or CLI tool like **[grpcurl](https://github.com/fullstorydev/grpcurl)**. Please find an example how to access gRPC api using **grpcurl** below.

#### Installation
Read **https://github.com/fullstorydev/grpcurl**

## Usage
Best way to start using our public API is to list available services and describe them. This can be done 2 ways:
* using prepared proto files,
* using reflection.
### Proto files
Example of how to list available services in proto file and how to describe service.
```shell
$ grpcurl -import-path ./proto -proto bos/v1/actions.proto list
braiins.bos.v1.ActionsService

$ grpcurl -import-path ./proto -proto bos/v1/actions.proto describe
braiins.bos.v1.ActionsService is a service:
service ActionsService {
  // Method to pause mining
  rpc PauseMining ( .braiins.bos.v1.PauseMiningRequest ) returns ( .braiins.bos.v1.PauseMiningResponse );
  // Method to reboot whole miner
  rpc Reboot ( .braiins.bos.v1.RebootRequest ) returns ( .braiins.bos.v1.RebootResponse );
  // Method to restart bosminer
  rpc Restart ( .braiins.bos.v1.RestartRequest ) returns ( .braiins.bos.v1.RestartResponse );
  // Method to resume mining
  rpc ResumeMining ( .braiins.bos.v1.ResumeMiningRequest ) returns ( .braiins.bos.v1.ResumeMiningResponse );
  // Method to start bosminer
  rpc Start ( .braiins.bos.v1.StartRequest ) returns ( .braiins.bos.v1.StartResponse );
  // Method to stop bosminer
  rpc Stop ( .braiins.bos.v1.StopRequest ) returns ( .braiins.bos.v1.StopResponse );
}

```

### Reflection
BOS+ Public API has gRPC Server Reflection enabled. gRPC Server Reflection provides information about publicly-accessible gRPC services on a server, and assists clients at runtime to construct RPC requests and responses without precompiled service information. It is used by gRPC CLI, which can be used to introspect server protos.

Readme more at **[gRPC Server Reflection Tutorial](https://grpc.github.io/grpc/core/md_doc_server_reflection_tutorial.html)**.

#### Examples
List available services
```shell
$ grpcurl -plaintext miner:50051 list
braiins.bos.ApiVersionService
braiins.bos.v1.ActionsService
braiins.bos.v1.AuthenticationService
braiins.bos.v1.ConfigurationService
braiins.bos.v1.CoolingService
braiins.bos.v1.MinerService
braiins.bos.v1.PoolService
braiins.bos.v1.TunerService
grpc.reflection.v1alpha.ServerReflection
```

Describe available service
```shell
$ grpcurl -plaintext miner:50051 describe
braiins.bos.ApiVersionService is a service:
service ApiVersionService {
  rpc GetApiVersion ( .braiins.bos.ApiVersionRequest ) returns ( .braiins.bos.ApiVersion );
}
braiins.bos.v1.ActionsService is a service:
service ActionsService {
  // Method to pause mining
  rpc PauseMining ( .braiins.bos.v1.PauseMiningRequest ) returns ( .braiins.bos.v1.PauseMiningResponse );
  // Method to reboot whole miner
  rpc Reboot ( .braiins.bos.v1.RebootRequest ) returns ( .braiins.bos.v1.RebootResponse );
  // Method to restart bosminer
  rpc Restart ( .braiins.bos.v1.RestartRequest ) returns ( .braiins.bos.v1.RestartResponse );
  // Method to resume mining
  rpc ResumeMining ( .braiins.bos.v1.ResumeMiningRequest ) returns ( .braiins.bos.v1.ResumeMiningResponse );
  // Method to start bosminer
  rpc Start ( .braiins.bos.v1.StartRequest ) returns ( .braiins.bos.v1.StartResponse );
  // Method to stop bosminer
  rpc Stop ( .braiins.bos.v1.StopRequest ) returns ( .braiins.bos.v1.StopResponse );
}
braiins.bos.v1.AuthenticationService is a service:
service AuthenticationService {
  rpc Login ( .braiins.bos.v1.LoginRequest ) returns ( .braiins.bos.v1.LoginResponse );
}
...
```

### Authentication
Almost all requests require authentication. Requests authorize themselves with a token present in the Authorization header.

#### How to get auth token
Send a request with username and password to **Login** method in **AuthenticationService**. The token is present in the **authorization** header. To print response header use `-vv` flag for verbose output.

```shell
$ grpcurl -plaintext -vv -d '{"username": "xxxx", "password": "yyyy"}' miner:50051 braiins.bos.v1.AuthenticationService/Login

Resolved method descriptor:
rpc Login ( .braiins.bos.v1.LoginRequest ) returns ( .braiins.bos.v1.LoginResponse );

Request metadata to send:
(empty)

Response headers received:
authorization: FvZarvVQLCtzNaM6
content-type: application/grpc

Estimated response size: 0 bytes

Response contents:
{
  
}

Response trailers received:
(empty)
Sent 1 request and received 1 response
```

#### How to use auth token
To authenticate requests with a token just add `authorization` header with token using `-H` flag.

```shell
$ grpcurl -plaintext -H 'authorization:FvZarvVQLCtzNaM6' miner:50051 braiins.bos.v1.MinerService/GetMinerDetails
{
  "uid": "lAOJ0RbI6axyOyGf",
  "minerIdentity": {
    "brand": "MINER_BRAND_ANTMINER",
    "model": "MINER_MODEL_ANTMINER_S19J_PRO",
    "name": "Antminer S19J Pro"
  },
  "platform": "PLATFORM_AM3_BBB",
  "bosMode": "BOS_MODE_NAND",
  "bosVersion": {
    "current": "2023-04-20-0-0ce150e9-23.03-plus",
    "major": "2022-09-13-0-11012d53-22.08-plus",
    "bosPlus": true
  },
  "hostname": "bosminer",
  "macAddress": "94:e3:6d:fb:89:ad",
  "systemUptime": "90919"
}

```

### Version
It is important to note what version of Public API must be used for communication with the miner. Current API version on the miner is available via `GetApiVersion` method in `braiins.bos.ApiVersionService`. This service does not require authentication.
```shell
$ grpcurl -plaintext miner:50051 braiins.bos.ApiVersionService/GetApiVersion
{
  "major": "1",
  "pre": "alpha"
}
```
**Note**: Version `1.0.0-<ver>` is actually `1.0.0-<ver>.0`. `0` as default value was dropped during serialization.


### Proto files
#### 1. proto/bos/v1/actions.proto
Contains miner actions related protobuf messages and **ActionsService** with various method to control miner:
* **Start** - method to start mining,
* **Stop** - method to stop mining,
* **PauseMining** - method to pause mining,
* **ResumeMining** - method to resume mining,
* **Restart** - method to restart mining,
* **Reboot** - method to reboot whole miner,
* **SetLocateDeviceStatus** - method to enable/disable locate device mode,
* **GetLocateDeviceStatus** - method to retrieve the locate device mode status.

#### 2. proto/bos/v1/authentication.proto
Contains authentication related messages and **AuthenticationService** with various methods:
* **Login** - method to log in and get auth. token,
* **SetPassword** - method to set new password.

#### 3. proto/bos/v1/common.proto
Contains **SaveAction** enumeration used for setting configuration. Options:
* `SAVE_ACTION_SAVE` - only save new configuration
* `SAVE_ACTION_SAVE_AND_APPLY` - save and apply new configuration
* `SAVE_ACTION_SAVE_AND_FORCE_APPLY` - save and force apply new configuration

#### 4. proto/bos/v1/configuration.proto
Contains configuration related messages and **ConfigurationService** with various methods to read or modify miner settings:
* **GetMinerConfiguration** - method to read current miner configuration,
* **GetConstraints** - method to read current configuration constraints(min, max, default).

#### 5. proto/bos/v1/constraints.proto
Contains protobuf messages used for constraints.

#### 6. proto/bos/v1/cooling.proto
Contains cooling related messages and **CoolingService** with various methods to read cooling info and modify cooling settings:
* **GetCoolingState** - method to read current temperature measurements and fans states,
* **SetImmersionMode** - method to set/toggle immersion mode.

#### 7. proto/bos/v1/license.proto
Contains license related messages and **LicenseService** with method to read license state:
* **GetLicenseState** - method to read current license state.

#### 8. proto/bos/v1/miner.proto
Contains miner related messages and **MinerService** with various methods to read info about miner:
* **GetMinerStatus** - method to fetch miner status,
* **GetMinerDetails** - method to read miner details info like model, IP, uptime, etc.,
* **GetMinerStats** - method to read aggregated miner stats,
* **GetHashboards** - method to read miner hashboards state and statistics,
* **GetSupportArchive** - method to download BOS support archive,
* **EnableHashboards** - method to enable hashboards,
* **DisableHashboards** - method to disable hashboards.

#### 9. proto/bos/v1/performance.proto
Contains tuner related messages and **TunerService** with various methods to read or modify tuner:
* **GetTunerState** - method to read current tuner state and available tuner profiles,
* **ListTargetProfiles** - method to set default power target,
* **SetDefaultPowerTarget** - method to set default power target,
* **SetPowerTarget** - method to set specific power target,
* **IncrementPowerTarget** - method to increment currently configured power target by a specific value,
* **DecrementPowerTarget** - method to decrement current configured power target by a specific value,
* **SetDefaultHashrateTarget** - method to set default hashrate target,
* **SetHashrateTarget** - method to set specific hashrate target,
* **IncrementHashrateTarget** - method to increment currently configured hashrate target by a specific value,
* **DecrementHashrateTarget** - method to decrement current configured hashrate target by a specific value,
* **SetDPS** - method to set Dynamic Performance Scaling,
* **SetPerformanceMode** - method to set performance mode,
* **GetActivePerformanceMode** - method to read active(runtime) performance mode.

#### 10. proto/bos/v1/pool.proto
Contains pools related messages and **PoolService** with various methods to read or modify pool settings:
* **GetPoolGroups** - method to read current pools state and statistics,
* **CreatePoolGroup** - method to create pool group
* **UpdatePoolGroup** - method to update default pool group,
* **RemovePoolGroup** - method to remove pool group.

#### 11. proto/bos/v1/units.proto
Contains protobuf messages representing various units like Voltage, Frequency, etc.

#### 12. proto/bos/v1/work.proto
Contains mining work related protobuf messages.

#### 13. proto/bos/version.proto
Contains **ApiVersionService** service with **GetApiVersion** to be able to read current Public API version available for communication with miner




### Contact
Do you have questions or specific needs from the API? Our dev and support teams are always available to help. You can send a request to our support team or open a GitHub issue.

You can also join our Telegram group at https://t.me/BraiinsOS point and customize it to fit your project's specific needs. 

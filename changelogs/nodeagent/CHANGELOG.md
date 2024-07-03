# Azure Batch Node Agent Change Log
### About the Batch Node Agent
The Batch Node Agent executes customer-defined tasks on compute nodes 
within a pool. This includes any actions that must be performed on the 
compute nodes themselves including (but not limited to) preparation,
reporting, maintenance, execution, and removal of said tasks.

### A Note on Dates
Next to each version below, a date is given. Due to the global nature of the
Azure Batch service, new versions of the agent are not available in every 
region simultaneously upon release. The date given below each version is
the date the new version begins deployment to all supported regions. It is 
the _earliest possible_ time you could see the new agent version. There is
no guarantee that a pool has a particular version of the agent even if 
it was created after the release date of that version.

### Getting the Latest Version of the Batch Node Agent
All new Azure Batch pools are automatically provisioned with the latest 
Node Agent, unless the Azure Batch team deems it necessary to limit or 
rollback a certain version due to possible customer impacting concerns.
In order to get the latest version of the Batch Node Agent, a new pool should
be created or the pool should be resized to 0 nodes and then back to the
desired target node count.

### Determining what version of the Batch Node Agent is on a Node
Using a client targeting REST API version `2018-08-01.7.0` or
newer call the [get compute node](
https://docs.microsoft.com/rest/api/batchservice/computenode/get)
or [list compute nodes](
https://docs.microsoft.com/rest/api/batchservice/computenode/list)
APIs and examine the `version` property of [nodeAgentInfo](
https://docs.microsoft.com/rest/api/batchservice/computenode/list#nodeagentinformation).

## Change Log

### 1.11.11
#### Released: 2024-05-06
- Fix issue with local temp disk mount with containers.
- Support up to 150 tasklets for multi-instance tasks.
- Fix CVE-2023-49081, CVE-2023-50782, CVE-2023-26130, CVE-2023-22190.
- Support new path isolation options for container tasks.

### 1.11.8
#### Released: 2024-02-02
- Re-establish the Docker client and retry when ImageNodeFound errors occur when creating containers.
- Return specific error code for when a task is terminated due to wall clock time being exceeded.
- Temporary disks were incorrectly handled for `CloudServiceConfiguration` pools.

### 1.11.5
#### Released: 2023-11-29
- Fix bug Reactivate failed task cannot work.
- Fix bug tvm stucking joinPoolInProgress.
- Fix bug Task remains in running state in Azure Portal while Job was ended by Customer.
- Fix CVE-2023-37276 and GHSA-v8gr-m533-ghj9.

### 1.11.3
#### Released: 2023-11-06
- Pool certificates on Linux are no longer returned for GetFile, GetFileProperties, and ListFiles APIs.
- Fix issue where files not in AZ_BATCH_NODE_ROOT_DIR were allowed via GetFile and GetFileProperties.
- Added support for Virtual Machine with no local temporary disk. Not all Virtual Machine SKUs without local temporary disks will be supported. Please consult the [List Supported Virtual Machine Sizes](https://learn.microsoft.com/rest/api/batchmanagement/location/list-supported-virtual-machine-skus) API for more information.
- Recheck local cache before making repeat IMDS queries on retries.
- Default Docker client response timeout increased to 5 minutes.

### 1.10.9
#### Released: 2023-09-20
- Increased max connection limit when making Azure Storage calls to limit timeout errors when downloading many resource files.

### 1.10.8
#### Released: 2023-09-05
- Resolved CVE-2023-38325
- Increased timeout before failing when trying to authenticate to AutoStorage configured with a managed identity.

### 1.10.7
#### Released: 2023-08-25
- Tasks that are currently running on a VM were not rejected if they tried to be scheduled again.
- The maximum task history was previously limited to 10 items.
- Filesystem mounting would incorrectly terminate when waiting for unattended-upgrades to finish. 
- Downloading app packages could get into an endless loop if managed identity was incorrectly configured.
- Deleting a task could sometimes cause a race condition where the task state would be lost.
- Ubuntu 18.04 has been deprecated per OS being EOL.
- Debian 10 has been deprecated per OS being EOL.
- Certain windows images would fail to bootstrap if they had both Docker and Containerd services installed.
- Added Blobfuse support for ubuntu 22.04.

### 1.10.0
#### Released: 2023-06-27
- Add retries for Docker(dockerpy) create due to intermittent failures on first task.
- Add containerd support.
- Add retries when mount installation fails.
- Add timeout setting for mount operation.

### 1.9.43
#### Released: 2023-04-19
- Recover if a VM cannot communicate with storage and retry when out of ports.
- Convert path ownership to async for critical methods.
- Retry on incomplete payload error.
- Handling exception from terminate running task and perform escalating recovery.

### 1.9.42
#### Released: 2023-04-12
- Add EL9 NodeAgentSku for RHEL like images.
- Fix ACR login error.
- Add corrupt executable as a non-retryable error.
- Sign external exe's with the correct profile.
- Resolved CVE-2023-0286 for vulnerable Node Agent SKUs.

### 1.9.33
#### Released: 2022-11-04
- Resolved CVE-2022-3602 and CVE-2022-3786 for vulnerable Node Agent SKUs.

### 1.9.32
#### Released: 2022-10-25
- Fixed Managed Identity support in Sovereign Clouds.

### 1.9.31
#### Released: 2022-10-04
- Socket read timeouts on chunked resource file downloads were not correctly retried.

### 1.9.30
#### Released: 2022-09-16
- Fixed rate limiting errors when querying IMDS for managed identity tokens.
- ACR credentials when using managed identities are automatically refreshed when they expire.
- Fixed handling long filenames during task garbage collection.
- 7-Zip is now the preferred compression tool on Windows for agent decompression during installation.
- Fixed an issue where a compute node could get stuck yielding tasks after encountering disk full error.

### 1.9.29
#### Released: 2022-07-19
- Added NodeAgentSku support for Ubuntu 22.04
- Fix VNet injection issue when container fails to start
- Improve retry logic for HTTP requests
- Improve security of generated passwords

### 1.9.28
#### Released: 2022-06-17
- Fix issue with Windows SSL handling
- Fix issue which wrongly reporting RDMA enabled on some particular VM SKUs
- Fix issue where large output files could fail due too invalid XML error messages.
- Fix issue where deleting jobs with a large amount of tasks could result in deadlock.
- Remove Support for the following NodeAgentSku's
  - CentOS 8
  - Debian 8
  - Debian 9
- Improved logging when using containers
- Added support for multi-instance tasks to be retried.

### 1.9.27
#### Released: 2022-05-07
- Fix client authentication

### 1.9.24
#### Released: 2022-02-04
- Remove infinite retries when attempting to persist state to storage.
- Fix issue where Windows nodes would enter an unusable state if the Docker daemon took longer than expected to come up.
- Fix issue causing 5 minute delays when attempting to mount Azure NFS 3.0 due to lack of support for a diagnostic probe call.
- Improved recovery logic when nodes fail to join a pool.
- Managed Identity support for customer facing Azure services.
- Allow the node agent to function in images without a /sys folder.
- Improved behavior regarding losing disks after events including preemption
- Deprecated Ubuntu 16.04 NodeAgentSku
- Add EL8 NodeAgentSku for RHEL like images including Centos, Alma, and Rocky distributions.
- Update upload files internal errors to be deemed retryable
- Add support for specifying HTTP headers to be used when uploading output files.
- Improved error handling around creating users.
- Improved handling regarding network issues while joining pool.

### 1.9.10
#### Released: 2021-05-17
- Added retries on Windows for Docker initialize calls to improve stability
- Fix regression which caused SRIOV enabled VM sizes to be unusable
- Fix regression which caused all file upload errors to be classified as internal errors.
- Improved logging

### 1.9.9
#### Released: 2021-04-21
- ApplicationPackage zip files are now deleted after successful unpacking
- Added FILE_NO_FOUND as a retryable error when downloading container images
- Fixed file lock race condition in windows bootstrap introduced in 1.9.7

### 1.9.8
#### Released: 2021-03-22
- Standard tasks will now be rescheduled on an internal error instead of being retried locally

### 1.9.7
#### Released: 2021-03-22
- Added retries on some Windows Docker pipe errors

### 1.9.6
#### Released: 2021-02-25
- Fix issue where RHEL based images would not start.
- Add support for RHEL 8 based images.
- Fix issue where downloading resource files could be retried forever. Tasks will now eventually fail with `ResourceDownloadTimedOut` and be eligible for retries.
- Change functionality where the agent would retry a task and increment its `retryCount` on failure up to three times if `maxTaskRetryCount` was set, prior to yielding the task to be rescheduled.  This behavior has changed to yield the task after the first failure.

### 1.9.3
#### Released: 2021-02-02
- Add additional error code to better communicate task terminations cause.
- Fix issue in mounting NFS due to support packages sometimes being incorrectly detected as installed.
- Added support for using images based on Ubuntu 20.04.

### 1.8.7
#### Released: 2020-09-29
- Fix issue in application package download retry logic.
- Application package and container image downloads will now transition the respective task into `running` state.
  Prior to this the task would remain in `active` state until these actions completed.

### 1.8.6
#### Released: 2020-09-09
- Add support for [variable slots scheduling](https://docs.microsoft.com/azure/batch/batch-parallel-node-tasks).

### 1.8.5
#### Released: 2020-08-10
- Fix bug on newly provisioned Windows nodes failing to transition to `idle` in `VirtualMachineConfiguration` pools.

### 1.8.4
#### Released: 2020-08-04
- Fix bug where uploading output files could fail due to intermittent DNS failures.
- Fix bug in node preparation where some classifications of timeout errors were no longer being retried.  
- Fix bug where spawning the process for a task could fail in rare cases.
- Nodes will now detect when the OS disk is approaching being full and 
  will be marked as `Unusable` with a corresponding `NodeError`.
  Running tasks will be requeued for execution elsewhere.

### 1.8.3
#### Released: 2020-06-30
- Fix regression in `CloudServiceConfiguration` pools where nodes would get stuck in unusable after reboot.
- Improve error handling during resource file download failure due to DNS resolution failures.
- Fix bug where data was not cleaned up properly for `JobPreparationTask` when enabled for re-execution on reboot.

### 1.8.2
#### Released: 2020-06-09
- Fix bug where the node agent could get in a bad state if too many requests are
  sent concurrently.
- Fix a race condition for customers using temporary disk encryption which
  resulted in nodes being `unusable`.
- Fix bug when using per-job Virtual Network injection.
- Linux pools now expose another environment variable for application package
  references which preserves case `AZ_BATCH_APP_PACKAGE_{Application}_{Version}`
  in addition to the existing `AZ_BATCH_APP_PACKAGE_{APPLICATION}_{VERSION}`.
- Optimize setup steps for pools with specified `MountConfiguration`.

### 1.8.0
#### Released: 2020-05-04
- Improve error messages for user login failures.

### 1.7.14
#### Released: 2020-03-07
- Append a `\n` to empty certificate password files in Linux.
  OpenSSL expects a newline if the file is otherwise empty.
- Add a new environment variable for Batch Node Agent reserved 
  disk space: `AZ_BATCH_RESERVED_DISK_SPACE_BYTES`.
- Add a new environment variable for the Start Task working directory:
  `AZ_BATCH_NODE_STARTUP_WORKING_DIR`.
- Fix bug where container task terminate can fail.
- Improve error handling for container pull.

### 1.7.10
#### Released: 2020-02-14
- Fix bug where certificates installed to the "CurrentUser" store
  on Windows could have inaccessible private keys for short periods of time 
  when the pool had `MaxTasksPerNode` greater than 1 and tasks were run 
  with `UserScope` `Pool`.
    - This was caused by tasks reinstalling the certificate into the certificate 
      store each time they launched.
- Fix bug where the `FailureInfo` property of the `StartTask` could persist 
  through retries, causing a successful `StartTask` to look like it had 
  failed.

### 1.7.9
#### Released: 2020-01-30
- Fix bug where `Task` scope `AutoUser` profiles were not properly deleted.
- Fix bug preventing `blobfuse` installation in some cases.

### 1.7.8
#### Released: 2020-01-24
- Fix bug which could cause a node to get stuck in the `Unusable` state if
  a multi-instance task was deleted shortly after it completed.

### 1.7.7
#### Released: 2020-01-17
- Add Debian 9/10 support for blobfuse mounts (specified in 
  `MountConfiguration` on the `Pool`). Note that Debian 10 support is not yet
  available in Batch but is coming soon.
- Add CentOS 8 support for NFS/CIFS/Azure Files (specified in 
  `MountConfiguration` on the `Pool`). Note that CentOS 8 support is not yet
  available in Batch but is coming soon.
- Fix bug which could cause nodes to block Pool resize and be unable 
  to reboot when they ran out of disk space.
- Improve error message if PATH not set in Windows.

### 1.7.6
#### Released: 2020-01-07
- Fix bug which could cause terminate job to get stuck in some cases.
- Fix bug where Job Preparation tasks would not retry more than 3 times 
  even if they had been configured to do so.
- Fix bug where after a node ran out of disk space it would have one or 
  more tasks stuck in `Running` state and not respond to reboot requests.
- Fix bug which could cause tasks with dependencies configured to fail on 
  some non-English locales.
- Fix bug where a `ResourceFile` with an Azure Storage container URL specified
  would not correctly download files from a container with public access 
  configured.
- Fix bug which in rare cases could cause deletion of multi-instance subtasks 
  to get stuck.
- Improve error handling for disk full in Linux where the file system has 
  run out of free inodes.
- Improve error message reported when a task has run out of wall clock time 
  and is terminated.

### 1.7.5
#### Released: 2019-12-06
- Fix bug where the node did not correctly block new tasks from being 
  scheduled when it detected the disk was full.
- Fix bug which in rare cases could cause tasks run on nodes with 
  `MaxTasksPerNode` > 1 to intermittently fail.
- Fix bug which could cause a task to fail when it referenced a specific
  `ApplicationPackage` version if the `Application` had a different 
  default version which was not referenced.
- Fixed bug which could cause Linux nodes to be `Unusable` due to issues
  with the SSH service.

### 1.7.4
#### Released: 2019-10-23
- Fix rare bug where Linux nodes (usually CentOS) would immediately go into 
  `Unusable` state after starting due to incorrectly detecting
  the machine drive layout.
- Fix bug which could cause tasks with `ContainerSettings` which had
  a long `ContainerRunOptions` to fail to properly launch,
  resulting in the task becoming stuck in `Running` state on the node. 

### 1.7.2
#### Released: 2019-10-21
- Add support for SR-IOV IB/RDMA devices with Docker containers.
- Improve SSH daemon configuration on first boot for Linux VMs.
- Slightly increase priority of node agent and controller processes 
  on Linux to decrease probability of CPU starvation in situations 
  of CPU over subscription which can help prevent unnecessary 
  node recoveries.
- Fix bug where some requests to Azure Storage were improperly retried.

### 1.7.1
#### Released: 2019-10-09
- Fix bug which caused the agent to deadlock when infinitely recursive 
  environment variables were defined on a task.
- Fix a number of rare bugs which could cause internal metadata corruption
  and prevent tasks from being run.
- Fix bug which caused the default-version application package environment
  variable (e.g. `AZ_BATCH_APP_PACKAGE_Foo` for an application named `Foo`) 
  to be unset when a pool level application was already installed 
  and then a more recent task level package was installed.
- Fix bug where DNS errors during `ResourceFile` download were incorrectly 
  categorized as `BlobNotFound`.
- Fix bug which prevented `NFSMountConfiguration` from functioning in cases
  where the `nfs-common` package was not pre-installed on the operating system.
- Fix bug where a task interrupted due to an unexpected reboot would appear in
  the compute node's `RecentTasks` and also left task-related data on disk.
- Fix bug where a job preparation task which had surpassed its data retention 
  time would block that job from being deleted forever.
- Fix bug where an internal error or pool shrink with `Requeue` option could
  cause an interrupted `JobManager` task to incorrectly terminate the job
  when `KillJobOnCompletion` was set to `true` on the `JobManager`.

### 1.6.6
#### Released: 2019-08-26
- Nodes will now detect when the ephemeral disk is approaching full and 
  will be marked as `Unusable` with a corresponding `NodeError`.
  Running tasks will be requeued for execution elsewhere.
- Allow delete, terminate, and disable job while the node is in `Unusable`
  state, as long as no job release task is required to run.
- Improve resource file download error messages.
- Support for Ubuntu 14.04 dropped.
- Reduced load on the scheduler by filtering what information is returned
  during compute node initialization.
- Fix bug where job release tasks would not run properly if the 
  job preparation task was deleted due to retention time. When a job release 
  task is run and the job preparation task has been cleaned up due to 
  retention time the `AZ_BATCH_JOB_PREP_DIR` environment variable 
  points to where the directory _was_ (it was deleted due to data 
  retention time)
- Fix bug where non-standard tasks such as the start task or 
  job preparation task were being reported in task history 
  (`RecentTasks` on the node) unintentionally.
- Fix bug where parallel ongoing operations could corrupt state and cause
  the node to be unable to perform new work.
- Fix bug where during list file operations when an error occurred an 
  incomplete file list with no continuation token would be returned rather
  than an error.
- Fix bug where `AZ_BATCH_POOL_ID` was not correctly preserving case.

### 1.6.3
#### Released: 2019-07-22
- Add ability to mount remote file systems. Supported file system types are 
  Blobfuse, CIFS/SMB, Azure Files, and NFS. Support for specifying remote 
  file systems on a Pool will be in an upcoming Batch API version.
- Improve error handling and reporting for nodes in `Unusable` state. 
  Additional details about why the node is `Unusable` will now consistently
  be available in the `errors` property on the 
  [ComputeNode](https://docs.microsoft.com/rest/api/batchservice/computenode/get#computenode).
- Allow custom ephemeral disk mount points and support additional 
  common file system types for the ephemeral disk in Linux.
- Support `OpenLogic CentOS-HPC 7.6`.
- Fix bug where container image tags with uppercase characters were not
  downloaded correctly.
- Fix bug where tasks running as non-elevated users in Linux incorrectly 
  had all of their groups cleared.

### 1.5.11
#### Released: 2019-06-05
- Fix regression introduced in 1.5.10 where reboots or reimages could be 
  delayed up to 15 minutes.

### 1.5.10
#### Released: 2019-05-31
- Fix bug where files with unicode characters in names were not properly 
  uploaded during Output File upload.
- Fix bug where RHEL and Oracle Linux compute nodes were provisioned 
  as `unusable`.
- Fix bug where compute node recovery attempts via reboot were sometimes 
  not honored.
- Continued improvements in retry logic for container login and pull failures.

### 1.5.9
#### Released: 2019-05-16
- Fix bug where task metrics could be dropped due to integer overflow.
- Fix extremely rare bug causing tasks to be stuck in `running` state even 
  when the associated processes had exited on Windows.
- Improve error handling when terminating a process fails due to
  an operating system error.

### 1.5.8
#### Released: 2019-05-02
- Fix bug which caused the node to become `unusable` when the ephemeral
  drive was lost but the operating system drive was retained due to 
  incorrectly attempting to create already existing pool scope user accounts.

### 1.5.7
#### Released: 2019-04-26
- Improve retries on DNS resolution failures.
- Retry up to a maximum of 3 times for a retryable task which fails in 
  succession on a compute node before attempting to reschedule on another node
  if possible.
- Fix bug which could cause statistics collection to be delayed.

### 1.5.6
#### Released: 2019-04-19
- Add preliminary support for per-job VNET injection.
- Fix bug where a task unable to complete on the compute node was 
  incorrectly reported as completed.
- Fix bug where a task encountering a bad image format exception was 
  treated as an internal error and retried forever.
- Pool scope users are no longer deleted and recreated on node boot.
  This attempts to reduce a number of internal errors encountered either 
  because the Windows user profile service has an issue, or the customer has 
  done something to lock the user profile directory. Pool scope users and 
  their associated data now persist for the lifetime of the node,
  including through reboots.
- Improve error handling for Docker login and pull actions.

### 1.5.5
#### Released: 2019-04-03
- Fix bug where application package environment variables were sometimes
  not correctly updated when the default version of the application was 
  changed and the node was rebooted.
- Fix bug where after a node reboot in Linux, sometimes the `List Task Files`
  or `Get Task File` APIs would fail to find a file that was actually on disk.
- Fix bug where `ExitConditions` were ignored and overwritten to be empty.

### 1.5.4
#### Released: 2019-03-25
- Internal telemetry improvements to help the product team better diagnose 
  issues after VMs have been removed from a pool.
- Fix issue where a concurrent termination or deletion operation in 
  conjunction with command process exit would prevent new tasks from 
  being run.

### 1.5.3
#### Released: 2019-03-12
- Add support for `--mount` option on container tasks.
- Add support for an upcoming Batch Service feature allowing container
  tasks to use the working directory of the container rather than 
  the default Batch working directory for the task.
  - This feature will be enabled in a future Batch REST API version.
- Fix bug which could cause a task to get stuck if deleted immediately
  after it started running.
- Fix bug where the multi-instance master subtask could 
  mistakenly overwrite details about the execution of the other subtasks. This 
  caused the subtasks to not show up in the API until they reached 
  `completed` state.
- Fix bug where `docker login` was not retried when throttled.
- Improve handling of Windows user profile creation getting stuck.

### 1.4.12
#### Released: 2019-02-15
- Fix bug which could corrupt the download of the final portion of a 
  `ResourceFile` in rare cases.

### 1.4.11
#### Released: 2019-02-05
- Fix bug where the OS drive is replaced (but the ephemeral drive is retained)
  which caused nodes to get stuck in `Unusable`.

### 1.4.10
#### Released: 2019-02-03
- Fix bug that caused the node to get stuck in `Unusable` 
  due to rare cases of unavailable networking at the time of agent boot.
- Fix bug where frequent reboots could cause the agent to be unable
  to join a pool, even if the reboots stopped happening.
- Fix bug where job preparation tasks could get stuck on VMs with 
  `MaxTaskPerVMs` greater than 1 if multiple tasks were scheduled to
  the node at the same time.
- Fix bug where files downloaded by the node could get corrupted.
- Fix bug where the agent could not download `ResourceFile`s which specified
  `AutoStorageContainerName`.
- Fix bug where node preparation tasks, job preparation tasks, and job release
  tasks were put into an incorrect state if the agent had an internal error.

### 1.4.9
#### Released: 2019-01-14
- Fix bug where connection/HTTP failures happening persistently for a long 
  period of time could cause the agent to enter an unrecoverable state.
- Retry longer on connection errors.
- Retry on intermittent DNS failures.
- Move Windows user profiles to the ephemeral drive for 
  `CloudServiceConfiguration` based pools.

### 1.4.7
#### Released: 2018-11-29
- Implement a timeout on Windows user profile creation. If the user profile
  creation takes too long, the task is retried (possibly on another 
  compute node).
- Fix bug on Windows where some tasks which create child processes could 
  prevent task termination even if the parent process exited, causing 
  the task to get stuck.
- Fix application package root directory to be `applications`
  and not `apppackages`.
- Fix bug where tasks which were retried on the node did 
  not properly clean up their working directory prior to retry.
- Add support for specifying Windows user login mode (`batch` or `interactive`).
- Add support for Windows Server 2019 in `VirtualMachineConfiguration` pools.

### 1.4.5
#### Released: 2018-11-13
- Fix bug where certain connection errors were not properly 
  retried when performing resource file downloads on Windows.

### 1.4.4
#### Released: 2018-11-06
- Fix bug where multi-instance tasks with common resource files and normal 
  resource files would fail to download the common resource files in 
  some cases.
- Fix bug where the agent could leak memory when preparing the task
  command line for execution in Windows.
- Fix bug which prevented Batch service logs from being written to
  disk on Windows.
- Improve error handling for all resource file downloads.

### 1.4.3
#### Released: 2018-10-29
- Fix bug where files which were marked read-only on Windows could not
  be deleted in some cases.
- Fix bug where file deletion failures during node boot could delay
  initialization for a long time, causing the node get marked `Unusable`.
- Fix bug where the tasks spawned by the NodeAgent on Windows would incorrectly 
  have the win32 `DllDirectory` path set. This allows native applications 
  using the `SetDllDirectory` API to behave as intended.
- Use a more generous connection/transmission timeout when uploading files via
  `OutputFiles`.
- Fix bug where unversioned environment variables for applications 
  (e.g. `AZ_BATCH_APP_PACKAGE_AppName`) could incorrectly point at older 
  package versions in some cases if both old and new packages were
  installed onto the same node.
- Reduce log file disk usage for VMs with limited ephemeral disk space.

### 1.4.2
#### Released: 2018-10-16
- Fix bug introduced in `1.3.0.12` where nodes with application packages
  could go to `Unusable` after being rebooted.
- Improve the logs generated when uploading `OutputFiles` in 
  `fileuploadout.txt`.

### 1.3.0.12
#### Released: 2018-09-28
- Fix bug where once a node had installed a task level application package
  it would include that application package environment variable on other 
  tasks even if they didn't specify a dependency on that application package.
- Fix bug where job preparation tasks which failed due to a certificate 
  installation error could never be re-run, which blocked the job on that node
  forever.
- Reduce the time to install and prepare the Batch components on Windows 
  compute nodes. In ideal cases this reduces the time taken for a node to
  reach `idle` state when initially joining a pool.

### 1.3.0.11
#### Released: 2018-09-13
- Signed binaries are now provided for Windows builds of the Node Agent.
- Fix bug where some Node Agent files had a corrupt digital signature.
- Reduce how aggressively the Node Agent retries on directory delete failures.
- Fix bug where sometimes autoscale metrics could be invalid.
- Fix bug where Windows tasks running in parallel (on pools with 
  `MaxTasksPerNode` > 1) could inherit each others stdout/stderr handles.
  This could cause failures when trying to delete the task's files because
  running processes would still have locks on stdout/stderr or lead 
  to restarts.

### 1.3.0.10
#### Released: 2018-08-31
- Fix regression where rebooting a node with certificates could cause it to 
  become unresponsive.

### 1.3.0.9
#### Released: 2018-08-21
- Reduce agent disk usage by removing extraneous logging and reducing
  footprint of stored certificate data.
- Fix bug where the agent could get stuck attempting to perform environment
  variable expansion on an infinite loop of environment variables.
- Improve error returned to user if an environment block has a loop in it.
- Fix bug when using `ContainerSettings` with `--hostname`.

### 1.3.0.8
#### Released: 2018-08-13
- Add support required for task `result` OData filter for all task types.
- Improve detection of Blob Not Found errors when downloading 
  resource files or application packages.
- Improve directory deletion logic:
  - Files or directories locked by running processes.
  - Files or directories have unexpected ownership/permissions.
- Added support for the following new images:
  - `Canonical:UbuntuServer:18.04-LTS`
  - `microsoft-azure-batch:centos-container:7-5`
- Fix bug and added retries where where docker login could fail due to 
  a timeout error.
- Fix bug where node couldn't run any tasks after a reboot, due to getting
  stuck trying to garbage collect task files.

### 1.3.0.6
#### Released: 2018-07-10
- Add support for software entitlements given to the Node Agent when it joins
  the pool (for use in the StartTask).
- Add support for the Node Agent exposing its version and
  last update time in the public Azure Batch REST API.
  - This will be utilized in an upcoming REST API release.
- Improve telemetry emitted by the Node Agent if it fails to properly
  bootstrap.
- Fix bug where using 
  `microsoft-azure-batch:ubuntu-server-container:16-04-lts` or
  `microsoft-azure-batch:ubuntu-server-container-rdma:16-04-lts` marketplace
  images on a GPU SKU would cause kernel updates to be perpetually held.
- Fix bug where in rare cases tasks could get stuck in `running` state if
  the Node Agent process was terminated at a certain point.
- Fix bug where task application packages could sometimes fail to
  download if the task was rescheduled after previously running elsewhere.

### 1.3.0.2
#### Released: 2018-05-25
- Add Batch icon and file version info to Windows executables.
- Cap tasks to 95% CPU on single core Windows VMs to prevent Azure
  infrastructure agents from starvation.
- Reduce Node Agent CPU/disk usage when spawning new tasks and running tasks.
- Update agent to support new `microsoft-azure-batch` published marketplace
  images with Docker container support.
  - Includes using nvidia-docker2 in pools supporting containers.
- Fix bug where node state transition times were not updated in certain cases.
- Fix bug where very old nodes which have run hundreds of thousands of tasks
  could have a significant fraction of their overall disk space used by the
  Node Agent. In extreme cases their disk could even be filled up.
- Fix bug where `AZ_BATCH_JOB_PREP_DIR` was set to the wrong value.
  - This bug was a regression which caught early on in the rollout of
    1.3.0.1.
- Fix bug where resource file download could corrupt a file.
- Fix bug on Windows when running a multi-instance task with scope == 'task'
  which caused all subsequent multi-instance tasks in that scope
  (or any task with scope == 'pool') to fail, because we inadvertently
  deleted the pool scope user when the first task completed.

### 1.2.0.14
#### Released: 2018-04-09
- Fix bug where deleting a job in 'active' state would incorrectly skip
  running the job release task.
- Fix bug where the agent encounters an internal error but fails to
  yield the task back to Batch for scheduling on a different node,
  instead the agent just completed the task with a
  'schedulingError'/'failureInfo'.
- Fix bug where a task with 'maxRetries' == -1 didn't retry at all.
  The expected behavior if 'maxRetries' == -1 is for the task to retry
  forever.

### 1.2.0.12
#### Released: 2018-04-04
- Reduce Node Agent disk/CPU usage while the agent is idle or running a task
  and waiting for that task to exit.
- Improve 'docker pull' error handling and add retries for Node Agents running
  in container pools.
- Fix bug where container pools didn't do 'image get' before 'docker pull'
  in some cases.

### 1.2.0.9
#### Released: 2018-03-02
- Fix bug bootstrapping nvidia driver version.
- Fix bug where container pools did an unnecessary extra "GET" operation when
  pulling the container image.
- Fix bug which caused tasks to get stuck in "running" forever caused by
  incorrect read logic.

### 1.2.0.7
#### Released: 2018-02-15
- For container pools, attempt Docker service start if the Docker service is
  not already started on Windows.
- Improve error handling/logging for CreateProfile during task process
  execution on Windows.
- Add Debian 9 support.

### 1.2.0.4
#### Released: 2018-02-01
- Add support for Node Agent log file upload feature, which allows customers
  to upload the Azure Batch Node Agent internal log files to an Azure Storage
  container for later investigation by the product team.
- Fix bug where the Node Agent would fail to start properly in some
  sovereign clouds.

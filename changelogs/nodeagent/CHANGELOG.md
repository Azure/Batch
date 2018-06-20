### 1.3.0.2 - 2018-05-25
- Add Batch icon and file version info to Windows executables.
- Cap tasks to 95% CPU on single core VMs to prevent Azure infrastructure
  agents from starvation.
- Reduce node agent CPU/disk usage when spawning new tasks and running tasks.
- Update agent to support new `microsoft-azure-batch` published marketplace 
  images with support.
  - This includes using nvidia-docker2 in container pools.
- Fix bug where node state transition times were not updated in certain cases.
- Fix bug where very old nodes which have run hundreds of thousands of tasks
  could have a significant fraction of their overall disk space used by the
  node agent. In extreme cases their disk could even be filled up.
- Fix bug where AZ_BATCH_JOB_PREP_DIR was set to the wrong value.
  - This bug was a regression which caught early on in the rollout of 
    1.3.0.1.
- Fix bug where resource file download could corrupt a file.
- Fix bug on Windows when running a multi-instance task with scope == 'task'
  which caused all subsequent multi-instance tasks in that scope 
  (or any task with scope == 'pool') to fail, because we inadvertently 
  deleted the pool scope user when the first task completed.

### 1.2.0.14 - 2018-04-09
- Fix bug where deleting a job in 'active' state would incorrectly skip 
  running the job release task.
- Fix bug where the agent encounters an internal error but fails to 
  yield the task back to Batch for scheduling on a different node,
  instead the agent just completed the task with a 
  'schedulingError'/'failureInfo'.
- Fix bug where a task with 'maxRetries' == -1 didn't retry at all. 
  The expected behavior if 'maxRetries' == -1 is for the task to retry
  forever.

### 1.2.0.12 - 2018-04-04
- Reduce node agent disk/CPU usage while the agent is idle or running a task 
  and waiting for that task to exit.

### 1.2.0.11 - 2018-03-18
- Improve 'docker pull' error handling and add retries for node agents running 
  in container pools.
- Fix bug where container pools didn't do 'image get' before 'docker pull' 
  in some cases.

### 1.2.0.9 - 2018-03-02
- Fix bug bootstrapping nvidia driver version.
- Fix bug where container pools did an unnecessary extra "GET" operation when
  pulling the container image.
- Fix bug which caused tasks to get stuck in "running" forever caused by
  incorrect read logic.

### 1.2.0.6 - 2018-02-15
- For container pools, attempt Docker service start if the Docker service is 
  not already started on Windows.
- Improve error handling/logging for CreateProfile during task process
  execution on Windows.
- Add Debian 9 support.

### 1.2.0.4 - 2018-02-01
- Add support for node agent log file upload feature, which allows customers
  to upload the Azure Batch node agent internal log files to an Azure Storage 
  container for later investigation by the product team.
- Fix bug where the node agent would fail to start properly in some
  sovereign clouds.

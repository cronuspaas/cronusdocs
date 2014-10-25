Agent Configuration
===================

Agent configurations and default values, configurations can be override through API call

**Agent Configurations**

.. code-block:: bash

  agent_health_report_interval=120               # interval when agent health is checked
  agent_thread_timeout=600                       # agent thread timeout
  app_restart_init_delay=15                      # artificial delay before agent starts services after system reboot
  download_skip_prop=true                        # not require cronus prop file
  download_thread_attempt=1                      # download retry
  download_thread_max_time=7200                  # download timeout ceiling
  download_thread_min_progress_time=10           # download progress timeout
  download_thread_min_time=60                    # download timeout floor
  download_thread_rate_per_sec=102400            # download rate throttle
  exec_thread_progress_timeout=60                # task execution thread progress timeout
  exec_thread_sleep_time=1                       # interval when check task execution progress
  exec_thread_timeout=600                        # task execution thread timeout
  health_agent_file_descriptor_usage_threshold=500 # max number of file descriptor
  health_agent_mem_usage_threshold=200000        # max memory
  health_disk_usage_gc_buffer=20                 # agent disk percentage for gc
  health_disk_usage_gc_threshold=70              # agent disk percentage threshold for gc
  health_disk_usage_percent_threshold=90         # max disk usage percentage
  keep_past_manifests=3                          # how many past manifest to keep
  monitor_check_timeinterval=120                 # application monitoring interval
  monitor_probation_enabled=true                 # probation for application monitoring script
  monitor_publish_enabled=false                  # whether to publish application monitoring data
  packageMgr_garbage_freq=1800                   # agent gc interval in seconds
  packageMgr_install_package_age=43200           # agent package gc max age for installed package
  packageMgr_install_package_min_age=7200        # agent package gc min age for installed package (shorter than this is not subject to gc)
  packageMgr_package_age=86400                   # agent package gc max age for downloaded package
  packageMgr_package_min_age=7200                # agent package gc min age for downloaded package (shorter than this is not subject to gc)
  pkiauth_enabled=false                          # pki auth
  safeshutdown_busy_check_timeout=86400          # timeout checking for safe shutdown
  skip_cleanup_on_failure=false                  # skip cleanup when deploy application failure
  system_restart_time_threshold=300              # system uptime check for system reboot
  threadMgr_garbage_freq=60                      # agent thread gc interval
  threadMgr_thread_age=3600                      # agent thread age for gc


**Override Configuration**

.. code-block:: bash

  curl -k -X POST -d '{"configs": {"<configName>": "<configValue>"}}' https://host:12020/agent/config

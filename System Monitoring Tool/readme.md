# System Monitoring Tool

## Description
This Bash script provides a real-time monitoring tool for various system metrics, including:

- **CPU Usage**
- **Memory Usage**
- **Disk Usage**
- **Network Statistics**
- **System Uptime**

The script refreshes the displayed metrics every 5 seconds, providing a continuous overview of system performance.

---

## Features

1. **CPU Usage**
   - Calculates the percentage of CPU usage by parsing output from the `top` command.

2. **Memory Usage**
   - Displays memory usage as a percentage using the `free` command.

3. **Disk Usage**
   - Shows the percentage of disk usage for the root (`/`) directory.

4. **Network Statistics**
   - Retrieves incoming and outgoing network data from `/proc/net/dev` for the `eth0` interface.

5. **System Uptime**
   - Calculates and displays the system uptime in days, hours, and minutes.

---

## Code
```bash
#!/bin/bash

# Function to get CPU usage
get_cpu_usage() {
    cpu_usage=$(top -bn1 | grep "Cpu(s)" | \
                sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | \
                awk '{print 100 - $1"%"}')
    echo "CPU Usage: $cpu_usage"
}

# Function to get Memory usage
get_memory_usage() {
    memory_usage=$(free -m | awk 'NR==2{printf "Memory Usage: %.2f%%\n", $3*100/$2 }')
    echo "$memory_usage"
}

# Function to get Disk usage
get_disk_usage() {
    disk_usage=$(df -h | awk '$NF=="/"{printf "Disk Usage: %s\n", $5}')
    echo "$disk_usage"
}

# Function to get Network statistics
get_network_usage() {
    # Using /proc/net/dev to get network statistics
    network_usage=$(awk '/eth0/ {print "Network Usage: In: " $2/1024 " KB, Out: " $10/1024 " KB"}' /proc/net/dev)
    echo "$network_usage"
}

# Function to get System uptime
get_system_uptime() {
    system_start_time=$(uptime -s)
    current_time=$(date +%s)
    start_time=$(date -d "$system_start_time" +%s)
    uptime_seconds=$((current_time - start_time))
    uptime_days=$((uptime_seconds / 86400))
    uptime_hours=$(( (uptime_seconds % 86400) / 3600 ))
    uptime_minutes=$(( (uptime_seconds % 3600) / 60 ))
    echo "System Uptime: $uptime_days days, $uptime_hours hours, $uptime_minutes minutes"
}

# Function to display all metrics
display_metrics() {
    echo "========================================="
    echo "          System Monitoring Tool         "
    echo "========================================="
    get_cpu_usage
    get_memory_usage
    get_disk_usage
    get_network_usage
    get_system_uptime
    echo "========================================="
}

# Main loop to refresh metrics every 5 seconds
while true; do
    clear
    display_metrics
    echo "press ctrl+c to exit"
    sleep 5
done
```

---

## Usage

1. Save the script to a file, e.g., `system_monitor.sh`.
2. Make the script executable:
   ```bash
   chmod +x system_monitor.sh
   ```
3. Run the script:
   ```bash
   ./system_monitor.sh
   ```
4. Press `Ctrl+C` to exit the monitoring tool.

---

## Requirements

Ensure the following commands are available on your system:
- `top`
- `free`
- `df`
- `awk`
- `grep`
- `sed`
- `uptime`

---

## Notes
- The tool is designed for Linux-based systems.


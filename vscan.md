🛠️ Script Description

This script automates detailed service scanning with Nmap. Using an Nmap scan result file containing open ports, the script:

    🛡️ Extracts unique IP addresses from the scanned hosts.
    🔍 Identifies open ports and formats them appropriately for Nmap.
    🚀 Automatically runs a detailed scan (-sCV) for each IP and its open ports.
    📁 Saves results in organized files prefixed with versionScan_.

⚙️ How to Use

./script.sh <file>

📂 Where <file> is the Nmap scan result file containing open port information.
🚨 Requirements

    Superuser privileges (sudo) to run Nmap.
    A valid input file with Nmap scan results.

📝 Script Code

#!/bin/bash

# 🛑 Check if a file was provided as an argument
if [ -z "$1" ]; then
    echo "❌ Usage: $0 <file>"
    exit 1
fi

# 📂 File provided by the user
allTargets=$1

# 🛑 Verify if the file exists
if [ ! -f "$allTargets" ]; then
    echo "❌ The file $allTargets does not exist."
    exit 1
fi

# 🔍 Extract open ports and concatenate them with commas
ports=$(cat "$allTargets" | grep "Ports:" | grep -oP '\d+(?=/open)' | tr '\n' ',' | sed 's/,$//')

# 🛡️ Extract unique IPs
ips=$(cat "$allTargets" | grep "Host" | awk -F 'Host: ' '{print $2}' | awk '{print $1}' | uniq)

# 🛑 Check if ports and IPs are found
if [ -z "$ports" ] || [ -z "$ips" ]; then
    echo "⚠️ No open ports or IPs found in the file."
    exit 1
fi

# 🚀 Run the Nmap command for each IP
for ip in $ips; do
    echo "🔎 Running: sudo nmap -sCV -p$ports $ip -oN versionScan_$ip"
    sudo nmap -sCV -p"$ports" "$ip" -oN "versionScan_$ip"
done

🎉 Benefits

    Automates detailed service scanning (-sCV).
    Simplifies results management by saving each scan in a separate file.
    Saves time by efficiently processing multiple hosts.

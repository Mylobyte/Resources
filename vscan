#!/bin/bash

# Verifica si el archivo fue proporcionado como argumento
if [ -z "$1" ]; then
    echo "Uso: $0 <archivo>"
    exit 1
fi

# Archivo proporcionado por el usuario
allTargets=$1

# Verifica si el archivo existe
if [ ! -f "$allTargets" ]; then
    echo "El archivo $allTargets no existe."
    exit 1
fi

# Extrae los puertos abiertos y los concatena con comas
ports=$(cat "$allTargets" | grep "Ports:" | grep -oP '\d+(?=/open)' | tr '\n' ',' | sed 's/,$//')

# Extrae las IPs únicas
ips=$(cat "$allTargets" | grep "Host" | awk -F 'Host: ' '{print $2}' | awk '{print $1}' | uniq)

# Verifica si hay puertos e IPs
if [ -z "$ports" ] || [ -z "$ips" ]; then
    echo "No se encontraron puertos abiertos o IPs en el archivo."
    exit 1
fi

# Ejecuta el comando nmap para cada IP
for ip in $ips; do
    echo "Ejecutando: sudo nmap -sCV -p$ports $ip -oN versionScan_$ip"
    sudo nmap -sCV -p"$ports" "$ip" -oN "versionScan_$ip"
done

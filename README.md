#!/bin/bash

for list in ${MAPPING[@]}
do
	HOST_IP=`echo "$list" | cut -f 1 -d','`
    echo "--------------------- $HOST_IP ------------------------"

    sshpass -p "$SERVER_PASSWORD" ssh -o ConnectTimeout=10 -o StrictHostKeyChecking=no $SERVER_USER@"$HOST_IP" "/usr/bin/go-adb listdevices | jq -r '.devicelist[].SerialNumber' | wc -l"

    sshpass -p "$SERVER_PASSWORD" ssh -o StrictHostKeyChecking=no $SERVER_USER@"$HOST_IP" "./Documents/devops_scripts/resetusb.sh"

    sshpass -p "$SERVER_PASSWORD" ssh -o ConnectTimeout=10 -o StrictHostKeyChecking=no $SERVER_USER@"$HOST_IP" "/usr/bin/go-adb listdevices | jq -r '.devicelist[].SerialNumber' | wc -l"


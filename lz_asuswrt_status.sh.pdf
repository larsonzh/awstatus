#!/bin/sh
# lz_asuswrt_status.sh v1.1.1
# By LZ 妙妙呜 (larsonzhang@gmail.com)

LZ_VERSION=v1.1.1

## 路由器WIFI无线网络操作wl指令名称
WL=wl

## 路由器WIFI无线网络操作wl指令可执行文件路径
WL_PATH=/usr/sbin

echo
echo $(date) [$$]
echo
echo LZ $LZ_VERSION script commands start......
echo By LZ \(larsonzhang@gmail.com\)
echo

Route_Model=$( nvram get productid 2> /dev/null | sed -n 1p )
Hardware_Type=$( uname -m 2> /dev/null | sed -n 1p )
Host_Name=$( uname -n 2> /dev/null | sed -n 1p )
Kernel_Name=$( uname 2> /dev/null | sed -n 1p )
Kernel_Release=$( uname -r 2> /dev/null | sed -n 1p )
Kernel_Version=$( uname -v 2> /dev/null | sed -n 1p )
OS_Name=$( uname -o 2> /dev/null | sed -n 1p )
[ -z "$Route_Model" ] && Route_Model=unknown
[ -z "$Hardware_Type" ] && Hardware_Type=unknown
[ -z "$Host_Name" ] && Host_Name=unknown
[ -z "$Kernel_Name" ] && Kernel_Name=unknown
[ -z "$Kernel_Release" ] && Kernel_Release=unknown
[ -z "$Kernel_Version" ] && Kernel_Version=unknown
[ -z "$OS_Name" ] && OS_Name=unknown
echo -e Route Model\\t: "$Route_Model"
echo -e Hardware Type\\t: "$Hardware_Type"
echo -e Host Name\\t: "$Host_Name"
echo -e Kernel Name\\t: "$Kernel_Name"
echo -e Kernel Release\\t: "$Kernel_Release"
echo -e Kernel Version\\t: "$Kernel_Version"
echo -e OS Name\\t\\t: "$OS_Name"

if [ "$( uname -o 2> /dev/null | sed -n 1p )" = "Merlin-Koolshare" ]; then
	echo Firmware Version: "$( nvram get extendno 2> /dev/null | cut -d "X" -f2 | cut -d "-" -f1 | cut -d "_" -f1 | sed -n 1p )"
else
	firmware_version=$( nvram get firmver 2> /dev/null | sed -n 1p )
	[ -n "$firmware_version" ] && {
		firmware_buildno=$( nvram get buildno 2> /dev/null | sed -n 1p )
		[ -n "$firmware_buildno" ] && {
			firmware_webs_state_info=$( nvram get webs_state_info 2> /dev/null | sed 's/^[^_]*[_]/&LZZL/' | sed 's/[_]LZZL/\./' | sed -n 1p )
			if [ -z "$firmware_webs_state_info" ]; then
				firmware_webs_state_info_beta=$( nvram get webs_state_info_beta 2> /dev/null | sed 's/^[^_]*[_]/&LZZL/' | sed 's/[_]LZZL/\./' | sed -n 1p )
				if [ -z "$firmware_webs_state_info_beta" ]; then
					firmware_version="$firmware_version.$firmware_buildno"
				else
					firmware_version="$firmware_version.$firmware_webs_state_info_beta"
				fi
			else
				firmware_version="$firmware_version.$firmware_webs_state_info"
			fi
			[ -z "$firmware_version" ] && firmware_version=unknown
			echo Firmware Version: "$firmware_version"
		}
	}
fi

Firmware_Build=$( nvram get buildinfo 2> /dev/null | sed -n 1p )
[ -n "$Firmware_Build" ] && echo -e Firmware Build\\t: "$Firmware_Build"

Bootloader_CFE=$( nvram get bl_version 2> /dev/null | sed -n 1p )
[ -n "$Bootloader_CFE" ] && echo -e Bootloader \(CFE\): "$Bootloader_CFE"

route_local_info="$( ifconfig br0 2> /dev/null )"
if [ -n "$route_local_info" ]; then
	Route_Status=$( echo $route_local_info | awk -F " " '{print $2}' | sed -n 1p )
	Route_Encap=$( echo $route_local_info | awk -F " " '{print $3}' | awk -F ":" '{print $2}' | sed -n 1p )
	Route_HWaddr=$( echo $route_local_info | awk -F " " '{print $5}' | sed -n 1p )
	Route_IP_Addr=$( echo $route_local_info | awk -F " " '{print $7}' | awk -F ":" '{print $2}' | sed -n 1p )
	Local_Bcast_Addr=$( echo $route_local_info | awk -F " " '{print $8}' | awk -F ":" '{print $2}' | sed -n 1p )
	Local_Net_Mask=$( echo $route_local_info | awk -F " " '{print $9}' | awk -F ":" '{print $2}' | sed -n 1p )
	echo
	[ -z "$Route_Status" ] && Route_Status=unknown
	[ -z "$Route_Encap" ] && Route_Encap=unknown
	[ -z "$Route_HWaddr" ] && Route_HWaddr=unknown
	[ -z "$Route_IP_Addr" ] && Route_IP_Addr=unknown
	[ -z "$Local_Bcast_Addr" ] && Local_Bcast_Addr=unknown
	[ -z "$Local_Net_Mask" ] && Local_Net_Mask=unknown
	echo -e Route Status\\t: "$Route_Status"
	echo -e Route Encap\\t: "$Route_Encap"
	echo -e Route HWaddr\\t: "$Route_HWaddr"
	echo -e Route IP Addr\\t: "$Route_IP_Addr"
	echo Local Bcast Addr: "$Local_Bcast_Addr"
	echo -e Local Net Mask\\t: "$Local_Net_Mask"
fi

echo
cat /proc/cpuinfo 2> /dev/null

CPU_frequency=$( nvram get clkfreq 2> /dev/null | sed -n 1p | awk -F ',' '{print $1}' )
Memory_frequency=$( nvram get clkfreq 2> /dev/null | sed -n 1p | awk -F ',' '{print $2}' )
if [ -n "$CPU_frequency" -a -n "$Memory_frequency" ]; then
	CPU_frequency="$CPU_frequency MHz"
	Memory_frequency="$Memory_frequency MHz"
	echo
	echo -e CPU frequency\\t: "$CPU_frequency"
	echo Memory frequency: "$Memory_frequency"
fi

echo
CPU_temperature=$( cat /proc/dmu/temperature 2> /dev/null | sed -e 's/.C$/°C/g' | sed -e '/^$/d' | sed -n 1p )
if [ -z "$CPU_temperature" ]; then
	CPU_temperature=$( cat /sys/class/thermal/thermal_zone0/temp 2> /dev/null | awk '{print $1 / 1000}' | sed -n 1p )
	[ -n "$CPU_temperature" ] && CPU_temperature="$CPU_temperature"°C || CPU_temperature=unknown
	echo -e CPU temperature\\t: "$CPU_temperature"
else
	echo "$CPU_temperature"
fi

if [ -z "$( which ${WL} 2> /dev/null )" ]; then
	WL=${WL_PATH}/${WL}
elif [ "$( which ${WL} 2> /dev/null )" != "${WL_PATH}/${WL}" -a "${WL_PATH}" != "/usr/sbin" ]; then
	WL=${WL_PATH}/${WL}
fi

interface_2g=$( nvram get wl0_ifname 2> /dev/null | sed -n 1p )
interface_5g1=$( nvram get wl1_ifname 2> /dev/null | sed -n 1p )
interface_5g2=$( nvram get wl2_ifname 2> /dev/null | sed -n 1p )

interface_2g_temperature= ; interface_5g1_temperature= ; interface_5g2_temperature= ;

[ -n "$interface_2g" ] && interface_2g_temperature=$( ${WL} -i $interface_2g phy_tempsense 2> /dev/null | awk '{print $1 / 2 + 20}' | sed -n 1p )
[ -n "$interface_2g_temperature" ] && interface_2g_temperature="$interface_2g_temperature"°C || interface_2g_temperature="offline or unknown"

[ -n "$interface_5g1" ] && interface_5g1_temperature=$( ${WL} -i $interface_5g1 phy_tempsense 2> /dev/null | awk '{print $1 / 2 + 20}' | sed -n 1p )
[ -n "$interface_5g1_temperature" ] && interface_5g1_temperature="$interface_5g1_temperature"°C || interface_5g1_temperature="offline or unknown"

[ -n "$interface_5g2" ] && {
	interface_5g2_temperature=$( ${WL} -i $interface_5g2 phy_tempsense 2> /dev/null | awk '{print $1 / 2 + 20}' | sed -n 1p )
	[ -n "$interface_5g2_temperature" ] && interface_5g2_temperature="$interface_5g2_temperature"°C || interface_5g2_temperature="offline or unknown"
}

wl_txpwr_2g="offline or unknown" ; wl_txpwr_5g1="$wl_txpwr_2g" ; wl_txpwr_5g2="$wl_txpwr_2g" ;

[ -n "$interface_2g" ] && {
	interface_2g_power="$( ${WL} -i $interface_2g txpwr_target_max 2> /dev/null | awk '{print $NF}' | sed -n 1p )"
	interface_2g_power_max="$( ${WL} -i $interface_2g txpwr1 2> /dev/null | sed -n 1p | awk '{print $5,$7}' | sed -e 's/\(^.*\)[ ]\(.*$\)/\t( \1 dBm \/ \2 mW )/g' )"
	[ -n "$interface_2g_power" ] && wl_txpwr_2g="$interface_2g_power dBm / $( awk -v x=$interface_2g_power 'BEGIN {printf "%.2f\n", 10^(x/10)}' ) mW$interface_2g_power_max"
}

[ -n "$interface_5g1" ] && {
	interface_5g1_power="$( ${WL} -i $interface_5g1 txpwr_target_max 2> /dev/null | awk '{print $NF}' | sed -n 1p )"
	interface_5g1_power_max="$( ${WL} -i $interface_5g1 txpwr1 2> /dev/null | sed -n 1p | awk '{print $5,$7}' | sed -e 's/\(^.*\)[ ]\(.*$\)/\t( \1 dBm \/ \2 mW )/g' )"
	[ -n "$interface_5g1_power" ] && wl_txpwr_5g1="$interface_5g1_power dBm / $( awk -v x=$interface_5g1_power 'BEGIN {printf "%.2f\n", 10^(x/10)}' ) mW$interface_5g1_power_max"
}

[ -n "$interface_5g2" ] && {
	interface_5g2_power="$( ${WL} -i $interface_5g2 txpwr_target_max 2> /dev/null | awk '{print $NF}' | sed -n 1p )"
	interface_5g2_power_max="$( ${WL} -i $interface_5g2 txpwr1 2> /dev/null | sed -n 1p | awk '{print $5,$7}' | sed -e 's/\(^.*\)[ ]\(.*$\)/\t( \1 dBm \/ \2 mW )/g' )"
	[ -n "$interface_5g2_power" ] && wl_txpwr_5g2="$interface_5g2_power dBm / $( awk -v x=$interface_5g2_power 'BEGIN {printf "%.2f\n", 10^(x/10)}' ) mW$interface_5g2_power_max"
}

if [ -z "$interface_5g2" ]; then
	echo -e 2.4 GHz\\t\\t: "$interface_2g_temperature"
	echo -e 5 GHz\\t\\t: "$interface_5g1_temperature"
	echo
	echo 2.4 GHz Tx Power: "$wl_txpwr_2g"
	echo -e 5 GHz Tx Power\\t: "$wl_txpwr_5g1"
else
	echo -e 2.4 GHz\\t\\t: "$interface_2g_temperature"
	echo -e 5 GHz-1\\t\\t: "$interface_5g1_temperature"
	echo -e 5 GHz-2\\t\\t: "$interface_5g2_temperature"
	echo
	echo 2.4 GHz Tx Power: "$wl_txpwr_2g"
	echo 5 GHz-1 Tx Power: "$wl_txpwr_5g1"
	echo 5 GHz-2 Tx Power: "$wl_txpwr_5g2"
fi

if [ -n "$( which ethctl 2> /dev/null )" ]; then
	eth_x_set=$( ifconfig 2> /dev/null | grep -E 'eth[0-9]{1,10}' | awk '{print $1}' | sort -n )
	if [ -n "$eth_x_set" ]; then
		x=0
		until [ -z "$( echo $eth_x_set | grep -o eth$x | sed -n 1p )" ]
		do
			ethx_info=$( ethctl eth$x media-type 2> /dev/null )
			if [ -n "$ethx_info" ]; then
				echo
				echo Ethernet Port $x Status:
				echo "$ethx_info"
			fi
			let x++
		done
	fi
elif [ -n "$( which robocfg 2> /dev/null )" ]; then
	echo
	robocfg show 2> /dev/null
fi

nvram show 1> /dev/null 2> /tmp/lz_nvram.size
if [ -f /tmp/lz_nvram.size ]; then
	NVRAM_usage=$( cat /tmp/lz_nvram.size 2> /dev/null \
				| grep -Eio 'size: [0-9]{1,10} bytes \([0-9]{1,10} left\)' \
				| sed -e 's/^.*[ ]\([0-9]\{1,10\}\)[ ].*[ ](\([0-9]\{1,10\}\).*$/\1 \2/g' \
				| awk '{print $1, $1 + $2}' \
				| sed -e 's/\(^[0-9]\{1,10\}\) \([0-9]\{1,10\}\)/\1 \/ \2 bytes/g' \
				| sed -n 1p )
	if [ -n "$NVRAM_usage" ]; then
		echo
		echo -e NVRAM usage\\t: "$NVRAM_usage"
	fi
	rm /tmp/lz_nvram.size > /dev/null 2>&1
fi

<<EOF
NVRAM_usage=$( nvram show 2>&1 \
			| grep -Eio 'size: [0-9]{1,10} bytes \([0-9]{1,10} left\)' \
			| sed -e 's/^.*[ ]\([0-9]\{1,10\}\)[ ].*[ ](\([0-9]\{1,10\}\).*$/\1 \2/g' \
			| awk '{print $1, $1 + $2}' \
			| sed -e 's/\(^[0-9]\{1,10\}\) \([0-9]\{1,10\}\)/\1 \/ \2 bytes/g' \
			| sed -n 1p )
if [ -n "$NVRAM_usage" ]; then
	echo
	echo -e NVRAM usage\\t: "$NVRAM_usage"
fi
EOF

echo
free 2> /dev/null

echo
NAND_bad_block=$( dmesg 2> /dev/null | grep nand_read_bbt | grep "bad block" )
if [ -n "$NAND_bad_block" ]; then
	echo "$NAND_bad_block"
	echo Number of NAND bad blocks: "$( echo "$NAND_bad_block" | grep -c '^.*$' )"
else
	echo No bad blocks found in NAND.
fi

echo
cat /proc/mtd 2> /dev/null
echo
cat /proc/partitions 2> /dev/null
echo
df -hT 2> /dev/null
echo
brctl show 2> /dev/null
echo
cat /proc/net/vlan/config 2> /dev/null

if [ -n "$( which bcmmcastctl 2> /dev/null )" ]; then
	echo
	bcmmcastctl show 2> /dev/null
else
	if [ -n "$( ps 2> /dev/null | grep igmpproxy | grep -v grep | sed -n 1p )" ]; then
		echo
		ps 2> /dev/null | grep igmpproxy | grep -v grep
		cat "$( ps 2> /dev/null | grep igmpproxy | grep -v grep | sed -n 1p | awk '{print $6}' )" 2> /dev/null
	fi
fi

if [ -n "$( ps 2> /dev/null | grep udpxy | grep -v grep | sed -n 1p )" ]; then
	echo
	ps 2> /dev/null | grep udpxy | grep -v grep
fi

echo
ip route show 2> /dev/null

if [ -n "$( ip route show 2> /dev/null | grep nexthop | sed -n 1p )" ]; then
	echo
	ip route show table 100 2> /dev/null
	echo
	ip route show table 200 2> /dev/null
fi

echo
echo LZ $LZ_VERSION script commands executed!
echo
echo $(date) [$$]
echo

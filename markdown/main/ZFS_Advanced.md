<!-- source: https://wiki.gentoo.org/wiki/ZFS/Advanced | group: Gentoo Wiki (Main) | wiki-title: ZFS/Advanced -->
---
title: ZFS/Advanced
url: https://wiki.gentoo.org/wiki/ZFS/Advanced
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-01"
fingerprint: "3b170c228ae989ca"
license: CC BY-SA 4.0
---

# ZFS/Advanced

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Overview

This section delves into advanced topics for ZFS users, encompassing a wide range of sophisticated functionalities and optimization techniques. Users will discover in-depth coverage of automation scripts that streamline routine tasks and system maintenance, along with comprehensive ZFS tuning strategies for maximizing performance across different workload scenarios. The topics include advanced dataset management, custom scripting for monitoring and maintenance, performance optimization through ARC and ZIL tuning, sophisticated backup and restoration procedures, and enterprise-level high availability configurations. Additionally, this section explores advanced encryption methodologies, RAID-Z expansion techniques, special VDEV optimization, and container integration strategies. Whether managing a home server or enterprise storage infrastructure, these advanced topics will help users leverage ZFS's full potential through proper system integration, automated maintenance, and performance monitoring. Particular attention is given to real-world scenarios and practical implementations, ensuring that users can effectively apply these advanced concepts to their specific use cases.

## Automation Scripts

**Abstraction:** Manually updating the ZFS kernel module can be a time-consuming and error-prone process that involves:

- Monitoring the OpenZFS GitHub releases page for updates
- Checking kernel compatibility for each new release
- Downloading and installing the appropriate versions
- Ensuring system consistency between the ZFS module and kernel versions

**System Requirements**:
Make sure you have the following prerequisites:

- Linux system with bash shell
- curl installed for downloading release information
- Internet connection to access GitHub
- Sufficient permissions to install kernel modules and updates
- Directory with appropriate write permissions for script operation

The scripts below will be triggered by one script called system\_update.sh. It should be executed each time the user updates their system. If there are no new updates based on the provided arguments, the script will exit. This is to ensure that the script checks for any new kernel versions, preferably after a post-sync. Create your hook script in postsync.d

`root #````
sudo nano -w /etc/portage/postsync.d/zfs-check
```
**`/etc/portage/postsync.d/zfs-check`**

**Setting Up eix post sync**

```
#!/bin/bash
/etc/hooks.d/zfs/system_update.sh stable 0 "/home/masterkuckles"
```
Now make it executable:

`root #````
chmod +x /etc/portage/postsync.d/zfs-check
```
The script will install the latest available kernel version from the portage tree and remove any outdated kernels. If an error occurs during kernel installation, compilation, or while installing the new ZFS module, the script will catch it and exit. The script’s functionality depends on the arguments provided.

**`/etc/hooks.d/zfs/system_update.sh`**

**System Update**

```
#!/bin/bash
source ./compatibility.sh 
TYPE=$(uname -m)
KEY=""
ZFS_KEY=""
STATUS="$1"
CONFIG_PATH="$3"
# Function that will check to see if there is an update or not
check_kernel() {
    # Assuming that user merged kernel source from portage
    local pkg=$(cat /var/lib/portage/world | grep -E '^sys-kernel\/[[:alpha:]]+-[[:alpha:]]+:[0-9]+\.[0-9]+\.[0-9]+$')
    if echo $pkg | grep -Eq 'gentoo-sources'; then
        # Can't assign an array to the map. Therefore, make it a string
        # and convert it back into an array using mapfile
        INSTALLED_KERNELS["gentoo-sources"]=$pkg
        KEY="gentoo-sources"
        # TODO Need to find the most latest stable version and increment it by one
        touch /etc/portage/package.mask/gentoo-sources
        #echo >=
    else
        if echo $pkg | grep -Eq 'gentoo-kernel-bin'; then
            INSTALLED_KERNELS["gentoo-kernel-bin"]=$pkg
            KEY="gentoo-kernel-bin"
            # TODO Need to find the most latest stable version and increment it by one
            touch /etc/portage/package.mask/gentoo-kernel-bin
            #echo >=
        else
            system_update_bug_report
            exit 0
        fi 
    fi
    return 0
}
# Function that will check and see if there is an update for zfs
check_zfs() {
    # Re-Update the installed kernel map
    check_kernel
    curl -s https://packages.gentoo.org/packages/sys-fs/zfs > gentoo_zfs_sources.html
    local line=$(grep -oP 'title="\d+\.\d+\.\d+ [^"]+"' gentoo_zfs_sources.html)
    local installed_zfs=$(cat /etc/portage/package.mask/zfs | sed 's/^[^a-zA-Z]*//;s/[[:space:]]*$//')
    installed_zfs=$(echo "$installed_zfs" | sed 's/[[:space:]]*$//')
    # Create an array that holds zfs and zfs-kmod package versions
    mapfile -t CURRENT_ZFS <<< "$(echo "$installed_zfs" | tr ' ' '\n')"
    local current=${CURRENT_ZFS[0]}
    while read -d '"' -r chunk; do
        if [[ $chunk =~ ^title= ]]; then
            continue  # Skip the "title=" part
        fi
        if [[ $chunk =~ ^([0-9]+\.[0-9]+\.[0-9]+)[[:space:]]is[[:space:]](testing|stable|unknown)[[:space:]]on[[:space:]]([a-z0-9]+)$ ]]; then
            version="${BASH_REMATCH[1]}"
            status="${BASH_REMATCH[2]}"
            architecture="${BASH_REMATCH[3]}"
            # Initialize if empty
            ZFS_DICT[$architecture]="${ZFS_DICT[$architecture]:-}"
            # Append new value
            ZFS_DICT[$architecture]+="$status:$version "
        fi
    done < <(echo "$line")
    local zfs_releases=""
    if [ "$TYPE" == "x86_64" ]; then
        zfs_releases=${ZFS_DICT["amd64"]}
    else 
        zfs_releases=${ZFS_DICT[$TYPE]}
    fi
    IFS=' ' read -r -a lines <<< "$zfs_releases"
    for line in "${lines[@]}"; do
        IFS=':' read -r status_val version_val <<< "$line"
        if [[ "$DEBUG" -eq 1 ]]; then
            echo "Printing out Versions:"
            echo $status_val
            echo "====================================="
 
        fi
        # Note: Issue could occur here if the DOM tree structure changes the versions around
        if [ "$STATUS" == "$status_val" ]; then 
            if [ ! -n "$ZFS_KEY" ]; then
                ZFS_KEY="zfs-$version_val"
            fi
            if [ ! -n "$current" ]; then
                current="sys-fs/zfs-$version_val"
            fi
            AVAILABLE_ZFS["$status_val"]+="sys-fs/zfs-$version_val "
        fi
    done
    # Check to see if the user has the latest version
    local new_zfs=${AVAILABLE_ZFS["$STATUS"]}
    local usr_version=$(echo "$current" | sed 's/.*-\(.*\)/\1/')
    IFS=' ' read -r -a zfs <<< "$new_zfs"
    for ele in "${zfs[@]}"; do
        available_version=$(echo "$ele" | sed 's/^[^:]*://')
        if [[ "$(echo -e "$usr_version\n$available_version" | sort -V | head -n 1)" == "$usr_version" && "$usr_version" != "$available_version" ]]; then
            echo "A zfs update is available! Going to update"
            if [ -d "/etc/portage/package.mask" ]; then 
                rm /etc/portage/package.mask/zfs
                echo ">=sys-fs/$ZFS_KEY" >> /etc/portage/package.mask/zfs
                echo ">=sys-fs/zfs-kmod-$available_version" >> /etc/portage/package.mask/zfs
            else
                echo ">=sys-fs/$ZFS_KEY" >> /etc/portage/package.mask
                echo ">=sys-fs/zfs-kmod-$available_version" >> /etc/portage/package.mask
            fi
            return 0
        else 
            echo "System has the latest version already installed!"
            exit 0
        fi
    done
    return 0
}
# Function that will install the new kernel 
install_kernel() {
    local usr_kernel_str=${INSTALLED_KERNELS[$KEY]}
    local output=0
    local copy_config=0
    for build in "${BUILD_KERNELS[@]}"; do
        version=$(echo "$build" | sed 's/^[^-]*-//')
        eselect kernel set $build
        cd /usr/src/linux || error "install_kernel" "Folder /usr/src/linux does not exist!"
        # Note: config file needs to be copied somewhere else other than home
        if [ "$copy_config" -ne 1 ]; then
            config=$(ls $CONFIG_PATH/*-config | head -n 1)
            cp -Prv $config ./.config
            copy_config=1
        fi
        if [ ! -d "/lib/modules/$version-$TYPE" ]; then 
            make menuconfig
            make -j3 || error "install_kernel" "Failed to compile" && \
            make modules_install || error "install_kernel" "Failed to install modules" && \
            make install || error "install_kernel" "Failed to install kernel"
            cp -Prv arch/x86/boot/bzImage /boot/"vmlinuz-$version-$TYPE"
        fi
        if [ -d "/usr/src/initramfs" ]; then
            new_path="$version-$TYPE"
            if [[ "$output" -eq 0 ]]; then
                echo "====================================="
                echo "Creating the directories in /usr/src/initramfs!"
                output=1
            fi
            if [ -d "/usr/src/initramfs/lib" ] && [ -d "/usr/src/initramfs/lib/modules" ]; then
                if [ ! -d "/usr/src/initramfs/lib/modules/$new_path" ]; then  
                    mkdir -vp /usr/src/initramfs/lib/modules/$new_path
                fi
            else
                if [ ! -d  "/usr/src/initramfs/lib" ]; then
                    mkdir -vp /usr/src/initramfs/lib 
                fi
                if [ ! -d "/usr/src/initramfs/lib/modules" ]; then 
                    mkdir -vp /usr/src/initramfs/lib/modules
                fi
                if [ ! -d "/usr/src/initramfs/lib/modules/$new_path" ]; then 
                    mkdir -vp /usr/src/initramfs/lib/modules/$new_path
                fi
            fi
            if [ ! -d "/usr/src/initramfs/lib/modules/$new_path/extra" ]; then
                echo "Creating the extra folder to copy over the modules!"
                mkdir -vp /usr/src/initramfs/lib/modules/$new_path/extra
            fi
        fi
    done
    mapfile -t usr_kernel_arr <<< "$usr_kernel_str"
    local clean_up=0
    for usr_kernels in "${usr_kernel_arr[@]}"; do
        if [ "$clean_up" -ne 0 ]; then
            echo "====================================="
            echo "Cleaning up old directories in /usr/src/initramfs/lib/modules!"
            echo "====================================="
            clean_up=1
        fi
        # Strip off the package name and colon to extract just the version
        usr_version=$(echo "$usr_kernels" | sed 's/^[^:]*://')
        if [ -d "/usr/src/initramfs/$usr_version-gentoo-$TYPE" ]; then
            if [ -d "/usr/src/initramfs/lib/modules/$usr_version-gentoo-$TYPE" ]; then 
                echo "====================================="
                echo "Removing old folders from /usr/src/initramfs/modules/$usr_version-gentoo-$TYPE"
                rm -r /usr/src/initramfs/lib/modules/"$usr_version-gentoo-$TYPE" || error "install_kernel" "Failed to remove $usr_version-gentoo-$TYPE from /usr/src/initramfs/lib/modules/"
            fi
            if [ -d "/lib/modules/$usr_version-gentoo-$TYPE" ]; then 
                echo "====================================="
                echo "Removing old folders from /lib/modules/$usr_version-gentoo-$TYPE"
                rm -r /lib/modules/"$usr_version-gentoo-$TYPE" || error "install_kernel" "Failed to remove $usr_version-gentoo-$TYPE from /lib/modules/"
            fi 
            if [ -d "/boot/vmlinuz-$usr_version-gentoo-$TYPE" ]; then 
                echo "====================================="
                echo "Removing the old vmlinuz from /boot/vmlinuz-$usr_version-gentoo-$TYPE"
                rm  /boot/"vmlinuz-$usr_version-gentoo-$TYPE" || error "install_kernel" "Failed to remove vmlinuz-$usr_version-$TYPE from /boot"
            fi 
            if [ -d "/boot/initramfs-$usr_version-$TYPE.img" ]; then 
                echo "====================================="
                echo "Removing the old initramfs from /boot/initramfs-$usr_version-$TYPE.img"
                rm /boot/"initramfs-$usr_version-$TYPE.img" || error "install_kernel" "Failed to remove initramfs-$usr_version-$TYPE.img from /boot"
            fi 
        else 
            if [ -d "/lib/modules/$usr_version-gentoo-$TYPE" ]; then 
                echo "====================================="
                echo "Removing old folders from /lib/modules/$usr_version-gentoo-$TYPE"
                rm -r /lib/modules/"$usr_version-gentoo-$TYPE" || error "install_kernel" "Failed to remove $usr_version-gentoo-$TYPE from /lib/modules/"
            fi 
            if [ -d "/boot/vmlinuz-$usr_version-gentoo-$TYPE" ]; then 
                echo "====================================="
                echo "Removing the old vmlinuz from /boot/vmlinuz-$usr_version-gentoo-$TYPE"
                rm  /boot/"vmlinuz-$usr_version-gentoo-$TYPE" || error "install_kernel" "Failed to remove vmlinuz-$usr_version-$TYPE from /boot"
            fi 
            if [ -d "/boot/initramfs-$usr_version-$TYPE.img" ]; then 
                echo "====================================="
                echo "Removing the old initramfs from /boot/initramfs-$usr_version-$TYPE.img"
                rm /boot/"initramfs-$usr_version-$TYPE.img" || error "install_kernel" "Failed to remove initramfs-$usr_version-$TYPE.img from /boot"
            fi 
        fi
    done
    return 0
}
# Function will only install the specific version based on the argument that is passed in before run time
install_kernel_resources() {
    local usr_kernel_str=${INSTALLED_KERNELS[$KEY]}
    local available_kernel_str=""
    local build_str=""
    if [ "$TYPE" == "x86_64" ]; then
        available_kernel_str=${ARCH_DICT["amd64"]}
    else 
        available_kernel_str=${ARCH_DICT[$TYPE]}
    fi
    if [ ! -n "$available_kernel_str" ] || [ ! -n "$usr_kernel_str" ]; then
        system_update_bug_report
        exit 0
    fi
    # Split the lines at white-spaces
    IFS=' ' read -r -a lines <<< "$available_kernel_str"
    for line in "${lines[@]}"; do
        IFS=':' read -r status_val version_val <<< "$line"
        if [[ "$DEBUG" -eq 1 ]]; then
            echo "Printing out Versions:"
            echo $status_val
            echo "====================================="
        fi
        if [ "$STATUS" == "$status_val" ]; then 
            AVAILABLE_KERNELS["$status_val"]+="sys-kernel/$KEY:$version_val "
        fi
    done
    mapfile -t usr_kernel_arr <<< "$usr_kernel_str"
    IFS=' ' read -r -a kernel_arr <<< "${AVAILABLE_KERNELS["$STATUS"]}"
    local copy_config=0
    local kernel_update=0
    for usr_kernels in "${usr_kernel_arr[@]}"; do
        # Strip off the package name and colon to extract just the version
        usr_version=$(echo "$usr_kernels" | sed 's/^[^:]*://')
        if [ ! -n "$usr_version" ]; then
            system_update_bug_report
            exit 0
        fi
        for available_kernels in "${kernel_arr[@]}"; do
            # Strip off the package name and colon to extract just the version
            available_version=$(echo "$available_kernels" | sed 's/^[^:]*://')
            if [ ! -n "$available_kernels" ]; then
                system_update_bug_report
                exit 0
            fi
            if [[ "$(echo -e "$usr_version\n$available_version" | sort -V | head -n 1)" == "$usr_version" && "$usr_version" != "$available_version" ]]; then
                if [[ ! "$build_str" =~ "linux-$available_version-gentoo" ]]; then
                    build_str+="linux-$available_version-gentoo "
                fi
                # Note: config file needs to be copied somewhere else other than home
                if [ "$copy_config" -ne 1 ]; then
                    copy_config=1
                    kernel_update=1
                    cp -Prv /usr/src/"linux-$usr_version-gentoo"/.config $CONFIG_PATH/"$usr_version-gentoo-$TYPE-config"
                fi
                if ! equery list =sys-kernel/"$KEY-$available_version" > /dev/null; then 
                    emerge -v =sys-kernel/"$KEY-$available_version" || error "install_kernel_resources" "Failed to install the new kernel!"
                    emerge --deselect =sys-kernel/"$KEY-$available_version" || error "install_kernel_resources" "Failed to deselect the old kernels"
                    if [ -d "/usr/src/linux-$available_version-gentoo" ]; then 
                        rm -r  /usr/src/"linux-$available_version-gentoo"
                    fi
		        else 
			        echo "Kernel $KEY-available_version is already installed"
		        fi
            fi
        done 
    done
    if [ "$kernel_update" -ne 0 ]; then
        # Remove trailing spaces and newlines
        build_str=$(echo "$build_str" | sed 's/[[:space:]]*$//')
        mapfile -d ' ' -t BUILD_KERNELS <<< "$build_str"
        install_kernel
        emerge -a --depclean
    else 
        echo "====================================="
        echo "Already have the latest kernel version installed!"
        echo "Exiting script..."
        echo "====================================="
        clean_up
        exit 0
    fi
    return 0
}
# Function that will update to the newest zfs
install_zfs() {
    local compatible_kernel_ranges=${COMPATIBLE_RELEASES[$ZFS_KEY]}
    local installed_kernels_str=${INSTALLED_KERNELS[$KEY]}
    local start=$(echo "$compatible_kernel_ranges" | cut -d '-' -f1)
    local end=$(echo "$compatible_kernel_ranges" | sed 's/^[^-]*-//')
    mapfile -t kernels <<< "$installed_kernels_str"
    for ele in "${kernels[@]}"; do
        # Strip off the package name and colon to extract just the version
        version=$(echo "$ele" | sed 's/^[^:]*://')
        if version_greater_than_equal "$version" "$start" && version_less_than_equal "$version" "$end"; then
            new_path="$version-$TYPE"
            zfs_version=$(echo $ZFS_KEY | sed 's/^[^-]*-//')
            echo "Kernel version $kernel is within the range ($start - $end)."
            emerge --deselect ${CURRENT_ZFS[0]} || error "install_zfs" "Failed to uninstall the old zfs"
            emerge --deselect ${CURRENT_ZFS[1]} || error "install_zfs" "Failed to uninstall the old zfs kmod"
            emerge -1 =sys-fs/$ZFS_KEY || error "install_zfs" "Failed to install the newer zfs!"
            emerge -1 =sys-fs/zfs"-"kmod"-"$zfs_version || error "install_zfs" "Failed to install the new zfs-kmod!"
            if [ -d "/usr/src/initramfs" ]; then 
                cp -Prv /lib/modules/$new_path/extra/* /usr/src/initramfs/lib/modules/$new_path/extra/ || error "install_zfs" "Failed to copy over modules!"
                cp -Prv /lib/modules/$new_path/modules.* /usr/src/initramfs/lib/modules/$new_path/
            fi 
        else
            echo "====================================="
            echo "Kernel version $kernel is outside the range ($start - $end)."
        fi
    done 
    return 0
}
update_init() {
    local usr_kernel_str=${INSTALLED_KERNELS[$KEY]}
    mapfile -t usr_kernel_arr <<< "$usr_kernel_str"
    if [ -d "/usr/src/initramfs" ]; then 
        cd /usr/src/initramfs
    fi 
    for usr_kernels in "${usr_kernel_arr[@]}"; do
        usr_version=$(echo "$usr_kernels" | sed 's/^[^:]*://')
        if [ -d "/usr/src/initramfs/lib/modules/$usr_version-gentoo-$TYPE" ]; then
            lddtree --copy-to-tree /usr/src/initramfs /sbin/zfs 
            lddtree --copy-to-tree /usr/src/initramfs /sbin/zpool 
            lddtree --copy-to-tree /usr/src/initramfs /sbin/zed 
            lddtree --copy-to-tree /usr/src/initramfs /sbin/zgenhostid
            lddtree --copy-to-tree /usr/src/initramfs /sbin/zvol_wait 
            find . -not -path "/lib/modules/*" -o -path "./lib/modules/$usr_version-gentoo-$TYPE/*" -print0 | cpio --null --create --verbose --format=newc | gzip -9 > boot/initramfs-"$usr_version-gentoo-$TYPE".img
            echo "====================================="
        else 
            Dracut=$(cat /var/lib/portage/world | grep -E '^sys-kernel\/dracut-+:[0-9]+\.[0-9]+\.[0-9]+$')
            # Need to check to see if the string is empty
            if [[ -n "$Dracut" ]]; then
                # If it is not empty then generate initramfs using dracut
                dracut --force --kver="$usr_version" /boot/initramfs-"$usr_version-gentoo-$TYPE".img || error "update_init" "Failed to create initramfs using dracut!"
            fi
            GenKernel=$(cat /var/lib/portage/world | grep -E '^sys-kernel\/genkernel-+:[0-9]+\.[0-9]+\.[0-9]+$')
            # Need to check to see if the string is empty
            if [[ -n "$GenKernel" ]]; then
                # If it is not empty then generate initramfs using genkernel
                genkernel --kernel-config=/usr/src/linux-$usr_version-gentoo/.config initramfs --kerneldir=/usr/src/linux-$usr_version-gentoo || error "update_init" "Failed to create initramfs using genkernel!"
            else 
                error "update_init" "Error: The 'Dracut' and 'GenKernel' variables are empty.\nThis indicates a bug in the script. Please file a bug report or submit a pull request to resolve the issue at: https://github.com/alphastigma101/BashScripts\n\nSteps to follow:\n1. Provide details of your environment (e.g., OS, Bash version).\n2. Describe how to reproduce the issue.\n3. Submit a pull request with a fix if possible.\n\nIf you are unable to submit a fix, please report the issue with as much detail as you can."
            fi  
        fi 
    done 
    return 0
}
populate_data_structures() {
    curl -s https://packages.gentoo.org/packages/sys-kernel/$KEY > kernel_sources.html
    local line=$(grep -oP 'title="\d+\.\d+\.\d+ [^"]+"' kernel_sources.html)
    while read -d '"' -r chunk; do
        if [[ $chunk =~ ^title= ]]; then
            continue  # Skip the "title=" part
        fi
        if [[ $chunk =~ ^([0-9]+\.[0-9]+\.[0-9]+)[[:space:]]is[[:space:]](testing|stable|unknown)[[:space:]]on[[:space:]]([a-z0-9]+)$ ]]; then
            version="${BASH_REMATCH[1]}"
            status="${BASH_REMATCH[2]}"
            architecture="${BASH_REMATCH[3]}"
            # Initialize if empty
            ARCH_DICT[$architecture]="${ARCH_DICT[$architecture]:-}"
            # Append new value
            ARCH_DICT[$architecture]+="$status:$version "
        fi
    done < <(echo "$line")
    return 0
}
clean_up() {
    remove_html 
    return 0
}
main() {
    populate_zfs_releases
    check_kernel
    populate_data_structures
    install_kernel_resources "$STATUS"
    check_zfs
    install_zfs
    update_init
    update_bootloader
    clean_up
}
main
if [[ "$DEBUG" -eq 1 ]]; then
    debug_releases
    debug_arch_dict
    debug_installed_kernel
    debug_stable_kernel
    debug_testing_kernel
fi
```
This script web-scrapes [this page](https://github.com/openzfs/zfs/releases) to find the most recent updated ZFS module and the supported kernel version that goes with it. Keep in mind that the latest zfs module that is supported on that github openzfs will always have a stable version higher than the gentoo's current one. It would be very wise to use the zfs module that gentoo has marked down as stable.

If something crashes during run time, this script will be executed by calling a certain function. It will provide in depth details as what you can do. Keep in mind that this script can become very brittle and break if any of the DOM structures of the web-pages that are being web-scraped change.

**`/etc/hooks.d/zfs/bug_report.sh`**

**Bug Report**

```
#!/bin/bash
system_update_bug_report() {
    echo -e "### Bug Report: Issue with Kernel Update Detection in bash script\n"
    echo -e "#### Description:\n"
    echo -e "The script fails to correctly parse or match kernel packages ending with numeric versions. This issue affects the detection of installed kernel versions when running the script for Gentoo systems. Specifically, `gentoo-sources` or other kernel packages with numeric versions are not being detected properly, which impacts further operations in the script.\n"
    echo -e "#### Steps to Reproduce:\n"
    echo -e "#### How to Report a Bug:\n"
    echo -e "1. Visit the repository issues page: https://github.com/alphastigma101/BashScripts/issues\n"
    echo -e "2. Click on 'New Issue' to create a bug report.\n"
    echo -e "3. Provide a detailed description of the issue, including steps to reproduce the error and the expected vs. actual behavior.\n"
    echo -e "4. Include the relevant logs or output from running the script, as well as any error messages.\n"
    echo -e "5. Be sure to include your system and environment details such as Gentoo version, Portage version, and Bash version.\n"
    echo -e "\n#### Expected Behavior:\n"
    echo -e "The script should correctly parse and list installed kernel versions with numeric versioning, specifically matching lines like 'sys-kernel/gentoo-sources:6.1.118'.\n"
    echo -e "\n#### Actual Behavior:\n"
    echo -e "The script fails to detect or properly handle kernel packages that end with numeric versions (e.g., 'sys-kernel/gentoo-sources:6.1.118') and does not store them in the dictionary.\n"
    echo -e "\n#### Environment:\n"
    echo -e " - Operating System: Gentoo Linux\n"
    echo -e " - Script Version: $(git describe --tags)\n"
    echo -e " - Bash version: $(bash --version | head -n 1)\n"
    echo -e " - Portage version: $(emerge --info | grep -i 'portage' | head -n 1)\n"
    echo -e "\n#### Logs/Output:\n"
    echo -e "Please provide any additional log output below that could help identify the root cause of the issue.\n"
    echo -e "For example:\n"
    echo -e " - Output from running the kernel update script.\n"
    echo -e " - Any error messages.\n"
}
```
This script provides a list of implemented functions that can debug the data structures if DEBUG is set to 1. This is useful when unexcpected behavior occurs. And one of the reasons could be that there's no data inside a specific data structure, which will cause the script to break.

**`/etc/hooks.d/zfs/debug_data_structures.sh`**

**Debug Data Structures**

```
#!/bin/bash
declare -A INSTALLED_KERNELS
declare -A ARCH_DICT
declare -A ZFS_DICT
declare -A STABLE_KERNELS
declare -A TESTING_KERNELS
declare -A AVAILABLE_KERNELS
declare -A AVAILABLE_ZFS
declare -a BUILD_KERNELS
declare -a CURRENT_ZFS
debug_zfs_releases() {
    # Print out the compatible releases
    echo "Compatible ZFS Releases:"
    for release in "${!COMPATIBLE_RELEASES[@]}"; do
        echo "$release: Linux kernel ${COMPATIBLE_RELEASES[$release]}"
    done
    echo "====================================="
}
debug_arch_dict() {
    for arch in "${!ARCH_DICT[@]}"; do
        echo "Architecture: $arch"
        echo "Versions: ${ARCH_DICT[$arch]}"
        echo "-------------------"  
    done
    echo "====================================="
    return 0
}
debug_installed_kernel() {
    echo "Kernel Packages:"
    for key in "${!INSTALLED_KERNELS[@]}"; do
        echo "$key: ${INSTALLED_KERNELS[$key]}"
    done
    echo "====================================="
    return 0
}
debug_available_kernel() {
    echo "AVAILABLE_KERNELS:"
    for key in "${!AVAILABLE_KERNELS[@]}"; do
        echo "$key: ${AVAILABLE_KERNELS[$key]}"
    done
    return 0
}
```
Contains a series of functions that are used inside system\_update.sh or else where. Tha main reason why I created this script is to make the code more organize and easier to maintain.

**`/etc/portage/postsync.d/helper_functions.sh`**

**Helper Functions**

```
#!/bin/bash
# Function to compare if version is greater than or equal to the comparison version
version_greater_than_equal() {
    [[ "$(echo -e "$1\n$2" | sort -V | head -n 1)" == "$2" ]]
}
# Function to compare if version is less than or equal to the comparison version
version_less_than_equal() {
    [[ "$(echo -e "$1\n$2" | sort -V | tail -n 1)" == "$2" ]]
}
```
If something happens during run time, this the string literal of the function name where unexpected behaivor occured and the a brief sentence explaining what happen. Keep in mind, some errors can be false due to missing folders or files. Which isn't a problem, it just means that another if statement is needed.

**`/etc/portage/postsync.d/error_handling.sh`**

**Error Handling**

```
#!/bin/bash
# Function that will output errors
error() {
    local x="$1"
    local y="$2"
    local arr="$3"
    if [ "$x" == "install_kernel" ]; then
        echo "[ERROR:] In $x. $y! "
        echo "Cleaning up...."
        make clean || cd /usr/src/linux && make clean
        make mrproper
        exit 0
    elif  [ "$x" == "install_kernel_resources" ]; then
        echo "[ERROR:] In $x. $y! "
        exit 0
    elif [ "$x" == "install_zfs" ]; then 
        echo "[ERROR:] In $x. $y! "
        exit 0
    elif [ "$x" == "update_init" ]; then 
        echo "[ERROR:] In $x. $y! "
        exit 0
    fi
    exit 0
}
```
After everything is implemented and configured, make the scripts inside /etc/hooks.d/zfs executable:

`root #````
chmod +x /etc/hooks.d/zfs/*.sh
```

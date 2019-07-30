ansible_ios
====

ansible_ios is a collection of ansible YML files for cisco ios. It is tested on WSL (Windows Subsystems for Linux, Ubuntu1804). Result file will be placed under host directory with date format.

## Description

| YML file      | Output     | description    |
----------------|------------|-----------------
| all_shint.yml | individual | show interface status<br>show ip int brief | 
| all_shrun.yml | individual | show run |
| all_shrun.yml | individual | show tech-support |
| all_shrun.yml | all        | show version<br>show hardware |

## Requirement

## Install
    sudo -s
    mkdir ansible_ios
    cd ansible_ios
    git clone git@github.com:hogehogemon/ansible_ios
    
## Configuration
Modify an inventory file at /etc/ansible/hosts referring the inventory_sample. root previledge is required to modify this.

    [cisco]
    192.168.0.11 user=cisco pass=password en=password
    192.168.0.12 user=cisco pass=password en=password
    192.168.0.13 user=cisco pass=password en=password

    [cisco:vars]
    # Log output directory for WSL.
    dir_log=/mnt/c/temp/ansible_ios
    dir_tmp=/tmp
    
## Usage
Specify the yml file
    
    # ansible-playbook <yml file>
    
For example,    
    
    # ansible-playbook all_shrun.yml
   
    PLAY [cisco] **********************************************************************************************************
    
    TASK [execute command] ************************************************************************************************
    Tuesday 30 July 2019  16:27:21 +0900 (0:00:00.304)       0:00:00.304 **********
    ok: [192.168.0.12 -> localhost]
    ok: [192.168.0.11 -> localhost]
    ok: [192.168.0.13 -> localhost]
    
    TASK [log write out] **************************************************************************************************
    Tuesday 30 July 2019  16:27:26 +0900 (0:00:05.922)       0:00:06.226 **********
    changed: [192.168.0.11 -> localhost]
    changed: [192.168.0.13 -> localhost]
    changed: [192.168.0.12 -> localhost]
    
    TASK [mkdir [hostname]] ***********************************************************************************************
    Tuesday 30 July 2019  16:27:29 +0900 (0:00:02.722)       0:00:08.949 **********
    ok: [192.168.0.11 -> localhost]
    ok: [192.168.0.13 -> localhost]
    ok: [192.168.0.12 -> localhost]
    
    TASK [parse config] ***************************************************************************************************
    Tuesday 30 July 2019  16:27:31 +0900 (0:00:01.499)       0:00:10.449 **********
    changed: [192.168.0.12 -> localhost]
    changed: [192.168.0.13 -> localhost]
    changed: [192.168.0.11 -> localhost]
    
    TASK [backup file by date] ********************************************************************************************
    Tuesday 30 July 2019  16:27:32 +0900 (0:00:01.640)       0:00:12.089 **********
    changed: [192.168.0.12 -> localhost]
    changed: [192.168.0.11 -> localhost]
    changed: [192.168.0.13 -> localhost]
    
    PLAY RECAP ************************************************************************************************************
    192.168.0.11                : ok=5    changed=3    unreachable=0    failed=0
    192.168.0.12                : ok=5    changed=3    unreachable=0    failed=0
    192.168.0.13                : ok=5    changed=3    unreachable=0    failed=0
    
    Tuesday 30 July 2019  16:27:34 +0900 (0:00:01.606)       0:00:13.696 **********
    ===============================================================================
    execute command ------------------------------------------------------------------------------------------------ 5.92s
    log write out -------------------------------------------------------------------------------------------------- 2.72s
    parse config --------------------------------------------------------------------------------------------------- 1.64s
    backup file by date -------------------------------------------------------------------------------------------- 1.61s
    mkdir [hostname] ----------------------------------------------------------------------------------------------- 1.50s
    
Result for 192.168.0.11 would be 

    # ls -l /mnt/c/temp/ansible_ios/192.168.0.11
    total 1404
    -rwxrwxrwx 1 test test    8969 Jul 18 15:11 192.168.0.11-shrun-20190730-1702.log
    -rwxrwxrwx 1 test test    8968 Jul 30 16:27 192.168.0.11-shrun.log

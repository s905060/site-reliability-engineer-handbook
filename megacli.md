# Megacli

1. Show Rebuid procedure
```
/opt/MegaRAID/MegaCli/MegaCli64 -PDRbld -ShowProg -physdrv[20:2] -aALL
```

2. Check ES
```
/opt/MegaRAID/MegaCli/MegaCli64 -PDList -aAll -NoLog | grep -Ei "(enclosure|slot)"
```

3. Check all drives status
```
/opt/MegaRAID/MegaCli/MegaCli64 -PDList -aAll -NoLog 
```

4. Check all Virtual Disks status
```
/opt/MegaRAID/MegaCli/MegaCli64 -LdPdInfo -aAll -NoLog
```

RAID Level：

|--|--|
|--|--|
|RAID Level : Primary-1, Secondary-0, RAID Level Qualifier-0|RAID 1
|RAID Level : Primary-0, Secondary-0, RAID Level Qualifier-0|RAID 0
|RAID Level : Primary-5, Secondary-0, RAID Level Qualifier-3|RAID 5
|RAID Level : Primary-1, Secondary-3, RAID Level Qualifier-0|RAID 10

5. Online Raid
```
/opt/MegaRAID/MegaCli/MegaCli64 -CfgLdAdd -r0[0:11] WB NORA Direct CachedBadBBU -strpsz64 -a0 -NoLog
/opt/MegaRAID/MegaCli/MegaCli64 -CfgLdAdd -r5 [12:2,12:3,12:4,12:5,12:6,12:7] WB Direct -a0 
```

6. Allocate hard drive
```
/opt/MegaRAID/MegaCli/MegaCli64 -PdLocate -start -physdrv[252:2] -a0 
```

7. Clean Foreign status
```
/opt/MegaRAID/MegaCli/MegaCli64 -CfgForeign -Clear -a0 
```

8. Check offline drive in RAID array
```
/opt/MegaRAID/MegaCli/MegaCli64 -pdgetmissing -a0 
```

9. Replace bad drive 
```
/opt/MegaRAID/MegaCli/MegaCli64 -pdreplacemissing -physdrv[12:10] -Array5 -row0 -a0 
```

10. Run rebuild manually
```
/opt/MegaRAID/MegaCli/MegaCli64 -pdrbld -start -physdrv[12:10] -a0 
```

11. Chek Megacli log
```
/opt/MegaRAID/MegaCli/MegaCli64 -FwTermLog dsply -a0 > adp2.log
```

12. Set up HotSpare
```
/opt/MegaRAID/MegaCli/MegaCli64 -pdhsp -set [-Dedicated [-Array2]] [-EnclAffinity] [-nonRevertible] -PhysDrv[4:11] -a0
/opt/MegaRAID/MegaCli/MegaCli64 -pdhsp -set [-EnclAffinity] [-nonRevertible] -PhysDrv[32：1}] -a0
```

13. Disable Rebuild
```
/opt/MegaRAID/MegaCli/MegaCli64 -AdpAutoRbld -Dsbl -a0 
```

14. Set up rebuild rate
```
/opt/MegaRAID/MegaCli/MegaCli64 -AdpSetProp RebuildRate -30 -a0 
```
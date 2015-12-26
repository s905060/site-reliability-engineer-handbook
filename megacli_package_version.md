# Megacli package version

1. for viewing the status of each disk, and highlighted megacli script

```
/opt/MegaRAID/MegaCli/MegaCli64 -PDList -aAll -NoLog| grep -Ei '(enclosure|slot|error|firmware|pre|Foreign|^PD type|^Raw Size)' | awk '{if($0 ~ /Slot Number/ || $0 ~ /Enclosure/){printf("\033[32m%s ",$0);if($0 ~/Slot Number/) printf("\033[0m \n")}else if($0 ~ /bad/||$0 ~/Failed/||$0 ~ /Foreign State: Foreign/){print "\033[31m"$0"\033[0m"}else{print "\033[33m"$0"\033[0m"}}'
```
# Dmidecode

Burned into the motherboard manufacturer's serial number (SN) is unique, it can be used as a server unique index. It can easily crawl to the server's serial number by dmidecode command under Linux. 

However, due to various manufacturers of SMBios write specifications are not the same brush, we need to do some compatible operating. 
A brief summary of what the table below

|Firm	|General grab method
|-- |-- |
|Dell	|dmidecode -s system-serial-number
|HP	    |dmidecode -s system-serial-number
|IBM	|dmidecode -s system-serial-number
|Huawei	|dmidecode -s system-serial-number (Huawei rack server) or dmidecode -s baseboard-serial-number (Huawei blade)

Use a shell to cover all models, as follows:
```
get_sn(){
    local mySN=`dmidecode -s system-serial-number | grep -v '#'`
    if echo "${mySN}" | grep -qiE "^NotSpecified|^None|^ToBeFilledByO.E.M.|O.E.M." ; then
            mySN=`dmidecode -s baseboard-serial-number`
    fi
    # For RHEL4 and CentOS4, dmidecode -s parameter is not supported, you need to use a different method to obtain SN
    if grep -q 'release 4'  /etc/redhat-release ; then
        mySN=`dmidecode | grep -A5 'System Information' | grep 'Serial Number' | awk '{print $3}' | sed 's/^[ \t]*//g' | sed 's/[ \t]$//g'`
    fi
    echo $mySN
}
```
# Xen Common notes

###Determine whether the CPU supports virtualization
Intel CPU's flag is vmx, AMD's CPU is svm. CPU support but if their cat / proc / cpuinfo see, please check the BIOS is off Intel's VT or AMD SVM

`cat / proc / cpuinfo | grep - color - E ( VMX | svm )`

###View Xen version
```
cat / sys / hypervisor / Version / Major
cat / sys / hypervisor / Version / minor
cat / sys / hypervisor / Version / Extra
```
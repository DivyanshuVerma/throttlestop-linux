# throttlestop-linux
Steps to disable bd-prochot bit on linux on select dell laptops

## Description of issue


## Remedy
The original cpu speed can be brought back by disabling the bd-prochot bit which throttles the cpu.

## Caveats
You need to keep an eye on cpu temperatures to prevent prolonged high cpu workloads as it won't throttle anymore. In the worst case, your cpu might abruptly shut itself off when it reaches >100°C

## Steps
Install cpufrequtils to check cpu usage on all cores.

Install msr-tools for modprobe, rdmsr and wrmsr to probe/read/write model-specific registers (MSRs) of an x86 CPU

```
$ sudo apt install cpufrequtils msr-tools
```

Enable probing

```
$ sudo modprobe msr
```

Read 0x1FC register on all cores and output as decimal

```
$ sudo rdmsr -a -d 01xFC

2359389
2359389
2359389
2359389

```

Switch off the bd-prochot bit by subtracting 1 from the decimal output above. Do not copy paste the below command, but 

```
$ sudo wrmsr 0x1FC 2359388
```

Verify by checking if cpu speed is changing 

```
$ watch -n 1 cpufreq-info

Every 1.0s: cpufreq-info

cpufrequtils 008: cpufreq-info (C) Dominik Brodowski 2004-2009
Report errors and bugs to cpufreq@vger.kernel.org, please.
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency: 4294.55 ms.
  hardware limits: 800 MHz - 3.20 GHz
  available cpufreq governors: performance, powersave
  current policy: frequency should be within 800 MHz and 3.20 GHz.
                  The governor "powersave" may decide which speed to use
                  within this range.
  current CPU frequency is 2.70 GHz.
analyzing CPU 1:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 1
  CPUs which need to have their frequency coordinated by software: 1
  maximum transition latency: 4294.55 ms.
  hardware limits: 800 MHz - 3.20 GHz
  available cpufreq governors: performance, powersave
  current policy: frequency should be within 800 MHz and 3.20 GHz.
                  The governor "powersave" may decide which speed to use
                  within this range.
  current CPU frequency is 2.69 GHz.
analyzing CPU 2:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 2
  CPUs which need to have their frequency coordinated by software: 2
  maximum transition latency: 4294.55 ms.
  hardware limits: 800 MHz - 3.20 GHz
  available cpufreq governors: performance, powersave
  current policy: frequency should be within 800 MHz and 3.20 GHz.
                  The governor "powersave" may decide which speed to use
                  within this range.
  current CPU frequency is 2.70 GHz.
analyzing CPU 3:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 3
  CPUs which need to have their frequency coordinated by software: 3
  maximum transition latency: 4294.55 ms.
  hardware limits: 800 MHz - 3.20 GHz
  available cpufreq governors: performance, powersave
  current policy: frequency should be within 800 MHz and 3.20 GHz.
                  The governor "powersave" may decide which speed to use
                  within this range.
  current CPU frequency is 2.70 GHz.

```

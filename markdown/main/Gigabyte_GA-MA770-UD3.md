<!-- source: https://wiki.gentoo.org/wiki/Gigabyte_GA-MA770-UD3 | group: Gentoo Wiki (Main) | wiki-title: Gigabyte GA-MA770-UD3 -->
---
title: Gigabyte GA-MA770-UD3
url: https://wiki.gentoo.org/wiki/Gigabyte_GA-MA770-UD3
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-22"
fingerprint: b5813a574ba67eb9
license: CC BY-SA 4.0
---

# Gigabyte GA-MA770-UD3

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## General Information

The GA-MA770-UD3 is an AMD motherboard with an AM2+ socket and SB700 chipset. This page details the rev 1.0 hardware; there are also 2.0 and 2.1 variants with minor updates.

## Hardware Specs

| CPU Support | AM2/AM2+/AM3 (amdfam10h; up to Phenom II X6 1090T) | 
|---|---|
| Memory | 4× DDR 2 slots, maximum 16GB DDR2-1200+ | 
| Expansion Slots |  | 
| Internal Storage |  | 
| Connectivity |  | 

## Kernel Configuration

### CPU

### Disks

**{00:11.0 SATA controller,00:14.1 IDE interface}: Advanced Micro Devices, Inc. \[AMD/ATI\] SB7x0/SB8x0/SB9x0 {SATA Controller \[AHCI mode\],IDE Controller}**

Floppy disk support is there, if needed:

**00:14.3 ISA bridge: Advanced Micro Devices, Inc. \[AMD/ATI\] SB7x0/SB8x0/SB9x0 LPC host controller**

### Sound

### Ethernet

**02:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 02)**

### Peripherals

### Sensors

Many AMD Family 10h (i.e. Athlon/Phenom II) chips have a faulty temperature sensor. It's safe to enable the driver here, but the kernel will detect affected CPUs and disable it automatically for them. In that case there's no reason to leave it enabled at all.

lm-sensors doesn't have an upstream config file for this chipset; the following is a best-effort attempt to write one:

**`/etc/sensors.d/it8720-GA-MA770-UD3.conf`**

**lm-sensors config file**

Sensor output with the above configuration:

`user $``sensors | sed -n '/it87/,/^   }$/ p'`
it8720-isa-0228
Adapter: ISA adapter
DDR2 1.8V:     1.82 V  (min =  +1.71 V, max =  +1.89 V)
+3.3V:         3.34 V  (min =  +3.14 V, max =  +3.47 V)
+5V:           5.03 V  (min =  +4.76 V, max =  +5.24 V)
+12V:         12.35 V  (min = +11.39 V, max = +12.61 V)
Vcore?:        1.65 V  (min =  +1.42 V, max =  +2.21 V)
5VSB:          4.97 V  (min =  +4.76 V, max =  +5.24 V)
Vbat:          3.14 V
CPU Fan:     1527 RPM  (min =    0 RPM)
Sys Fan:     1584 RPM  (min =    0 RPM)
Sys Temp:     +37.0°C  (low  = +28.0°C, high = +48.0°C)  sensor = thermistor
CPU Temp:     +45.0°C  (low  = +25.0°C, high = +65.0°C)  sensor = thermal diode

The raw unfiltered sensor values for reference:

`user $``sensors -jc /dev/null | sed -n '/it87/,/^   }$/ p'````
   "it8720-isa-0228":{
      "Adapter": "ISA adapter",
      "in0":{
         "in0_input": 1.296,
         "in0_min": 0.896,
         "in0_max": 1.264,
         "in0_alarm": 0.000,
         "in0_beep": 0.000
      },
      "in1":{
         "in1_input": 1.824,
         "in1_min": 1.712,
         "in1_max": 1.888,
         "in1_alarm": 0.000,
         "in1_beep": 0.000
      },
      "in2":{
         "in2_input": 3.344,
         "in2_min": 3.136,
         "in2_max": 3.472,
         "in2_alarm": 0.000,
         "in2_beep": 0.000
      },
      "+5V":{
         "in3_input": 2.992,
         "in3_min": 2.832,
         "in3_max": 3.120,
         "in3_alarm": 0.000,
         "in3_beep": 0.000
      },
      "in4":{
         "in4_input": 3.072,
         "in4_min": 2.848,
         "in4_max": 3.152,
         "in4_alarm": 0.000,
         "in4_beep": 0.000
      },
      "in5":{
         "in5_input": 1.584,
         "in5_min": 1.424,
         "in5_max": 2.208,
         "in5_alarm": 0.000,
         "in5_beep": 0.000
      },
      "in6":{
         "in6_input": 3.344,
         "in6_min": 0.000,
         "in6_max": 4.080,
         "in6_alarm": 0.000,
         "in6_beep": 0.000
      },
      "5VSB":{
         "in7_input": 2.960,
         "in7_min": 2.832,
         "in7_max": 3.120,
         "in7_alarm": 0.000,
         "in7_beep": 0.000
      },
      "Vbat":{
         "in8_input": 3.136
      },
      "fan1":{
         "fan1_input": 1530.000,
         "fan1_min": 0.000,
         "fan1_alarm": 0.000,
         "fan1_beep": 0.000
      },
      "fan2":{
         "fan2_input": 1588.000,
         "fan2_min": 0.000,
         "fan2_alarm": 0.000,
         "fan2_beep": 0.000
      },
      "fan3":{
         "fan3_input": 0.000,
         "fan3_min": 0.000,
         "fan3_alarm": 0.000,
         "fan3_beep": 0.000
      },
      "fan5":{
         "fan5_input": 0.000,
         "fan5_min": 0.000,
         "fan5_alarm": 0.000,
         "fan5_beep": 0.000
      },
      "temp1":{
         "temp1_input": 37.000,
         "temp1_max": 48.000,
         "temp1_min": 28.000,
         "temp1_alarm": 0.000,
         "temp1_type": 4.000,
         "temp1_offset": 6.000,
         "temp1_beep": 0.000
      },
      "temp2":{
         "temp2_input": 45.000,
         "temp2_max": 65.000,
         "temp2_min": 25.000,
         "temp2_alarm": 0.000,
         "temp2_type": 3.000,
         "temp2_offset": 6.000,
         "temp2_beep": 0.000
      },
      "temp3":{
         "temp3_input": 79.000,
         "temp3_max": 127.000,
         "temp3_min": 127.000,
         "temp3_alarm": 0.000,
         "temp3_type": 4.000,
         "temp3_offset": 6.000,
         "temp3_beep": 0.000
      },
      "cpu0_vid":{
         "cpu0_vid": 1.250
      },
      "intrusion0":{
         "intrusion0_alarm": 0.000
      }
   }
```
## Other details

### lspci output

Some of these can be disabled in the BIOS, in which case they won't appear here at all.

### Known hardware issues

- The ethernet chip may forget its speed over a suspend and default to 100Mbit after resume. To work around this, put ethtool -s eth0 speed 1000 in the appropriate place in your suspend scripts.
- Most AMD family 10h CPUs have a processor erratum related to the temperature sensor's unreliability, which is why the driver is disabled above.
- The SB700 chipset is affected by a kernel quirk that prevents it using message-signalled interrupts due to the possibility of data corruption when a floppy drive is used. There's a [patch on the Gentoo forums](https://forums.gentoo.org/viewtopic-t-905708.html) (updated version [here](https://forums.gentoo.org/viewtopic-p-7874714.html#7874714)) to disable the kernel's workarounds, which may or may not improve performance slightly.

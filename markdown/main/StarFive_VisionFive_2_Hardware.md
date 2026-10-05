<!-- source: https://wiki.gentoo.org/wiki/StarFive_VisionFive_2/Hardware | group: Gentoo Wiki (Main) | wiki-title: StarFive VisionFive 2/Hardware -->
---
title: StarFive VisionFive 2/Hardware
url: https://wiki.gentoo.org/wiki/StarFive_VisionFive_2/Hardware
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-01"
fingerprint: "5f13895499869fea"
license: CC BY-SA 4.0
---

# StarFive VisionFive 2/Hardware

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Hardware components

#### TL;DR

- The base functionalities are operational under recent (6.12.x) kernels but the more advanced ones are "hidden"
- Operational hardware supported out-of-the-box by the mainstream kernel: GPU, USB, eMMC, SD Card slot, GPIO, both 1GbE NICs, PWMDAC (audio jack) and the UART ;
- Hidden advanced functionalities:
  - Video out (HDMI) ;
  - Audio DSP engine ;
  - Image processing engine ;
  - Video encoder/decoder engines ;
  - JPEG engine ;
- Old and heavily customized 6.6/5.10 patched kernels giving all of the hidden functionalities are on Github (see [https://github.com/starfive-tech/linux/](https://github.com/starfive-tech/linux/))

#### Details

The StarFive VisionFive 2 is a Single Board Computer (SBC) based on a StarFive JH7110 SoC (*rv64gc* subarch), an Imagination BXE-4-32 GPU and several other components detailed hereafter. It comes in variants of 2/4/8 GB of LPDDR4 memory. See the document [VisionFive2\_QSG.pdf](https://doc-en.rvspace.org/VisionFive2/PDF/VisionFive2_QSG.pdf) for a full specification. This SBC supports TF/SD, eMMC, USB and NVMe storage devices, as well as having a 40-pin GPIO header and a 2-bit RGPIO boot device selector switch.

- **CPU:**
  - Model: SiFive JH7110 SoC running at 1.5 GHz with 5 HARTs (a HART is a CPU core abstraction, i.e. an execution context containing a full set of RISC-V architectural registers that executes its program independently from other HARTs)
  - Cores (HARTs):
    - 4× U7 64-bits RISC‑V cores (RV64GC) for general usage
    - 1x S7 64-bits RISC‑V core (RV64IMAC) for monitoring (not used in Linux)
    - 1x E24 32-bits RISC‑V core (RV32IMAFCB) for low power and control/configure tasks as a coprocessor in JH7110 SoC (Not used in Linux)
  - Datasheet: [u74mc\_core\_complex\_manual\_21G1.pdf](https://starfivetech.com/uploads/u74mc_core_complex_manual_21G1.pdf)
  - Notes:
    - The [Starfive fork of the Linux kernel](https://github.com/starfive-tech/linux) includes the code to use the E24 (no support at upstream)

- **System DRAM**:
  - Model: BIWIN 2/4/8 GB LPDDR4
  - Notes:
    - Not to be confused with the on-chip SRAM memory

- **EEPROMs**:
  - Model: 24FC04H (4 kbits / 512 bytes) - I2C
  - Datasheet: [24AA04H-24LC04BH-24FC04H-4K-I2C-Serial-EEPROM-With-Half-Array-Write-Protect-20002119C.pdf](https://ww1.microchip.com/downloads/aemDocuments/documents/MPD/ProductDocuments/DataSheets/24AA04H-24LC04BH-24FC04H-4K-I2C-Serial-EEPROM-With-Half-Array-Write-Protect-20002119C.pdf)
  - Notes:
    - Stores some board information like the Ethernet addresses, the product serial number, etc. **No code stored here.**
    - Reference mentioned in: [starfive/visionfive2/visionfive2-i2c-eeprom.c](https://github.com/starfive-tech/u-boot/blob/JH7110_VisionFive2_devel/board/starfive/visionfive2/visionfive2-i2c-eeprom.c) (GitHub)
    - Referred as "EEPROM" in  [manual](https://doc-en.rvspace.org/VisionFive2/PDF/VisionFive2_QSG.pdf%7Cthe)
  - Model: GD25LQ128 (128mbit / 16 mega-bytes) - QSPI
  - Datasheet: [DS-00291-GD25LQ128D-Rev1.9.pdf](https://www.gigadevice.com.cn/Public/Uploads/uploadfile/files/20220714/DS-00291-GD25LQ128D-Rev1.9.pdf)
  - Linux kernel support: supported at upstream (MTD device on a QSPI controller)
  - Notes:
    - Stores U-Boot and various firmware files (DTB, OpenSBI, etc).
    - Referred as "QSPI Flash" or "SPI Flash" in [the manual](https://doc-en.rvspace.org/VisionFive2/PDF/VisionFive2_QSG.pdf)

- **Ethernet controllers (PHY/GMAC):**
  - Model/IP: 2x Motorcomm YT8531 (1 GbE) for board revision 1.3B (one YT8531 per Ethernet plug). Older boards use models YT8521C and YT8512C (100 MbE)
  - Datasheet: [YT8531\_xiliejieshaov0.3.pdf](https://en.motor-comm.com/Public/Uploads/uploadfile/files/20230308/YT8531_xiliejieshaov0.3.pdf)
  - Linux kernel support: supported at upstream

- **USB controller**:
  - Model/IP: Via Labs (VLI) VL805-Q6
  - Datasheet: [Via Labs VL805](https://www.via-labs.com/product_show.php?id=48)
  - Linux kernel support: supported at upstream
  - Notes:
    - Connected via the PCIe bus so shares the bandwidth
    - Discussion thread where the PDF datasheet is mentioned : ["PCIe to 4 USB ports use vl805 chipset on jetson nano custom carrier board"](https://forums.developer.nvidia.com/t/pcie-to-4-usb-ports-use-vl805-chipset-on-jetson-nano-custom-carrier-board/143085) ([direct link](https://forums.developer.nvidia.com/uploads/short-url/nQJG4jTyFv0G6HgeKLyN6jhK8FE.zip))
    - According to its datasheet, **this controller supports USB debugging**

- **Power Management Integrated Controller (PMIC)**:
  - Model/IP: X-Powers AXP-15060
  - Datasheet: [AXP15060 datasheet V0.1.pdf](https://files.pine64.org/doc/datasheet/star64/AXP15060%20datasheet%20V0.1.pdf)
  - Linux kernel support: supported at upstream

- **On-board audio DAC:**
  - Model/IP: in-house PWM-DAC
  - Linux kernel support: fully supported by Linux and ALSA without out-of-tree patches but no volume control as simple Resistor-Capacitor output which is connected to the audio jack
  - The WM8960 audio HAT for the Raspberry Pi 4 can be used on the GPIO header

- **GPIO PWM (Pulse Width Modulation generator):**
  - Mode/IP: in-house
  - Linux kernel support: supported with out-of-tree patches (see SDK), patches submitted at upstream but not merged as of 6.12/6.13
  - Notes:
    - The PWM generator is connected the GPIO and can be driven from `/sys`.

- **CAN (Controller Area Network) bus controller:**
  - Model/IP: CAST CAN-CTRL
  - Linux kernel support: supported with out-of-tree patches (see SDK), patches submitted at upstream but not merged as of 6.12/6.13
  - Notes:
    - An [expired license issue](https://patchwork.kernel.org/project/linux-riscv/cover/20240922145151.130999-1-hal.feng@starfivetech.com/) with this IP makes StarFive to only provide the *[Classic](https://www.can-cia.org/can-knowledge/can-cc)* (CC) CAN bus generation. No support for CAN FD and CAN XL.

- **I2C bus controller:**
  - Model/IP: Synopsys DesignWare I2C adapter
  - Linux kernel support: Supported at upstream

- **SPI bus controller:**
  - Model/IP: Cadence Quad SPI controller
  - Linux kernel support: Supported at upstream

- **Display (HDMI out) controllers:**
  - Model/IP: VeriSilicon Vivante DC8200 (display processing) + InnoSilicon HDMI transceiver
  - Linux kernel support: supported with out-of-tree patches (see SDK), patches submitted at upstream but not merged as of 6.12/6.13
  - Notes:
    - There is a TDA998X/TDA9950 IP for CEC control (according to the source/device tree code in the SDK) => [datasheet](https://media.digikey.com/pdf/data%20sheets/nxp%20pdfs/tda9950.pdf)
    - No complete public datasheet found but some scarse information can be found on [rvspace.org](https://doc-en.rvspace.org/JH7110/TRM/JH7110_TRM/block_diagram_display.html)
    - Some reverse engineering done see [here](https://lupyuen.codeberg.page/articles/display2.html) and [here](https://lupyuen.codeberg.page/articles/display3.html)
    - Vivante DC8000 is a family, no information specific to the DC-8200 model however
    - A [discussion thread](https://forums.sifive.com/t/vivante-dc8000-display-controller-documentation/6695/2) on SiFi forums mentions a [repository](https://github.com/eswincomputing/u-boot/blob/u-boot-2024.01-EIC7X/drivers/video/eswin/eswin_dc_reg.h) on GitHub where some information on the DC8000 can be found (HiFive Premier P550/ESWin 7700X SoC)
    - [ESWin 7700X SoC TRMs](https://github.com/eswincomputing/EIC7700X-SoC-Technical-Reference-Manual/releases) can be found on GitHub.
    - Some RUST code lies [here](https://codeberg.org/weathered-steel/jh71xx-pac.git) with descriptions

- **On-chip GPU, DSP units and accelerators:**
  - *GPU:*
    - Model/IP: Imagination IMG BXE-4-32 MC1
    - Datasheet: [product page](https://www.imaginationtech.com/product/img-bxe-4-32-mc4) but no public datasheet
    - Linux kernel support: supported with out-of-tree patches (see SDK), patches submitted at upstream but not yet merged as of 6.12/6.13
    - Notes:
      - Partial datasheet (covers registers only): [rogue-registers-description-docs.zip](https://developer.imaginationtech.com/wp-content/uploads/2023/12/rogue-registers-description-docs.zip)
    - Work in progress! Datasheet should be published "soon" (see ["Imagination Tech Publishes Open-Source PowerVR Vulkan Driver For Mesa" on Phoronix](https://www.phoronix.com/news/Open-Source-PowerVR-Vulkan))
  - *Audio DSP:*
    - Model/IP: Cadence Tensilica HiFi4 DSP
    - Datasheet: No public datasheet
    - Linux kernel support: supported with out-of-tree patches (see [Starfive Linux kernel fork](https://github.com/starfive-tech/linux)), code needs to be ported for recent kernels (last version available is for 6.6.x kernel series)
  - *Vision DSP:*
    - Model/IP: Cadence Tensilica Vision P6 DSP (VP6)
    - Datasheet: No public datasheet
    - Linux kernel support: supported with out-of-tree patches (see SDK)
  - *JPEG processing unit (JPU):*
    - Model/IP: Chips&Media CODAJ12
    - Datasheet: No public datasheet
    - Linux kernel support: supported with out-of-tree patches (see [Starfive Linux kernel fork](https://github.com/starfive-tech/linux)), code needs to be ported for recent kernels (last version available is for 6.6.x kernel series)
  - *Video stream encoder (H.264/H.265):*
    - Model/IP: Chips&Media WAVE512
    - Datasheet: No public datasheet
    - Linux kernel support: supported with out-of-tree patches (see [Starfive Linux kernel fork](https://github.com/starfive-tech/linux)), code needs to be ported for recent kernels (last version available is for 6.6.x kernel series)
    - Notes:
      - Some demonstration code exists in the [Visionfive 2 SDK](https://github.com/starfive-tech/soft_3rdpart/tree/JH7110_VisionFive2_devel/wave511)
  - *Video stream decoder (H.264/H.265):*
    - Model/IP: Chips&Media WAVE420L
    - Datasheet: No public datasheet
    - Linux kernel support: supported with out-of-tree patches (see [Starfive Linux kernel fork](https://github.com/starfive-tech/linux)), code needs to be ported for recent kernels (last version available is for 6.6.x kernel series)
    - Notes:
      - Some demonstration code exists in the [Visionfive 2 SDK](https://github.com/starfive-tech/soft_3rdpart/tree/JH7110_VisionFive2_devel/wave511)
  - *Crypto-engine:*
    - Model/IP: in-house
    - Linux kernel support: supported at upstream level

### RISC-V ISA standard and extensions supported

When identifying the RISC-V ISA standard and extensions for the target device, the following table may be useful:

| Name | Description |  |  |  | 
|---|---|---|---|---|
| RV32I | Base Integer Instruction Set - 32-bit |  |  |  | 
| RV32E | Base Integer Instruction Set (embedded) - 32-bit, 16 registers |  |  |  | 
| RV64I | Base Integer Instruction Set - 64-bit |  |  |  | 
| RV128I | Base Integer Instruction Set - 128-bit |  |  |  | 
| Extension |  |  |  |  | 
| M | Standard Extension for Integer Multiplication and Division |  |  |  | 
| A | Standard Extension for Atomic Instructions |  |  |  | 
| F | Standard Extension for Single-Precision Floating-Point |  |  |  | 
| D | Standard Extension for Double-Precision Floating-Point |  |  |  | 
| G | Shorthand for the base and above extensions |  |  |  | 
| Q | Standard Extension for Quad-Precision Floating-Point |  |  |  | 
| L | Standard Extension for Decimal Floating-Point |  |  |  | 
| C | Standard Extension for Compressed Instructions |  |  |  | 
| B | Standard Extension for Bit Manipulation |  |  |  | 
| J | Standard Extension for Dynamically Translated Languages |  |  |  | 
| T | Standard Extension for Transactional Memory |  |  |  | 
| P | Standard Extension for Packed-SIMD Instructions |  |  |  | 
| V | Standard Extension for Vector Operations |  |  |  | 
| N | Standard Extension for User-Level Interrupts |  |  |  | 
| H | Standard Extension for Hypervisor |  |  |  | 
| S | Standard Extension for Supervisor-level Instructions |  |  |  | 

In the case of the VisionFive 2, the heart is the [SiFive U74-MC Core Complex](https://starfivetech.com/uploads/u74mc_core_complex_manual_21G1.pdf) composed of:

- 4x U7 cores: `RV64IMAFDC` (shortform: `rv64gc`). They support the double-float operations (ABI = lp64d) and compressed instructions and M+S+U modes
- 1x S7 core: `RV64IMAFDC` supports only M+U modes
- All of those cores have their own PMP (Physical Memory Protection) units

Additional extensions to take into account for the U7 cores:

- `zicsr`: CSR (Control and Status Register) Instructions; implied by the F extension
- `Zba`: address generation
- `Zbb`: basic bit manipulation

This results in the following being the descriptive and shorthand flags for the VisionFive2 board respectively: `rv64imafdc_zicsr_zba_zbb`, `rv64gc_zba_zbb`

Unsupported features for the U74:

- Virtualisation: KVM requires the H extension
- SIMD instructions: No support for vector operations (V)

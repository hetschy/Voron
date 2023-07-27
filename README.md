References to relevant documentation and resources for assembly and installation of (my) Formbot Voron 2.4R2.

End-goal being a 3D printer that can be controlled via a web interface (MainsailOS), with the following features:

- [ ] Communication over CANbus
- [ ] Sensorless homing
- [ ] Voron TAP
- [ ] Voron StealthBurner
- [ ] Nevermore filtration

## Assembly

- [V2.4R2 Assembly Manual](https://github.com/VoronDesign/Voron-2/raw/Voron2.4/Manual/Assembly_Manual_2.4r2.pdf)
- [Linear Rail Greasing Guide](https://docs.ldomotors.com/en/guides/rail_grease_guide)

Ended up using [Mobil Mobilux EP2](https://www.mobil.com/en/lubricants/for-businesses/industrial/lubricants/products/products/mobilux-ep-2) (after first applying WD White Grease + PTFE, and another round of cleaning...). The rails now feel much smoother than with the WD product. 

1 ball bearing was removed from 1 side of the block on my MGN12 to, seemingly, even the numbers. Again, felt way smoother after. 

### Voron TAP

- https://github.com/VoronDesign/Voron-Tap

### Voron StealthBurner

- https://github.com/VoronDesign/Voron-Stealthburner

## MCU (BTT Octopus V1.1)

[Github: BTT Octopus](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0)
- [Overview](https://github.com/bigtreetech/docs/blob/master/docs/Octopus.md)
- [Wiring](https://docs.vorondesign.com/build/electrical/v2_octopus_wiring.html)
- [Pinout](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/blob/master/Hardware/BIGTREETECH%20Octopus%20-%20PIN.pdf)

Diag pins must be jumped to enable StallGuard [^1]

How to flash Klipper and install Mainsail (CAN-compatible?) [^2] [^3]

#### Firmware (Klipper)

- [Official homepage](https://www.klipper3d.org/)
- https://docs.vorondesign.com/build/software/octopus_klipper.html

#### Interface (MainsailOS)

- https://docs.mainsail.xyz/

#### Stepper Motor Driver (BTT TMC2209 V1.3)

- [TMC2209 Documentation](https://bigtreetech.github.io/docs/TMC2209.html)

#### Sensorless

- https://github.com/EricZimmerman/VoronTools/blob/main/Sensorless.md
- https://gist.github.com/clee/9108f7717defce8b1222698f816def0a

## BTT Pi V1.2

[User Manual](https://docs.google.com/document/d/1LT9zD-uSNdrcREYmAluGiTxOeXiZSYS_/edit)

- https://www.youtube.com/watch?v=IhB7zlay_ZE
- https://www.youtube.com/watch?v=Ry9Q-toA11w

## CANbus

![Wiring](https://github.com/maz0r/klipper_canbus/raw/main/images/canbus_wiring.jpg)

MCU to use UART/USB. Minimum bitrate is apparently 500k, but unclear (seen recommendations for 250k, 500k and 1000k).
I will configure my CAN-setup to use 1000000.

- https://www.klipper3d.org/CANBUS.html
- https://github.com/Esoterical/voron_canbus
- https://github.com/3DPTronics/CAN-BUS-Complete-Kit/blob/main/GUIDES/CANBUS_KIT_Instructions_v1.0.pdf

### BTT EBB36 (CAN V1.2)

- https://github.com/EricZimmerman/VoronTools/blob/main/EBB_CAN.md

Note: Jump 120Ohm resistors on both EBB36 and Canable Pro

![EBBJumper](https://user-images.githubusercontent.com/124253477/226155159-06afd94e-01fb-4256-89ec-10e59d236eac.png)![CanableJumper](/Files/Canable_jumper.png)

#### Firmware

- https://github.com/Esoterical/voron_canbus/tree/main/toolhead_flashing/common_hardware/BigTreeTech%20EBB36%20V1.2
- https://bigtreetech.github.io/docs/EBB%20Series.html

#### CANboot

Canboot is a bootloader which can be used to flash firmware on your CAN or USB toolhead board without changing your wiring!

This can greatly simplify maintenance, and is seen by many as a great quality of life improvement.

<mark>Before proceeding however it is critical that your CAN network is configured for your printer, failure to setup the network will cause a problem when you try to connect devices :) click here and select your controller for setup instructions!</mark>

EBB V1.2 should flash exactly the same as the 1.1 board but without the heater issue.

- https://github.com/maz0r/klipper_canbus/blob/main/controller/canboot.md
- https://github.com/maz0r/klipper_canbus/blob/main/toolhead/ebb36-42_v1.1.md
- https://github.com/maz0r/klipper_canbus/blob/main/toolhead/ebb36-42_v1.2.md
- https://github.com/Esoterical/voron_canbus/tree/main/toolhead_flashing/common_hardware/BigTreeTech%20EBB36%20V1.2

You want the Processor, Clock Reference, and Application Start offset to be set as per whatever board you are running. Make sure "Communication Interface" is set to "CAN bus" with the appropriate pins for your board. Also make sure the "Support bootloader entry on rapid double click of reset button" is marked. It makes it so a double press of the reset button will force the board into CanBOOT mode. Makes re-flashing after a mistake a lot easier.

![Reset](https://user-images.githubusercontent.com/124253477/221349624-69abcf3e-dfd8-48d0-b4f6-0ebd620f6b42.png)

**Updating CANboot and Klipper**

- https://github.com/Esoterical/voron_canbus/blob/main/toolhead_flashing/README.md#updating

### MKS CANable Pro V1.0

- https://github.com/makerbase-mks/CANable-MKS/tree/main/Hardware/MKS%20CANable%20Pro-V1.0
- https://github.com/maz0r/klipper_canbus/blob/main/controller/canable.md

#### Firmware

- https://github.com/maz0r/klipper_canbus/blob/main/controller/candlelight_fw.md
- [candleLight](https://github.com/maz0r/klipper_canbus/blob/main/controller/candlelight_fw.md) (a8a0757)
    - https://www.youtube.com/watch?v=6MChPbeG6D0


## Future purchases to consider:

- [MAX31865 V2.0](https://github.com/bigtreetech/BIGTREETECH-MAX31865/tree/master): For temperature sensors (chamber, unsure if needed or not)

## Future upgrades to consider:

- [KAMP: Klipper Adaptive Meshing & Purging](https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging): 
- [A better print_start macro](https://github.com/jontek2/A-better-print_start-macro)
- [Klippain](https://github.com/Frix-x/klippain): To generate config files


[^1]: https://www.youtube.com/watch?v=v9ahI2IsPV0
[^2]: https://www.youtube.com/watch?v=crquR-PHeDg
[^3]: https://www.youtube.com/watch?v=O_5F9NnVSmM
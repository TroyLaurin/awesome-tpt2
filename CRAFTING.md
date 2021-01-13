# Crafting

## Usage

This is a combination of global utility scripts that let you choose what you want to do, plus the crafting scripts which execute your choice.  Note that crafting assumes that you have enough ingots of the correct tiers to build what you want, and will stop before completing the output if you don't have enough ingots.  If you craft more ingots and start construction again then it should pick up where it left off without crafting extra waste products.

1. Choose the TIER you want to produce with '1' (loops from 1 to 10)
2. Choose the MODE with '2' and OUTPUT with '3'.  Refer to the following table to see what will be produced:
   Optionally, when in **producer** mode, you can hit '4' while in the building corresponding to the producer you want to craft, or when in **machine** mode you can hit '4' with the mouse cursor over the machine you want to craft in the machines tab in the factory.

| MODE | 1 (producers) | 2 (machines) | 3 (parts) |
| ---: | --- | --- | --- |
| **OUTPUT** | | | |
| 1      | White (town)              | Oven      | Chips (T1-5)       |
| 2      | Yellow (powerplant)       | Assembler | Plates             |
| 3      | Orange (mine)             | Refiner   | Dense plates       |
| 4      | Red (factory)             | Crusher   | Boards             |
| 5      | Purple (headquarters)     | Cutter    | Blocks             |
| 6      | Pink (arcade)             | Presser   | Pipes              |
| 7      | Green (laboratory)        | Mixer     | Cables             |
| 8      | Cyan (shipyard)           | Belt      | Wires (coil thing) |
| 9      | Light blue (trading post) | Shaper    | Insulated cables   |
| 10     | Dark blue (workshop)      | Boiler    | Rods               |
| 11     | Grey (museum)             | -         | Rings              |
| 12     | Brown (construction firm) | -         | Screws             |
| 13     | Black (statue of Cubos)   | -         | Motors             |
| 14     | -                         | -         | Pumps              |

3. Choose the COUNT of items you want to produce with '8' to count down and '9' to count up.  Counts up from 1-10, then 20, 30... 90, 100, then 200, 300 etc
4. Optionally choose whether to craft ALL items for the desired output (`CRAFT_PRESERVATION = 0.0`) or to use up components that are already in your inventory when crafting the output (`CRAFT_PRESERVATION = 1.0`) - toggled with '4'.
5. Hit '0' while in the factory to start production.

## The scripts (general)

craft init (1 0 9)
```
wakeup()

global.int.set("craft_busy", 0)
global.int.set("craft_status", 200)
global.int.set("craft_require_tier", 0)
global.double.set("craft_require_count", 0.0)
global.int.set("craft_tier", 1)
global.int.set("craft_mode", 1)
global.int.set("craft_output", 1)
global.double.set("craft_count", 1.0)
global.double.set("craft_inventory", 1.0)
```

`CmNyYWZ0IGluaXQBAAAABndha2V1cAAAAAAJAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BApjcmFmdF9idXN5CGNvbnN0YW50AgAAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X3N0YXR1cwhjb25zdGFudALIAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BBJjcmFmdF9yZXF1aXJlX3RpZXIIY29uc3RhbnQCAAAAABFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQTY3JhZnRfcmVxdWlyZV9jb3VudAhjb25zdGFudAMAAAAAAAAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQKY3JhZnRfdGllcghjb25zdGFudAIBAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BApjcmFmdF9tb2RlCGNvbnN0YW50AgEAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dAhjb25zdGFudAIBAAAAEWdsb2JhbC5kb3VibGUuc2V0CGNvbnN0YW50BAtjcmFmdF9jb3VudAhjb25zdGFudAMAAAAAAADwPxFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50AwAAAAAAAPA/`

craft tier up (1 1 1)
```
:global int craft_tier

key.1()
(global.int.get("craft_busy") == 0)

craft_tier = (craft_tier % 10) + 1
```

`DWNyYWZ0IHRpZXIgdXABAAAABWtleS4xAQAAAA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfYnVzeQhjb25zdGFudAQCPT0IY29uc3RhbnQCAAAAAAEAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQECmNyYWZ0X3RpZXIOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECmNyYWZ0X3RpZXIIY29uc3RhbnQEA21vZAhjb25zdGFudAIKAAAACGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAA=`

craft tier down (0 1 1)

```
:global int craft_tier


(global.int.get("craft_busy") == 0)

craft_tier = ((craft_tier+8) % 10) + 1
```

`D2NyYWZ0IHRpZXIgZG93bgAAAAABAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BApjcmFmdF9idXN5CGNvbnN0YW50BAI9PQhjb25zdGFudAIAAAAAAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQKY3JhZnRfdGllcg5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfdGllcghjb25zdGFudAQBKwhjb25zdGFudAIIAAAACGNvbnN0YW50BANtb2QIY29uc3RhbnQCCgAAAAhjb25zdGFudAQBKwhjb25zdGFudAIBAAAA`

craft mode up (1 1 2)

```
:global int craft_mode
:global int craft_output

key.2()
(global.int.get("craft_busy") == 0)

craft_mode = (craft_mode % 3) + 1
craft_output = 1
```

`DWNyYWZ0IG1vZGUgdXABAAAABWtleS4yAQAAAA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfYnVzeQhjb25zdGFudAQCPT0IY29uc3RhbnQCAAAAAAIAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQECmNyYWZ0X21vZGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECmNyYWZ0X21vZGUIY29uc3RhbnQEA21vZAhjb25zdGFudAIDAAAACGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dAhjb25zdGFudAIBAAAA`

craft mode down (0 1 2)

```
:global int craft_mode
:global int craft_output


(global.int.get("craft_busy") == 0)

craft_mode = ((craft_mode+1) % 3) + 1
craft_output = 1
```

`D2NyYWZ0IG1vZGUgZG93bgAAAAABAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BApjcmFmdF9idXN5CGNvbnN0YW50BAI9PQhjb25zdGFudAIAAAAAAgAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQKY3JhZnRfbW9kZQ5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfbW9kZQhjb25zdGFudAQBKwhjb25zdGFudAIBAAAACGNvbnN0YW50BANtb2QIY29uc3RhbnQCAwAAAAhjb25zdGFudAQBKwhjb25zdGFudAIBAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQCAQAAAA==`

craft output up (1 1 2)

```
:global int craft_mode
:global int craft_output
:local int max

key.3()
(global.int.get("craft_busy") == 0)

max = (07101300 / (100 ^ craft_mode)) % 100
craft_output = (craft_output % max) + 1
```

`D2NyYWZ0IG91dHB1dCB1cAEAAAAFa2V5LjMBAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BApjcmFmdF9idXN5CGNvbnN0YW50BAI9PQhjb25zdGFudAIAAAAAAgAAAA1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BANtYXgOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQIY29uc3RhbnQCdFtsAAhjb25zdGFudAQBLw5hcml0aG1ldGljLmludAhjb25zdGFudAJkAAAACGNvbnN0YW50BANwb3cOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECmNyYWZ0X21vZGUIY29uc3RhbnQEA21vZAhjb25zdGFudAJkAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dAhjb25zdGFudAQDbW9kDWxvY2FsLmludC5nZXQIY29uc3RhbnQEA21heAhjb25zdGFudAQBKwhjb25zdGFudAIBAAAA`

craft output down (0 1 2)

```
:global int craft_mode
:global int craft_output
:local int max


(global.int.get("craft_busy") == 0)

max = (07101300 / (100 ^ craft_mode)) % 100
craft_output = ((craft_output + max - 2) % max) + 1
```

`EWNyYWZ0IG91dHB1dCBkb3duAAAAAAEAAAAOY29tcGFyaXNvbi5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECmNyYWZ0X2J1c3kIY29uc3RhbnQEAj09CGNvbnN0YW50AgAAAAACAAAADWxvY2FsLmludC5zZXQIY29uc3RhbnQEA21heA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludAhjb25zdGFudAJ0W2wACGNvbnN0YW50BAEvDmFyaXRobWV0aWMuaW50CGNvbnN0YW50AmQAAAAIY29uc3RhbnQEA3Bvdw5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfbW9kZQhjb25zdGFudAQDbW9kCGNvbnN0YW50AmQAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0CGNvbnN0YW50BAErDWxvY2FsLmludC5nZXQIY29uc3RhbnQEA21heAhjb25zdGFudAQBLQhjb25zdGFudAICAAAACGNvbnN0YW50BANtb2QNbG9jYWwuaW50LmdldAhjb25zdGFudAQDbWF4CGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAA=`

craft producer set (pg1) (1 1 14)

```
:global int craft_output

key.4()
(global.int.get("craft_busy") == 0 & global.int.get("craft_mode") == 1)

craft_output = 1
gotoif(a, isopen("powerplant"))
gotoif(b, isopen("mine"))
gotoif(c, isopen("factory"))
gotoif(d, isopen("headquarters"))
gotoif(e, isopen("arcade"))
gotoif(f, isopen("laboratory"))
goto(99)
f: craft_output = craft_output + 1
e: craft_output = craft_output + 1
d: craft_output = craft_output + 1
c: craft_output = craft_output + 1
b: craft_output = craft_output + 1
a: craft_output = craft_output + 1
```

`GGNyYWZ0IHByb2R1Y2VyIHNldCAocGcxKQEAAAAFa2V5LjQBAAAAD2NvbXBhcmlzb24uYm9vbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfYnVzeQhjb25zdGFudAQCPT0IY29uc3RhbnQCAAAAAAhjb25zdGFudAQBJg5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfbW9kZQhjb25zdGFudAQCPT0IY29uc3RhbnQCAQAAAA4AAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dAhjb25zdGFudAIBAAAADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50Ag4AAAASdG93bi53aW5kb3cuaXNvcGVuCGNvbnN0YW50BApwb3dlcnBsYW50DmdlbmVyaWMuZ290b2lmCGNvbnN0YW50Ag0AAAASdG93bi53aW5kb3cuaXNvcGVuCGNvbnN0YW50BARtaW5lDmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgwAAAASdG93bi53aW5kb3cuaXNvcGVuCGNvbnN0YW50BAdmYWN0b3J5DmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgsAAAASdG93bi53aW5kb3cuaXNvcGVuCGNvbnN0YW50BAxoZWFkcXVhcnRlcnMOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCCgAAABJ0b3duLndpbmRvdy5pc29wZW4IY29uc3RhbnQEBmFyY2FkZQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAIJAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQKbGFib3JhdG9yeQxnZW5lcmljLmdvdG8IY29uc3RhbnQCYwAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA==`

craft producer set (pg2) (1 1 13)

```
:global int craft_output

key.4()
(global.int.get("craft_busy") == 0 & global.int.get("craft_mode") == 1)


gotoif(a, isopen("shipyard"))
gotoif(b, isopen("tradingpost"))
gotoif(c, isopen("workshop"))
gotoif(d, isopen("museum"))
gotoif(e, isopen("constructionfirm"))
gotoif(f, isopen("statueofcubos"))
goto(99)
f: craft_output = craft_output + 1
e: craft_output = craft_output + 1
d: craft_output = craft_output + 1
c: craft_output = craft_output + 1
b: craft_output = craft_output + 1
a: craft_output = craft_output + 7
```

`GGNyYWZ0IHByb2R1Y2VyIHNldCAocGcyKQEAAAAFa2V5LjQBAAAAD2NvbXBhcmlzb24uYm9vbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfYnVzeQhjb25zdGFudAQCPT0IY29uc3RhbnQCAAAAAAhjb25zdGFudAQBJg5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfbW9kZQhjb25zdGFudAQCPT0IY29uc3RhbnQCAQAAAA0AAAAOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCDQAAABJ0b3duLndpbmRvdy5pc29wZW4IY29uc3RhbnQECHNoaXB5YXJkDmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgwAAAASdG93bi53aW5kb3cuaXNvcGVuCGNvbnN0YW50BAt0cmFkaW5ncG9zdA5nZW5lcmljLmdvdG9pZghjb25zdGFudAILAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQId29ya3Nob3AOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCCgAAABJ0b3duLndpbmRvdy5pc29wZW4IY29uc3RhbnQEBm11c2V1bQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAIJAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQQY29uc3RydWN0aW9uZmlybQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAIIAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQNc3RhdHVlb2ZjdWJvcwxnZW5lcmljLmdvdG8IY29uc3RhbnQCYwAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQIY29uc3RhbnQEASsIY29uc3RhbnQCBwAAAA==`

craft machine set (1 1 4)

```
:global int craft_output
:global int y
:global int x

key.4()
(global.int.get("craft_busy") == 0 & global.int.get("craft_mode") == 2 & isopen("factory"))

x = d2i(floor(((x(position()) / i2d(width()) - 0.41) / 0.10) + 1.0))
y = d2i(floor(((0.80 - y(position()) / i2d(height())) / 0.214)))

gotoif(99, x<1 | x>5 | y<0 | y>1)
craft_output = x + y*5
```

`EWNyYWZ0IG1hY2hpbmUgc2V0AQAAAAVrZXkuNAEAAAAPY29tcGFyaXNvbi5ib29sD2NvbXBhcmlzb24uYm9vbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfYnVzeQhjb25zdGFudAQCPT0IY29uc3RhbnQCAAAAAAhjb25zdGFudAQBJg5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfbW9kZQhjb25zdGFudAQCPT0IY29uc3RhbnQCAgAAAAhjb25zdGFudAQBJhJ0b3duLndpbmRvdy5pc29wZW4IY29uc3RhbnQEB2ZhY3RvcnkEAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAF4A2QyaQxkb3VibGUuZmxvb3IRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUGdmVjMi54Dm1vdXNlLnBvc2l0aW9uCGNvbnN0YW50BAEvA2kyZAxzY3JlZW4ud2lkdGgIY29uc3RhbnQEAS0IY29uc3RhbnQDPQrXo3A92j8IY29uc3RhbnQEAS8IY29uc3RhbnQDmpmZmZmZuT8IY29uc3RhbnQEASsIY29uc3RhbnQDAAAAAAAA8D8OZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEAXkDZDJpDGRvdWJsZS5mbG9vchFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAOamZmZmZnpPwhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZQZ2ZWMyLnkObW91c2UucG9zaXRpb24IY29uc3RhbnQEAS8DaTJkDXNjcmVlbi5oZWlnaHQIY29uc3RhbnQEAS8IY29uc3RhbnQDMQisHFpkyz8OZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCYwAAAA9jb21wYXJpc29uLmJvb2wPY29tcGFyaXNvbi5ib29sD2NvbXBhcmlzb24uYm9vbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQBeAhjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAF4CGNvbnN0YW50BAE+CGNvbnN0YW50AgUAAAAIY29uc3RhbnQEAXwOY29tcGFyaXNvbi5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEAXkIY29uc3RhbnQEATwIY29uc3RhbnQCAAAAAAhjb25zdGFudAQBfA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQBeQhjb25zdGFudAQBPghjb25zdGFudAIBAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEAXgIY29uc3RhbnQEASsOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEAXkIY29uc3RhbnQEASoIY29uc3RhbnQCBQAAAA==`


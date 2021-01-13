# Crafting

## Usage

This is a combination of global utility scripts that let you choose what you want to do, plus the crafting scripts which execute your choice.  Note that crafting assumes that you have enough ingots of the correct tiers to build what you want, and will stop before completing the output if you don't have enough ingots.  If you craft more ingots and start construction again then it should pick up where it left off without crafting extra waste products.

1. Choose the TIER you want to produce with '1' (loops from 1 to 10)
2. Choose the MODE with '2' and OUTPUT with '3'.  Refer to the following table to see what will be produced.
3. Choose the COUNT of items you want to produce with '8' to count down and '9' to count up.  Counts up from 1-10, then 20, 30... 90, 100, then 200, 300 etc
4. Optionally choose whether to craft ALL items for the desired output (`CRAFT_PRESERVATION = 0.0`) or to use up components that are already in your inventory when crafting the output (`CRAFT_PRESERVATION = 1.0`) - toggled with '4'.
5. Hit '0' while in the factory to start production.

| MODE | 1 (producers) | 2 (machines) | 3 (parts) |
| ---: | --- | --- | --- |
| **OUTPUT** | | | |
| 1      | White (town)              | Oven      | Chips (T1-5)       |
| 2      | Yellow (powerplant)       | Assembler | Plates             |
| 3      | Orange (mine)             | Refiner   | Dense plates       |
| 4      | Red (factory)             | Crusher   | Blocks             |
| 5      | Purple (headquarters)     | Cutter    | Cables             |
| 6      | Pink (arcade)             | Presser   | Insulated cables   |
| 7      | Green (laboratory)        | Mixer     | Rods               |
| 8      | Cyan (shipyard)           | Belt      | Motors             |
| 9      | Light blue (trading post) | Shaper    | Pumps              |
| 10     | Dark blue (workshop)      | Boiler    | -                  |
| 11     | Grey (museum)             | -         | -                  |
| 12     | Brown (construction firm) | -         | -                  |
| 13     | Black (statue of Cubos)   | -         | -                  |

Optionally, when in **producer** mode, you can hit '4' while in the building corresponding to the producer you want to craft, or when in **machine** mode you can hit '4' with the mouse cursor over the machine you want to craft in the machines tab in the factory.

The `CRAFT_STATUS` global variable is inspired by HTTP status codes to tell you what happened in the craft:
* 102 means "Processing", or the craft is still running
* 412 means "Precondition Failed", or that you don't have enough dust/ingots at some tier to complete the craft.  The tier and amount that is needed is shown in the `CRAFT_REQUIRED_TIER` and `CRAFT_REQUIRED_COUNT` global variables.

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

max = (13101300 / (100 ^ craft_mode)) % 100
craft_output = (craft_output % max) + 1
```

`D2NyYWZ0IG91dHB1dCB1cAEAAAAFa2V5LjMBAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BApjcmFmdF9idXN5CGNvbnN0YW50BAI9PQhjb25zdGFudAIAAAAAAgAAAA1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BANtYXgOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQIY29uc3RhbnQC9OjHAAhjb25zdGFudAQBLw5hcml0aG1ldGljLmludAhjb25zdGFudAJkAAAACGNvbnN0YW50BANwb3cOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECmNyYWZ0X21vZGUIY29uc3RhbnQEA21vZAhjb25zdGFudAJkAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAxjcmFmdF9vdXRwdXQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dAhjb25zdGFudAQDbW9kDWxvY2FsLmludC5nZXQIY29uc3RhbnQEA21heAhjb25zdGFudAQBKwhjb25zdGFudAIBAAAA`

craft output down (0 1 2)

```
:global int craft_mode
:global int craft_output
:local int max


(global.int.get("craft_busy") == 0)

max = (13101300 / (100 ^ craft_mode)) % 100
craft_output = ((craft_output + max - 2) % max) + 1
```

`EWNyYWZ0IG91dHB1dCBkb3duAAAAAAEAAAAOY29tcGFyaXNvbi5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECmNyYWZ0X2J1c3kIY29uc3RhbnQEAj09CGNvbnN0YW50AgAAAAACAAAADWxvY2FsLmludC5zZXQIY29uc3RhbnQEA21heA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludAhjb25zdGFudAL06McACGNvbnN0YW50BAEvDmFyaXRobWV0aWMuaW50CGNvbnN0YW50AmQAAAAIY29uc3RhbnQEA3Bvdw5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfbW9kZQhjb25zdGFudAQDbW9kCGNvbnN0YW50AmQAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0CGNvbnN0YW50BAErDWxvY2FsLmludC5nZXQIY29uc3RhbnQEA21heAhjb25zdGFudAQBLQhjb25zdGFudAICAAAACGNvbnN0YW50BANtb2QNbG9jYWwuaW50LmdldAhjb25zdGFudAQDbWF4CGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAA=`

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

## Parts scripts

craft ingot (0 1 12)

```
:local int tier
:local double count

(isopen("factory"))

tier = global.int.get("craft_tier:ingot")
count = global.double.get("craft_count:ingot")
gotoif(99, tier < 1 | tier > 10 | count <= 0.0)

gotoif(99, count <= count("ingot", tier))
gotoif(bad, count > count("ingot", tier) + count("dust", tier) - 1.0)
  waitwhile(active("oven"))
  produce("dust", tier, ceil(count - count("ingot", tier)), "oven")
  waituntil(count("ingot", tier) >= count)
  goto(99) ; ok

bad: global.int.set("craft_require_tier", tier)
  global.double.set("craft_require_count", count)
  global.int.set("craft_status", 412)
```

`C2NyYWZ0IGluZ290AAAAAAEAAAASdG93bi53aW5kb3cuaXNvcGVuCGNvbnN0YW50BAdmYWN0b3J5DAAAAA1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BAR0aWVyDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BBBjcmFmdF90aWVyOmluZ290EGxvY2FsLmRvdWJsZS5zZXQIY29uc3RhbnQEBWNvdW50EWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBFjcmFmdF9jb3VudDppbmdvdA5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAD2NvbXBhcmlzb24uYm9vbA9jb21wYXJpc29uLmJvb2wOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCCgAAAAhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQCPD0IY29uc3RhbnQDAAAAAAAAAAAOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCYwAAABFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQCPD0TZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFaW5nb3QNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcg5nZW5lcmljLmdvdG9pZghjb25zdGFudAIKAAAAEWNvbXBhcmlzb24uZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAE+EWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWluZ290DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQEZHVzdA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEtCGNvbnN0YW50AwAAAAAAAPA/EWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEBG92ZW4PZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BARkdXN0DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXILZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEAS0TZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFaW5nb3QNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQEb3ZlbhFnZW5lcmljLndhaXR1bnRpbBFjb21wYXJpc29uLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVpbmdvdA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAI+PRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAxnZW5lcmljLmdvdG8IY29uc3RhbnQCYwAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQSY3JhZnRfcmVxdWlyZV90aWVyDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIRZ2xvYmFsLmRvdWJsZS5zZXQIY29uc3RhbnQEE2NyYWZ0X3JlcXVpcmVfY291bnQQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X3N0YXR1cwhjb25zdGFudAKcAQAA`

### Chips

craft part:1 (0 1 20)

```
; Chips
:global double craft_inventory
:local int tier
:local double count

(isopen("factory"))

tier = global.int.get("craft_tier:3.1")
count = global.double.get("craft_count:3.1")
gotoif(99, tier < 1 | tier > 10 | count < 1.0)

; Ingots (lo)
;   board_lo = global.double.get("craft_count:3.1") * i2d((864410 / (10 ^ mytier)) % 10)
;   circuit_lo = global.double.get("craft_count:3.1") * 2.0
global.int.set("craft_tier:ingot", tier * 2 - 1)
global.double.set("craft_count:ingot", max(0.0, count * i2d((864410 / (10 ^ tier)) % 10) - craft_inventory * (count("plate", tier * 2 - 1) + count("plate.circuit", tier * 2 - 1))) + max(0.0, ceil((count*2.0 - craft_inventory * (count("cable", tier * 2 - 1) + count("circuit", tier * 2 - 1)))/2.0)))
executesync("craft ingot")
gotoif(99, global.int.get("craft_status") > 199)

; Ingots (hi)
;   board_hi = global.double.get("craft_count:3.1") * i2d((862210 / (10 ^ mytier)) % 10)
;   circuit_hi = global.double.get("craft_count:3.1") * i2d((224420 / (10 ^ mytier)) % 10)
global.int.set("craft_tier:ingot", tier * 2)
global.double.set("craft_count:ingot", max(0.0, count * i2d((862210 / (10 ^ tier)) % 10) - craft_inventory * (count("plate", tier * 2) + count("plate.circuit", tier * 2))) + max(0.0, ceil((count * i2d((224420 / (10 ^ tier)) % 10) - craft_inventory * (count("cable", tier * 2) + count("circuit", tier * 2)))/2.0)))
executesync("craft ingot")
gotoif(99, global.int.get("craft_status") > 199)

; Input chips
global.int.set("craft_tier:3.1", tier - 1)
global.double.set("craft_count:3.1", count * 2.0 * (floor(664200.0 / (10.0 ^ i2d(tier))) % 10.0) - craft_inventory * count("chip", tier - 1))
executesync("craft part:1")
gotoif(99, global.int.get("craft_status") > 199)

global.int.set("craft_status:3.1", 1)

execute("craft part:1:board")
execute("craft part:1:circuit")

waitwhile(global.int.get("craft_status:3.1") < 7)
craft("chip", global.int.get("craft_tier:3.1"), global.double.get("craft_count:3.1"))
```

`DGNyYWZ0IHBhcnQ6MQAAAAABAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQHZmFjdG9yeRQAAAANbG9jYWwuaW50LnNldAhjb25zdGFudAQEdGllcg5nbG9iYWwuaW50LmdldAhjb25zdGFudAQOY3JhZnRfdGllcjozLjEQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuMQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAD2NvbXBhcmlzb24uYm9vbA9jb21wYXJpc29uLmJvb2wOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCCgAAAAhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPw5nbG9iYWwuaW50LnNldAhjb25zdGFudAQQY3JhZnRfdGllcjppbmdvdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAABFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQRY3JhZnRfY291bnQ6aW5nb3QRYXJpdGhtZXRpYy5kb3VibGUKZG91YmxlLm1heAhjb25zdGFudAMAAAAAAAAAABFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKgNpMmQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQIY29uc3RhbnQCmjANAAhjb25zdGFudAQBLw5hcml0aG1ldGljLmludAhjb25zdGFudAIKAAAACGNvbnN0YW50BANwb3cNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQDbW9kCGNvbnN0YW50AgoAAAAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQBKxNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BA1wbGF0ZS5jaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAACGNvbnN0YW50BAErCmRvdWJsZS5tYXgIY29uc3RhbnQDAAAAAAAAAAALZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoIY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVjYWJsZQ5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQBKxNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAACGNvbnN0YW50BAEvCGNvbnN0YW50AwAAAAAAAABAE2dlbmVyaWMuZXhlY3V0ZXN5bmMIY29uc3RhbnQEC2NyYWZ0IGluZ290DmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AmMAAAAOY29tcGFyaXNvbi5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEDGNyYWZ0X3N0YXR1cwhjb25zdGFudAQBPghjb25zdGFudALHAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BBBjcmFmdF90aWVyOmluZ290DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAABFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQRY3JhZnRfY291bnQ6aW5nb3QRYXJpdGhtZXRpYy5kb3VibGUKZG91YmxlLm1heAhjb25zdGFudAMAAAAAAAAAABFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKgNpMmQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQIY29uc3RhbnQCAigNAAhjb25zdGFudAQBLw5hcml0aG1ldGljLmludAhjb25zdGFudAIKAAAACGNvbnN0YW50BANwb3cNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQDbW9kCGNvbnN0YW50AgoAAAAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEASsKZG91YmxlLm1heAhjb25zdGFudAMAAAAAAAAAAAtkb3VibGUuY2VpbBFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKgNpMmQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQIY29uc3RhbnQCpGwDAAhjb25zdGFudAQBLw5hcml0aG1ldGljLmludAhjb25zdGFudAIKAAAACGNvbnN0YW50BANwb3cNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQDbW9kCGNvbnN0YW50AgoAAAAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVjYWJsZQ5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQHY2lyY3VpdA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS8IY29uc3RhbnQDAAAAAAAAAEATZ2VuZXJpYy5leGVjdXRlc3luYwhjb25zdGFudAQLY3JhZnQgaW5nb3QOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCYwAAAA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQMY3JhZnRfc3RhdHVzCGNvbnN0YW50BAE+CGNvbnN0YW50AscAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDmNyYWZ0X3RpZXI6My4xDmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAABFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQPY3JhZnRfY291bnQ6My4xEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqCGNvbnN0YW50AwAAAAAAAABACGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlDGRvdWJsZS5mbG9vchFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAEEUkQQhjb25zdGFudAQBLxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAAAkQAhjb25zdGFudAQDcG93A2kyZA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BANtb2QIY29uc3RhbnQDAAAAAAAAJEAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BARjaGlwDmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAABNnZW5lcmljLmV4ZWN1dGVzeW5jCGNvbnN0YW50BAxjcmFmdCBwYXJ0OjEOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCYwAAAA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQMY3JhZnRfc3RhdHVzCGNvbnN0YW50BAE+CGNvbnN0YW50AscAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEEGNyYWZ0X3N0YXR1czozLjEIY29uc3RhbnQCAQAAAA9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQEEmNyYWZ0IHBhcnQ6MTpib2FyZA9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQEFGNyYWZ0IHBhcnQ6MTpjaXJjdWl0EWdlbmVyaWMud2FpdHdoaWxlDmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BBBjcmFmdF9zdGF0dXM6My4xCGNvbnN0YW50BAE8CGNvbnN0YW50AgcAAAANZmFjdG9yeS5jcmFmdAhjb25zdGFudAQEY2hpcA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQOY3JhZnRfdGllcjozLjERZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuMQ==`

craft part:1:board (0 0 17)

```
:global double craft_inventory
:local int tier
:local double board_lo
:local double board_hi

   tier = global.int.get("craft_tier:3.1")
   board_lo = global.double.get("craft_count:3.1") * i2d((864410 / (10 ^ tier)) % 10)
   board_hi = global.double.get("craft_count:3.1") * i2d((862210 / (10 ^ tier)) % 10)

; Craft plates
lo1: gotoif(hi1, board_lo <= craft_inventory * (count("plate.circuit", tier * 2 - 1) + count("plate", tier * 2 - 1)))
   waitwhile(active("presser"))
   produce("ingot", tier * 2 - 1, board_lo - craft_inventory * (count("plate.circuit", tier * 2 - 1) + count("plate", tier * 2 - 1)), "presser")

hi1: gotoif(lo2, board_hi <= craft_inventory * (count("plate.circuit", tier * 2) + count("plate", tier * 2)))
   waitwhile(active("presser"))
   produce("ingot", tier * 2, board_hi - craft_inventory * (count("plate.circuit", tier * 2) + count("plate", tier * 2)), "presser")

; Craft circuit boards
lo2: gotoif(hi2, board_lo <= craft_inventory * (count("plate.circuit", tier * 2 - 1)))
   waitwhile(active("refinery") | count("plate", tier * 2 - 1) < board_lo - craft_inventory * (count("plate.circuit", tier * 2 - 1)))
   produce("plate", tier * 2 - 1, board_lo - craft_inventory * (count("plate.circuit", tier * 2 - 1)), "refinery")

hi2: gotoif(wait, board_hi <= craft_inventory * (count("plate.circuit", tier * 2)))
   waitwhile(active("refinery") | count("plate", tier * 2) < board_hi - craft_inventory * (count("plate.circuit", tier * 2)))
   produce("plate", tier * 2, board_hi - craft_inventory * (count("plate.circuit", tier * 2)), "refinery")

wait: waituntil(count("plate.circuit", tier * 2 - 1) >= board_lo & count("plate.circuit", tier * 2) >= board_hi)
global.int.set("craft_status:3.1", global.int.get("craft_status:3.1") + 2)
```

`EmNyYWZ0IHBhcnQ6MTpib2FyZAAAAAAAAAAAEQAAAA1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BAR0aWVyDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BA5jcmFmdF90aWVyOjMuMRBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BAhib2FyZF9sbxFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfY291bnQ6My4xCGNvbnN0YW50BAEqA2kyZA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludAhjb25zdGFudAKaMA0ACGNvbnN0YW50BAEvDmFyaXRobWV0aWMuaW50CGNvbnN0YW50AgoAAAAIY29uc3RhbnQEA3Bvdw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BANtb2QIY29uc3RhbnQCCgAAABBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BAhib2FyZF9oaRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfY291bnQ6My4xCGNvbnN0YW50BAEqA2kyZA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludAhjb25zdGFudAICKA0ACGNvbnN0YW50BAEvDmFyaXRobWV0aWMuaW50CGNvbnN0YW50AgoAAAAIY29uc3RhbnQEA3Bvdw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BANtb2QIY29uc3RhbnQCCgAAAA5nZW5lcmljLmdvdG9pZghjb25zdGFudAIHAAAAEWNvbXBhcmlzb24uZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2xvCGNvbnN0YW50BAI8PRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDXBsYXRlLmNpcmN1aXQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFcGxhdGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAARZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQHcHJlc3Nlcg9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWluZ290DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAAEWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2xvCGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BA9jcmFmdF9pbnZlbnRvcnkIY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQBKxNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQHcHJlc3Nlcg5nZW5lcmljLmdvdG9pZghjb25zdGFudAIKAAAAEWNvbXBhcmlzb24uZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2hpCGNvbnN0YW50BAI8PRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDXBsYXRlLmNpcmN1aXQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAErE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBXBsYXRlDmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAABFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAdwcmVzc2VyD2ZhY3RvcnkucHJvZHVjZQhjb25zdGFudAQFaW5nb3QOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAAEWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2hpCGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BA9jcmFmdF9pbnZlbnRvcnkIY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFcGxhdGUOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAdwcmVzc2VyDmdlbmVyaWMuZ290b2lmCGNvbnN0YW50Ag0AAAARY29tcGFyaXNvbi5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfbG8IY29uc3RhbnQEAjw9EWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BA9jcmFmdF9pbnZlbnRvcnkIY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAABFnZW5lcmljLndhaXR3aGlsZQ9jb21wYXJpc29uLmJvb2wWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQIcmVmaW5lcnkIY29uc3RhbnQEAXwRY29tcGFyaXNvbi5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFcGxhdGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEATwRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfbG8IY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BA1wbGF0ZS5jaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAAD2ZhY3RvcnkucHJvZHVjZQhjb25zdGFudAQFcGxhdGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAARYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfbG8IY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BA1wbGF0ZS5jaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAACGNvbnN0YW50BAhyZWZpbmVyeQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAIQAAAAEWNvbXBhcmlzb24uZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2hpCGNvbnN0YW50BAI8PRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDXBsYXRlLmNpcmN1aXQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAAEWdlbmVyaWMud2FpdHdoaWxlD2NvbXBhcmlzb24uYm9vbBZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAhyZWZpbmVyeQhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEATwRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfaGkIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BA1wbGF0ZS5jaXJjdWl0DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAA9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBXBsYXRlDmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAABFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAhib2FyZF9oaQhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDXBsYXRlLmNpcmN1aXQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAhyZWZpbmVyeRFnZW5lcmljLndhaXR1bnRpbA9jb21wYXJpc29uLmJvb2wRY29tcGFyaXNvbi5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQCPj0QbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfbG8IY29uc3RhbnQEASYRY29tcGFyaXNvbi5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAj49EGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2hpDmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BBBjcmFmdF9zdGF0dXM6My4xDmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BBBjcmFmdF9zdGF0dXM6My4xCGNvbnN0YW50BAErCGNvbnN0YW50AgIAAAA=`

craft part:1:circuit (0 0 17)

```
:global double craft_inventory
:local int tier
:local double circuit_lo
:local double circuit_hi

tier = global.int.get("craft_tier:3.1")
circuit_lo = global.double.get("craft_count:3.1") * 2.0
circuit_hi = global.double.get("craft_count:3.1") * i2d((224420 / (10 ^ tier)) % 10)

; Craft cables
lo1: gotoif(hi1, circuit_lo <= craft_inventory * (count("circuit", tier * 2 - 1) + count("cable", tier * 2 - 1)))
	waitwhile(active("refinery"))
	produce("ingot", tier * 2 - 1, ceil((circuit_lo - craft_inventory * (count("circuit", tier * 2 - 1) + count("cable", tier * 2 - 1))) / 2.0), "refinery")

hi1: gotoif(lo2, circuit_hi <= craft_inventory * (count("circuit", tier * 2) + count("cable", tier * 2)))
	waitwhile(active("refinery"))
	produce("ingot", tier * 2, ceil((circuit_hi - craft_inventory * (count("circuit", tier * 2) + count("cable", tier * 2))) / 2.0), "refinery")

; Craft circuit wires
lo2: gotoif(hi2, circuit_lo <= craft_inventory * count("circuit", tier * 2 - 1))
   waitwhile(count("cable", tier * 2 - 1) < circuit_lo - craft_inventory * count("circuit", tier * 2 - 1) | active("assembler"))
   produce("cable", tier * 2 - 1, circuit_lo - craft_inventory * count("circuit", tier * 2 - 1), "assembler")

hi2: gotoif(wait, circuit_hi <= craft_inventory * count("circuit", tier * 2))
   waitwhile(count("cable", tier * 2) < circuit_hi - craft_inventory * count("circuit", tier * 2) | active("assembler"))
   produce("cable", tier * 2, circuit_hi - craft_inventory * count("circuit", tier * 2), "assembler")

wait: waituntil(circuit_lo <= count("circuit", tier * 2 - 1) & circuit_hi <= count("circuit", tier * 2))
global.int.set("craft_status:3.1", global.int.get("craft_status:3.1") + 4)
```

`FGNyYWZ0IHBhcnQ6MTpjaXJjdWl0AAAAAAAAAAARAAAADWxvY2FsLmludC5zZXQIY29uc3RhbnQEBHRpZXIOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEDmNyYWZ0X3RpZXI6My4xEGxvY2FsLmRvdWJsZS5zZXQIY29uc3RhbnQECmNpcmN1aXRfbG8RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuMQhjb25zdGFudAQBKghjb25zdGFudAMAAAAAAAAAQBBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BApjaXJjdWl0X2hpEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BA9jcmFmdF9jb3VudDozLjEIY29uc3RhbnQEASoDaTJkDmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50CGNvbnN0YW50AqRsAwAIY29uc3RhbnQEAS8OYXJpdGhtZXRpYy5pbnQIY29uc3RhbnQCCgAAAAhjb25zdGFudAQDcG93DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEA21vZAhjb25zdGFudAIKAAAADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgcAAAARY29tcGFyaXNvbi5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9sbwhjb25zdGFudAQCPD0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAACGNvbnN0YW50BAErE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQECHJlZmluZXJ5D2ZhY3RvcnkucHJvZHVjZQhjb25zdGFudAQFaW5nb3QOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAALZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9sbwhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEB2NpcmN1aXQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFY2FibGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEAS8IY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQECHJlZmluZXJ5DmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgoAAAARY29tcGFyaXNvbi5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9oaQhjb25zdGFudAQCPD0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBKxNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVjYWJsZQ5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAARZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQIcmVmaW5lcnkPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAVpbmdvdA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAALZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9oaQhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEB2NpcmN1aXQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAErE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLwhjb25zdGFudAMAAAAAAAAAQAhjb25zdGFudAQIcmVmaW5lcnkOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCDQAAABFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2xvCGNvbnN0YW50BAI8PRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEB2NpcmN1aXQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAARZ2VuZXJpYy53YWl0d2hpbGUPY29tcGFyaXNvbi5ib29sEWNvbXBhcmlzb24uZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAACGNvbnN0YW50BAE8EWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECmNpcmN1aXRfbG8IY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8FmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQECWFzc2VtYmxlcg9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWNhYmxlDmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAAEWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECmNpcmN1aXRfbG8IY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAACGNvbnN0YW50BAlhc3NlbWJsZXIOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCEAAAABFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2hpCGNvbnN0YW50BAI8PRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEB2NpcmN1aXQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAAEWdlbmVyaWMud2FpdHdoaWxlD2NvbXBhcmlzb24uYm9vbBFjb21wYXJpc29uLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVjYWJsZQ5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEATwRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9oaQhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEB2NpcmN1aXQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAF8FmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQECWFzc2VtYmxlcg9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWNhYmxlDmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAABFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2hpCGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BA9jcmFmdF9pbnZlbnRvcnkIY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQHY2lyY3VpdA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQECWFzc2VtYmxlchFnZW5lcmljLndhaXR1bnRpbA9jb21wYXJpc29uLmJvb2wRY29tcGFyaXNvbi5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9sbwhjb25zdGFudAQCPD0TZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQHY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQBJhFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2hpCGNvbnN0YW50BAI8PRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQQY3JhZnRfc3RhdHVzOjMuMQ5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQQY3JhZnRfc3RhdHVzOjMuMQhjb25zdGFudAQBKwhjb25zdGFudAIEAAAA`

### Plates

craft part:2 (0 1 9)

```
; Regular plates
:global int craft_status
:local double count
:local int tier

isopen("factory")

tier = global.int.get("craft_tier:3.2")
count = global.double.get("craft_count:3.2")
gotoif(99, tier < 1 | tier > 10 | count < 1.0)

global.int.set("craft_tier:ingot", tier)
global.double.set("craft_count:ingot", count)
executesync("craft ingot")
gotoif(99, craft_status > 199)

waitwhile(active("presser"))
produce("ingot", tier, count, "presser")
```

`DGNyYWZ0IHBhcnQ6MgAAAAABAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQHZmFjdG9yeQkAAAANbG9jYWwuaW50LnNldAhjb25zdGFudAQEdGllcg5nbG9iYWwuaW50LmdldAhjb25zdGFudAQOY3JhZnRfdGllcjozLjIQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuMg5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAD2NvbXBhcmlzb24uYm9vbA9jb21wYXJpc29uLmJvb2wOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCCgAAAAhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPw5nbG9iYWwuaW50LnNldAhjb25zdGFudAQQY3JhZnRfdGllcjppbmdvdA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEWdsb2JhbC5kb3VibGUuc2V0CGNvbnN0YW50BBFjcmFmdF9jb3VudDppbmdvdBBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudBNnZW5lcmljLmV4ZWN1dGVzeW5jCGNvbnN0YW50BAtjcmFmdCBpbmdvdA5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9zdGF0dXMIY29uc3RhbnQEAT4IY29uc3RhbnQCxwAAABFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAdwcmVzc2VyD2ZhY3RvcnkucHJvZHVjZQhjb25zdGFudAQFaW5nb3QNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllchBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQHcHJlc3Nlcg==`

### Dense plates

craft part:3 (0 1 14)

```
; Dense plates
:global double craft_inventory
:local int tier
:local double count
:local double plates

(isopen("factory"))

tier = global.int.get("craft_tier:3.3")
count = global.double.get("craft_count:3.3")
gotoif(99, tier < 1 | tier > 10 | count < 1.0)

plates = count * 9.0 - craft_inventory * count("plate.stack", tier)

global.int.set("craft_tier:ingot", tier)
global.double.set("craft_count:ingot", plates)
executesync("craft ingot")
gotoif(99, global.int.get("craft_status") > 199)

plates: gotoif(stacks, plates <= craft_inventory * count("plate", tier))
  waitwhile(active("presser"))
  produce("ingot", tier, plates - craft_inventory * count("plate", tier), "presser")

stacks: waitwhile(count("plate", tier) < plates | active("presser"))
  craft("plate.stack", tier, count - craft_inventory * count("plate.stack", tier))
  produce("plate.stack", tier, count, "presser")
```

`DGNyYWZ0IHBhcnQ6MwAAAAABAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQHZmFjdG9yeQ4AAAANbG9jYWwuaW50LnNldAhjb25zdGFudAQEdGllcg5nbG9iYWwuaW50LmdldAhjb25zdGFudAQOY3JhZnRfdGllcjozLjMQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuMw5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAD2NvbXBhcmlzb24uYm9vbA9jb21wYXJpc29uLmJvb2wOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCCgAAAAhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPxBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BAZwbGF0ZXMRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoIY29uc3RhbnQDAAAAAAAAIkAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAtwbGF0ZS5zdGFjaw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyDmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BBBjcmFmdF90aWVyOmluZ290DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIRZ2xvYmFsLmRvdWJsZS5zZXQIY29uc3RhbnQEEWNyYWZ0X2NvdW50OmluZ290EGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBnBsYXRlcxNnZW5lcmljLmV4ZWN1dGVzeW5jCGNvbnN0YW50BAtjcmFmdCBpbmdvdA5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9zdGF0dXMIY29uc3RhbnQEAT4IY29uc3RhbnQCxwAAAA5nZW5lcmljLmdvdG9pZghjb25zdGFudAIMAAAAEWNvbXBhcmlzb24uZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBnBsYXRlcwhjb25zdGFudAQCPD0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEB3ByZXNzZXIPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAVpbmdvdA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBnBsYXRlcwhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBXBsYXRlDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEB3ByZXNzZXIRZ2VuZXJpYy53YWl0d2hpbGUPY29tcGFyaXNvbi5ib29sEWNvbXBhcmlzb24uZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBXBsYXRlDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEATwQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQGcGxhdGVzCGNvbnN0YW50BAF8FmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEB3ByZXNzZXINZmFjdG9yeS5jcmFmdAhjb25zdGFudAQLcGxhdGUuc3RhY2sNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllchFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQPY3JhZnRfaW52ZW50b3J5CGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEC3BsYXRlLnN0YWNrDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAtwbGF0ZS5zdGFjaw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAdwcmVzc2Vy`

### Blocks

craft part:4 (0 1 10)

```
; Blocks
:global double craft_inventory
:local int tier
:local double count
:local double platesperblock
:local double stacks
:local double plates

(isopen("factory"))

tier = global.int.get("craft_tier:3.4")
count = global.double.get("craft_count:3.4")
gotoif(99, tier < 1 | tier > 10 | count < 1.0)

plates: platesperblock = 4.0 * round((33332222220.0 / (10.0 ^ i2d(tier))) % 10.0)
  global.int.set("craft_tier:3.3", tier)
  global.double.set("craft_count:3.3", count * platesperblock - craft_inventory * count("plate.dense", tier))
  executesync("craft part:3")
gotoif(99, global.int.get("craft_status") > 199)

waitwhile(count("plate.dense", tier) < count * platesperblock)
craft("block", tier, count)
```

`DGNyYWZ0IHBhcnQ6NAAAAAABAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQHZmFjdG9yeQoAAAANbG9jYWwuaW50LnNldAhjb25zdGFudAQEdGllcg5nbG9iYWwuaW50LmdldAhjb25zdGFudAQOY3JhZnRfdGllcjozLjQQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuNA5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAD2NvbXBhcmlzb24uYm9vbA9jb21wYXJpc29uLmJvb2wOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCCgAAAAhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPxBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BA5wbGF0ZXNwZXJibG9jaxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAAAQQAhjb25zdGFudAQBKgxkb3VibGUucm91bmQRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAwZAULH0IIY29uc3RhbnQEAS8RYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAAJEAIY29uc3RhbnQEA3BvdwNpMmQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQDbW9kCGNvbnN0YW50AwAAAAAAACRADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BA5jcmFmdF90aWVyOjMuMw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEWdsb2JhbC5kb3VibGUuc2V0CGNvbnN0YW50BA9jcmFmdF9jb3VudDozLjMRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQOcGxhdGVzcGVyYmxvY2sIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2ludmVudG9yeQhjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAtwbGF0ZS5kZW5zZQ1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyE2dlbmVyaWMuZXhlY3V0ZXN5bmMIY29uc3RhbnQEDGNyYWZ0IHBhcnQ6Mw5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9zdGF0dXMIY29uc3RhbnQEAT4IY29uc3RhbnQCxwAAABFnZW5lcmljLndhaXR3aGlsZRFjb21wYXJpc29uLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAtwbGF0ZS5kZW5zZQ1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAE8EWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEDnBsYXRlc3BlcmJsb2NrDWZhY3RvcnkuY3JhZnQIY29uc3RhbnQEBWJsb2NrDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQ=`

### Cables

craft part:5 (0 1 9)

```
; Cables
:global double craft_inventory
:local int tier
:local double count

(isopen("factory"))

tier = global.int.get("craft_tier:3.5")
count = global.double.get("craft_count:3.5")
gotoif(99, tier < 1 | tier > 10 | count < 1.0)

global.int.set("craft_tier:ingot", tier)
global.double.set("craft_count:ingot", count / 2.0)
executesync("craft ingot")
gotoif(99, global.int.get("craft_status") > 199)

waitwhile(active("refinery"))
produce("ingot", tier, ceil(count / 2.0), "refinery")
```

`DGNyYWZ0IHBhcnQ6NQAAAAABAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQHZmFjdG9yeQkAAAANbG9jYWwuaW50LnNldAhjb25zdGFudAQEdGllcg5nbG9iYWwuaW50LmdldAhjb25zdGFudAQOY3JhZnRfdGllcjozLjUQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuNQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAD2NvbXBhcmlzb24uYm9vbA9jb21wYXJpc29uLmJvb2wOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCCgAAAAhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPw5nbG9iYWwuaW50LnNldAhjb25zdGFudAQQY3JhZnRfdGllcjppbmdvdA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEWdsb2JhbC5kb3VibGUuc2V0CGNvbnN0YW50BBFjcmFmdF9jb3VudDppbmdvdBFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBLwhjb25zdGFudAMAAAAAAAAAQBNnZW5lcmljLmV4ZWN1dGVzeW5jCGNvbnN0YW50BAtjcmFmdCBpbmdvdA5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9zdGF0dXMIY29uc3RhbnQEAT4IY29uc3RhbnQCxwAAABFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAhyZWZpbmVyeQ9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWluZ290DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXILZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEAS8IY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQECHJlZmluZXJ5`

### Insulated cables

craft part:6 (0 1 17)

```
; Insulated cables
:global double craft_inventory
:local int tier
:local double count
:local double cables
:local double rubber

(isopen("factory"))

tier = global.int.get("craft_tier:3.6")
count = global.double.get("craft_count:3.6")
gotoif(99, tier < 1 | tier > 10 | count < 1.0)

cables = count * max(max(1.0, i2d(tier) - 2.0), max(10.0 - (5.0 * ((i2d(tier) - 8.0) ^ 2.0)), ceil(((i2d(tier) - 1.0) ^ 1.5) - 11.0)))
rubber = count * max(0.0, ((2.0 * i2d(tier)) - 4.0) - max(0.0, 2.0 - ((i2d(tier) - 8.0) * (i2d(tier) - 9.0))))

global.int.set("craft_tier:ingot", tier)
global.double.set("craft_count:ingot", (cables - count("cable", tier)) / 2.0)
executesync("craft ingot")
gotoif(99, global.int.get("craft_status") > 199)

cables: gotoif(rubber, cables <= craft_inventory * count("cable", tier))
  waitwhile(active("refinery"))
  produce("ingot", tier, ceil((cables - craft_inventory * count("cable", tier)) / 2.0), "refinery")

rubber: gotoif(craft, rubber <= count("plate.rubber", 1))
  waitwhile(active("presser"))
  produce("rubber", tier, rubber, "presser")

craft: waituntil(count("cable", tier) >= cables & count("plate.rubber", 1) >= rubber)
craft("cable.insulated", tier, count)
```

`DGNyYWZ0IHBhcnQ6NgAAAAABAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQHZmFjdG9yeREAAAANbG9jYWwuaW50LnNldAhjb25zdGFudAQEdGllcg5nbG9iYWwuaW50LmdldAhjb25zdGFudAQOY3JhZnRfdGllcjozLjYQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuNg5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAD2NvbXBhcmlzb24uYm9vbA9jb21wYXJpc29uLmJvb2wOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCCgAAAAhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPxBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BAZjYWJsZXMRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoKZG91YmxlLm1heApkb3VibGUubWF4CGNvbnN0YW50AwAAAAAAAPA/EWFyaXRobWV0aWMuZG91YmxlA2kyZA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEtCGNvbnN0YW50AwAAAAAAAABACmRvdWJsZS5tYXgRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAAJEAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAAFEAIY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUDaTJkDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQDAAAAAAAAIEAIY29uc3RhbnQEA3Bvdwhjb25zdGFudAMAAAAAAAAAQAtkb3VibGUuY2VpbBFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZQNpMmQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBLQhjb25zdGFudAMAAAAAAADwPwhjb25zdGFudAQDcG93CGNvbnN0YW50AwAAAAAAAPg/CGNvbnN0YW50BAEtCGNvbnN0YW50AwAAAAAAACZAEGxvY2FsLmRvdWJsZS5zZXQIY29uc3RhbnQEBnJ1YmJlchFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKgpkb3VibGUubWF4CGNvbnN0YW50AwAAAAAAAAAAEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAAABACGNvbnN0YW50BAEqA2kyZA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEtCGNvbnN0YW50AwAAAAAAABBACGNvbnN0YW50BAEtCmRvdWJsZS5tYXgIY29uc3RhbnQDAAAAAAAAAAARYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUDaTJkDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQDAAAAAAAAIEAIY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGUDaTJkDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQDAAAAAAAAIkAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEEGNyYWZ0X3RpZXI6aW5nb3QNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllchFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQRY3JhZnRfY291bnQ6aW5nb3QRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQGY2FibGVzCGNvbnN0YW50BAEtE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS8IY29uc3RhbnQDAAAAAAAAAEATZ2VuZXJpYy5leGVjdXRlc3luYwhjb25zdGFudAQLY3JhZnQgaW5nb3QOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCYwAAAA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQMY3JhZnRfc3RhdHVzCGNvbnN0YW50BAE+CGNvbnN0YW50AscAAAAOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCDQAAABFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAZjYWJsZXMIY29uc3RhbnQEAjw9EWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BA9jcmFmdF9pbnZlbnRvcnkIY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFY2FibGUNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllchFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAhyZWZpbmVyeQ9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWluZ290DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXILZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQGY2FibGVzCGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BA9jcmFmdF9pbnZlbnRvcnkIY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFY2FibGUNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBLwhjb25zdGFudAMAAAAAAAAAQAhjb25zdGFudAQIcmVmaW5lcnkOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCEAAAABFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAZydWJiZXIIY29uc3RhbnQEAjw9E2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDHBsYXRlLnJ1YmJlcghjb25zdGFudAIBAAAAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEB3ByZXNzZXIPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAZydWJiZXINbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllchBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAZydWJiZXIIY29uc3RhbnQEB3ByZXNzZXIRZ2VuZXJpYy53YWl0dW50aWwPY29tcGFyaXNvbi5ib29sEWNvbXBhcmlzb24uZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAj49EGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBmNhYmxlcwhjb25zdGFudAQBJhFjb21wYXJpc29uLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAxwbGF0ZS5ydWJiZXIIY29uc3RhbnQCAQAAAAhjb25zdGFudAQCPj0QbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQGcnViYmVyDWZhY3RvcnkuY3JhZnQIY29uc3RhbnQED2NhYmxlLmluc3VsYXRlZA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50`

### Rods

craft part:7 (0 1 9)

```
; Rods
:global double craft_inventory
:local int tier
:local double count

(isopen("factory"))

tier = global.int.get("craft_tier:3.7")
count = global.double.get("craft_count:3.7")
gotoif(99, tier < 1 | tier > 10 | count < 1.0)

global.int.set("craft_tier:ingot", tier)
global.double.set("craft_count:ingot", count / 2.0)
executesync("craft ingot")
gotoif(99, global.int.get("craft_status") > 199)

waitwhile(active("refinery"))
produce("ingot", tier, ceil(count / 2.0), "shaper")
```

`DGNyYWZ0IHBhcnQ6NwAAAAABAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQHZmFjdG9yeQkAAAANbG9jYWwuaW50LnNldAhjb25zdGFudAQEdGllcg5nbG9iYWwuaW50LmdldAhjb25zdGFudAQOY3JhZnRfdGllcjozLjcQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQED2NyYWZ0X2NvdW50OjMuNw5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAD2NvbXBhcmlzb24uYm9vbA9jb21wYXJpc29uLmJvb2wOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBPAhjb25zdGFudAIBAAAACGNvbnN0YW50BAF8DmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCCgAAAAhjb25zdGFudAQBfBFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPw5nbG9iYWwuaW50LnNldAhjb25zdGFudAQQY3JhZnRfdGllcjppbmdvdA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEWdsb2JhbC5kb3VibGUuc2V0CGNvbnN0YW50BBFjcmFmdF9jb3VudDppbmdvdBFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBLwhjb25zdGFudAMAAAAAAAAAQBNnZW5lcmljLmV4ZWN1dGVzeW5jCGNvbnN0YW50BAtjcmFmdCBpbmdvdA5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdF9zdGF0dXMIY29uc3RhbnQEAT4IY29uc3RhbnQCxwAAABFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAhyZWZpbmVyeQ9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWluZ290DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHRpZXILZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEAS8IY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQEBnNoYXBlcg==`


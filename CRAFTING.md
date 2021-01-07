# Crafting

## Usage

This is a combination of global utility scripts that let you choose what you want to do, plus the crafting scripts which execute your choice.  Note that crafting assumes that you have enough ingots of the correct tiers to build what you want, and will stop before completing the output if you don't have enough ingots.  If you craft more ingots and start construction again then it should pick up where it left off without crafting extra waste products.

1. Choose the TIER you want to produce with '1' (loops from 1 to 10)
2. Choose the MODE with '2' and OUTPUT with '3'.  Refer to the following table to see what will be produced:

| MODE | 1 (parts) | 2 (machines) | 3 (producers) |
| ---: | --- | --- | --- |
| **OUTPUT** | | | |
| 1      | Chips            | Oven      | White |
| 2      | Insulated cables | Assembler | Blue |
| 3      | Pumps            | Refiner   | Orange |
| 4      | Motors           | Crusher   | Brown |
| 5      | Cubes            | Cutter    | TODO |
| 6      | Dense Plates     | Presser   | TODO |
| 7      | Dust (tier up)   | Mixer     | TODO |
| 8      | -                | Belt      | TODO |
| 9      | -                | Shaper    | TODO |
| 10     | -                | Boiler    | TODO |
| 11     | -                | -         | TODO |
| 12     | -                | -         | TODO |
| 13     | -                | -         | Black |

3. Choose the COUNT of items you want to produce with '8' to count down and '9' to count up.  Counts up from 1-10, then 20, 30... 90, 100, then 200, 300 etc
4. Optionally choose whether to craft ALL items for the desired output (`CRAFT_PRESERVATION = 0.0`) or to use up components that are already in your inventory when crafting the output (`CRAFT_PRESERVATION = 1.0`) - toggled with '4'.
5. Hit '0' while in the factory to start production.

## The scripts (general)

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>global_tier</td><td>1</td><td>2 1 1</td><td>

```
:global int tier
:global int crafting

wakeup()
key.1()

(crafting == 0)

tier = (tier % 10) + 1    
```

</td>
</tr>
</table>

`C2dsb2JhbF90aWVyAgAAAAZ3YWtldXAFa2V5LjEBAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAhjcmFmdGluZwhjb25zdGFudAQCPT0IY29uc3RhbnQCAAAAAAEAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEBHRpZXIOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEA21vZAhjb25zdGFudAIKAAAACGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAA=`

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>global_craft_mode</td><td>2</td><td>2 1 2</td><td>

```
:global int craft_mode
:global int craft_output
:global int crafting

wakeup()
key.2()

(crafting == 0)

craft_mode = (craft_mode % 3) + 1
craft_output = 1
```

</td>
</tr>
</table>

`EWdsb2JhbF9jcmFmdF9tb2RlAgAAAAZ3YWtldXAFa2V5LjIBAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAhjcmFmdGluZwhjb25zdGFudAQCPT0IY29uc3RhbnQCAAAAAAIAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQECmNyYWZ0X21vZGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECmNyYWZ0X21vZGUIY29uc3RhbnQEA21vZAhjb25zdGFudAIDAAAACGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dAhjb25zdGFudAIBAAAA`

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>global_craft_output</td><td>3</td><td>1 1 2</td><td>

```
:global int craft_mode
:global int craft_output
:local int max_value
:global int crafting

key.3()

(crafting == 0)

max_value = (13100700 / (100 ^ craft_mode)) % 100
craft_output = (craft_output % max_value) + 1
```

</td>
</tr>
</table>

`E2dsb2JhbF9jcmFmdF9vdXRwdXQBAAAABWtleS4zAQAAAA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQIY3JhZnRpbmcIY29uc3RhbnQEAj09CGNvbnN0YW50AgAAAAACAAAADWxvY2FsLmludC5zZXQIY29uc3RhbnQECW1heF92YWx1ZQ5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludAhjb25zdGFudAKc5scACGNvbnN0YW50BAEvDmFyaXRobWV0aWMuaW50CGNvbnN0YW50AmQAAAAIY29uc3RhbnQEA3Bvdw5nbG9iYWwuaW50LmdldAhjb25zdGFudAQKY3JhZnRfbW9kZQhjb25zdGFudAQDbW9kCGNvbnN0YW50AmQAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEDGNyYWZ0X291dHB1dA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0CGNvbnN0YW50BANtb2QNbG9jYWwuaW50LmdldAhjb25zdGFudAQJbWF4X3ZhbHVlCGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAA=`

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>global_craft_preservation</td><td>4</td><td>2 1 1</td><td>

```
:global double craft_preservation
:global int crafting

wakeup()
key.4()

(crafting == 0)

craft_preservation = (craft_preservation + 1.0) % 2.0
```

</td>
</tr>
</table>

`GWdsb2JhbF9jcmFmdF9wcmVzZXJ2YXRpb24CAAAABndha2V1cAVrZXkuNAEAAAAOY29tcGFyaXNvbi5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECGNyYWZ0aW5nCGNvbnN0YW50BAI9PQhjb25zdGFudAIAAAAAAQAAABFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASsIY29uc3RhbnQDAAAAAAAA8D8IY29uc3RhbnQEA21vZAhjb25zdGFudAMAAAAAAAAAQA==`

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>global_craft_countdown</td><td>8</td><td>1 1 6</td><td>

```
:local double tmp
:local int inc
:local double pow
:global double count
:global int crafting

key.8()

(crafting == 0)

   gotoif(a, count < 1.0)
   pow = double.floor(-0.01 + (count // 10.0))
a: inc = 10 ^ d2i(pow)
   tmp = count - i2d(inc)
   gotoif(99, tmp < 1.0)
   count = tmp
```

</td>
</tr>
</table>

`Fmdsb2JhbF9jcmFmdF9jb3VudGRvd24BAAAABWtleS44AQAAAA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQIY3JhZnRpbmcIY29uc3RhbnQEAj09CGNvbnN0YW50AgAAAAAGAAAADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgMAAAARY29tcGFyaXNvbi5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAE8CGNvbnN0YW50AwAAAAAAAPA/EGxvY2FsLmRvdWJsZS5zZXQIY29uc3RhbnQEA3Bvdwxkb3VibGUuZmxvb3IRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDexSuR+F6hL8IY29uc3RhbnQEASsRYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BANsb2cIY29uc3RhbnQDAAAAAAAAJEANbG9jYWwuaW50LnNldAhjb25zdGFudAQDaW5jDmFyaXRobWV0aWMuaW50CGNvbnN0YW50AgoAAAAIY29uc3RhbnQEA3BvdwNkMmkQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQDcG93EGxvY2FsLmRvdWJsZS5zZXQIY29uc3RhbnQEA3RtcBFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEAS0DaTJkDWxvY2FsLmludC5nZXQIY29uc3RhbnQEA2luYw5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAEWNvbXBhcmlzb24uZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEA3RtcAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPxFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQDdG1w`

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>global_craft_countup</td><td>9</td><td>1 1 5</td><td>

```
:local int inc
:local double pow
:global double count
:global int crafting

key.9()

(crafting == 0)

   gotoif(99, count > 9000000.0)
   gotoif(a, count < 1.0)
   pow = double.floor(0.01 + (count // 10.0))
a: inc = 10 ^ d2i(pow)
   count = count + i2d(inc)
```

</td>
</tr>
</table>

`FGdsb2JhbF9jcmFmdF9jb3VudHVwAQAAAAVrZXkuOQEAAAAOY29tcGFyaXNvbi5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECGNyYWZ0aW5nCGNvbnN0YW50BAI9PQhjb25zdGFudAIAAAAABQAAAA5nZW5lcmljLmdvdG9pZghjb25zdGFudAJjAAAAEWNvbXBhcmlzb24uZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPghjb25zdGFudAMAAAAAiCphQQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAIEAAAAEWNvbXBhcmlzb24uZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBPAhjb25zdGFudAMAAAAAAADwPxBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BANwb3cMZG91YmxlLmZsb29yEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50A3sUrkfheoQ/CGNvbnN0YW50BAErEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQDbG9nCGNvbnN0YW50AwAAAAAAACRADWxvY2FsLmludC5zZXQIY29uc3RhbnQEA2luYw5hcml0aG1ldGljLmludAhjb25zdGFudAIKAAAACGNvbnN0YW50BANwb3cDZDJpEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEA3BvdxFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAErA2kyZA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BANpbmM=`

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft</td><td>0</td><td>1 1 3</td><td>

```
:global int craft_mode
:global int craft_output
:global int crafting

key.0()

(isopen("factory")) & (crafting == 0)

crafting = 1
executesync(concat(concat("factory_craft_", i2s(craft_mode)),concat(".", i2s(craft_output))))
crafting = 0
```

</td>
</tr>
</table>

`DWZhY3RvcnlfY3JhZnQBAAAABWtleS4wAQAAAA9jb21wYXJpc29uLmJvb2wSdG93bi53aW5kb3cuaXNvcGVuCGNvbnN0YW50BAdmYWN0b3J5CGNvbnN0YW50BAEmDmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAhjcmFmdGluZwhjb25zdGFudAQCPT0IY29uc3RhbnQCAAAAAAMAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQECGNyYWZ0aW5nCGNvbnN0YW50AgEAAAATZ2VuZXJpYy5leGVjdXRlc3luYwZjb25jYXQGY29uY2F0CGNvbnN0YW50BA5mYWN0b3J5X2NyYWZ0XwNpMnMOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECmNyYWZ0X21vZGUGY29uY2F0CGNvbnN0YW50BAEuA2kycw5nbG9iYWwuaW50LmdldAhjb25zdGFudAQMY3JhZnRfb3V0cHV0Dmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAhjcmFmdGluZwhjb25zdGFudAIAAAAA`


## Part crafting
### Chips

<table>
<tr>
  <th>Script name</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft_1.1</td><td>0 0 9</td><td>

```
:global double count
:global int craft1_state
:global int tier

; Chips

gotoif(99, tier > 5)
executesync("factory_craft_1.1_tier")
craft1_state = 1

execute("factory_craft_1.1_plates")
execute("factory_craft_1.1_cables")
execute("factory_craft_1.1_boards")
execute("factory_craft_1.1_wires")

waitwhile(craft1_state < 31)
craft("chip", tier, count)
```

</td>
</tr>
</table>

`EWZhY3RvcnlfY3JhZnRfMS4xAAAAAAAAAAAJAAAADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AmMAAAAOY29tcGFyaXNvbi5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAT4IY29uc3RhbnQCBQAAABNnZW5lcmljLmV4ZWN1dGVzeW5jCGNvbnN0YW50BBZmYWN0b3J5X2NyYWZ0XzEuMV90aWVyDmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAxjcmFmdDFfc3RhdGUIY29uc3RhbnQCAQAAAA9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQEGGZhY3RvcnlfY3JhZnRfMS4xX3BsYXRlcw9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQEGGZhY3RvcnlfY3JhZnRfMS4xX2NhYmxlcw9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQEGGZhY3RvcnlfY3JhZnRfMS4xX2JvYXJkcw9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQEF2ZhY3RvcnlfY3JhZnRfMS4xX3dpcmVzEWdlbmVyaWMud2FpdHdoaWxlDmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdDFfc3RhdGUIY29uc3RhbnQEATwIY29uc3RhbnQCHwAAAA1mYWN0b3J5LmNyYWZ0CGNvbnN0YW50BARjaGlwDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudA==`

<table>
<tr>
  <td>factory_craft_1.1_tier</td><td>0 0 10</td><td>

```
:local double mycount
:local int mytier
:local double target
:global double count
:global int tier
:global double craft_preservation

gotoif(99, tier == 1)
target = double.min(4.0 * (i2d(tier) - 1.0), 12.0) * count
gotoif(99, craft_preservation * count("chip", tier - 1) >= target)

; save global state
mytier = tier
mycount = count

; craft lower-tier chips
tier = tier - 1
count = target - craft_preservation*count("chip", tier)
executesync("factory_craft_1.1")

;restore global state
tier = mytier
count = mycount
```

</td>
</tr>
</table>

`FmZhY3RvcnlfY3JhZnRfMS4xX3RpZXIAAAAAAAAAAAoAAAAOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCYwAAAA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQCPT0IY29uc3RhbnQCAQAAABBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BAZ0YXJnZXQRYXJpdGhtZXRpYy5kb3VibGUKZG91YmxlLm1pbhFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAAAQQAhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZQNpMmQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQDAAAAAAAA8D8IY29uc3RhbnQDAAAAAAAAKEAIY29uc3RhbnQEASoRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50DmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AmMAAAARY29tcGFyaXNvbi5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BARjaGlwDmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEAj49EGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEBnRhcmdldA1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BAZteXRpZXIOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQHbXljb3VudBFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEBHRpZXIOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAABFnbG9iYWwuZG91YmxlLnNldAhjb25zdGFudAQFY291bnQRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQGdGFyZ2V0CGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQEY2hpcA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllchNnZW5lcmljLmV4ZWN1dGVzeW5jCGNvbnN0YW50BBFmYWN0b3J5X2NyYWZ0XzEuMQ5nbG9iYWwuaW50LnNldAhjb25zdGFudAQEdGllcg1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BAZteXRpZXIRZ2xvYmFsLmRvdWJsZS5zZXQIY29uc3RhbnQEBWNvdW50EGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEB215Y291bnQ=`



<table>
<tr>
  <td>factory_craft_1.1_plates</td><td>0 0 11</td><td>

```
:global int craft1_state
:global double craft_preservation
:local double board_hi
:local double board_lo
:global int tier
:global double count
:global int location

; Ensure we have enough plates 
   board_lo = count * (double.floor(864410.0 / (10.0 ^ i2d(tier))) % 10.0)
   board_hi = count * (double.floor(862210.0 / (10.0 ^ i2d(tier))) % 10.0)

   gotoif(a, board_lo <= craft_preservation * (count("plate.circuit", (tier * 2) - 1) + count("plate", (tier * 2) - 1)))
   waitwhile(active("presser"))
   produce("ingot", (tier * 2) - 1, board_lo - craft_preservation * (count("plate.circuit", (tier * 2) - 1) + count("plate", (tier * 2) - 1)), "presser")
   waitwhile(active("presser"))

a: gotoif(b, board_hi <= craft_preservation * (count("plate.circuit", tier * 2) + count("plate", tier * 2)))
   waitwhile(active("presser"))
   produce("ingot", tier * 2, board_hi - craft_preservation * (count("plate.circuit", tier * 2) + count("plate", tier * 2)), "presser")
   waitwhile(active("presser"))

b: craft1_state = craft1_state + 2
```

</td>
</tr>
</table>

`GGZhY3RvcnlfY3JhZnRfMS4xX3BsYXRlcwAAAAAAAAAACwAAABBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BAhib2FyZF9sbxFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGUMZG91YmxlLmZsb29yEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAA0YSpBCGNvbnN0YW50BAEvEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAACRACGNvbnN0YW50BANwb3cDaTJkDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BANtb2QIY29uc3RhbnQDAAAAAAAAJEAQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQIYm9hcmRfaGkRYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlDGRvdWJsZS5mbG9vchFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAABFAqQQhjb25zdGFudAQBLxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAAAkQAhjb25zdGFudAQDcG93A2kyZA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQDbW9kCGNvbnN0YW50AwAAAAAAACRADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgcAAAARY29tcGFyaXNvbi5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfbG8IY29uc3RhbnQEAjw9EWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFcGxhdGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEB3ByZXNzZXIPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAVpbmdvdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAARYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfbG8IY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BA1wbGF0ZS5jaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQBKxNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEB3ByZXNzZXIRZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQHcHJlc3Nlcg5nZW5lcmljLmdvdG9pZghjb25zdGFudAILAAAAEWNvbXBhcmlzb24uZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2hpCGNvbnN0YW50BAI8PRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDXBsYXRlLmNpcmN1aXQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBKxNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEB3ByZXNzZXIPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAVpbmdvdA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAAEWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2hpCGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAErE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBXBsYXRlDmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEB3ByZXNzZXIRZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQHcHJlc3Nlcg5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnQxX3N0YXRlDmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdDFfc3RhdGUIY29uc3RhbnQEASsIY29uc3RhbnQCAgAAAA==`

<table>
<tr>
  <td>factory_craft_1.1_cables</td><td>0 0 11</td><td>

```
:global int craft1_state
:global int tier
:global double count
:global double craft_preservation
:local double circuit_hi
:local double circuit_lo

   circuit_lo = count * 2.0
   circuit_hi = count * (double.floor(224420.0 / (10.0 ^ i2d(tier))) % 10.0)

   gotoif(a, circuit_lo <= craft_preservation * (count("circuit", (tier * 2) - 1) + count("cable", (tier * 2) - 1)))
   waitwhile(active("refinery"))
   produce("ingot", (tier * 2) - 1, double.ceil(circuit_lo - craft_preservation * (count("circuit", (tier * 2) - 1) + count("cable", (tier * 2) - 1)) / 2.0), "refinery")
   waitwhile(active("refinery"))

a: gotoif(b, circuit_hi <= craft_preservation * (count("circuit", tier * 2) + count("cable", tier * 2)))
   waitwhile(active("refinery"))
   produce("ingot", tier * 2, double.ceil(circuit_hi - craft_preservation * (count("circuit", tier * 2) + count("cable", tier * 2)) / 2.0), "refinery")
   waitwhile(active("refinery"))

b: craft1_state = craft1_state + 4
```

</td>
</tr>
</table>

`GGZhY3RvcnlfY3JhZnRfMS4xX2NhYmxlcwAAAAAAAAAACwAAABBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BApjaXJjdWl0X2xvEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKghjb25zdGFudAMAAAAAAAAAQBBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BApjaXJjdWl0X2hpEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZQxkb3VibGUuZmxvb3IRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAACBlC0EIY29uc3RhbnQEAS8RYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAAJEAIY29uc3RhbnQEA3BvdwNpMmQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEA21vZAhjb25zdGFudAMAAAAAAAAkQA5nZW5lcmljLmdvdG9pZghjb25zdGFudAIHAAAAEWNvbXBhcmlzb24uZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECmNpcmN1aXRfbG8IY29uc3RhbnQEAjw9EWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQHY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFY2FibGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQECHJlZmluZXJ5D2ZhY3RvcnkucHJvZHVjZQhjb25zdGFudAQFaW5nb3QOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAAC2RvdWJsZS5jZWlsEWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECmNpcmN1aXRfbG8IY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQBKxNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVjYWJsZQ5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEAS8IY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQECHJlZmluZXJ5EWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQECHJlZmluZXJ5DmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgsAAAARY29tcGFyaXNvbi5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9oaQhjb25zdGFudAQCPD0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEASsTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFY2FibGUOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAABFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAhyZWZpbmVyeQ9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWluZ290DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAALZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9oaQhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEB2NpcmN1aXQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBKxNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVjYWJsZQ5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEvCGNvbnN0YW50AwAAAAAAAABACGNvbnN0YW50BAhyZWZpbmVyeRFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAhyZWZpbmVyeQ5nbG9iYWwuaW50LnNldAhjb25zdGFudAQMY3JhZnQxX3N0YXRlDmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAxjcmFmdDFfc3RhdGUIY29uc3RhbnQEASsIY29uc3RhbnQCBAAAAA==`

<table>
<tr>
  <td>factory_craft_1.1_boards</td><td>0 0 13</td><td>

```
:global int craft1_state
:local double board_hi
:local double board_lo
:global int tier
:global double count
:global int location
:global double craft_preservation

   board_lo = count * (double.floor(864410.0 / (10.0 ^ i2d(tier))) % 10.0)
   board_hi = count * (double.floor(862210.0 / (10.0 ^ i2d(tier))) % 10.0)
   
   gotoif(a, board_lo <= craft_preservation * (count("plate.circuit", (tier * 2) - 1)))
   waitwhile(count("plate", (tier * 2) - 1) < board_lo - craft_preservation * (count("plate.circuit", (tier * 2) - 1)))
   waitwhile(active("refinery"))
   produce("plate", (tier * 2) - 1, board_lo - craft_preservation * (count("plate.circuit", (tier * 2) - 1)), "refinery")
   waitwhile(active("refinery"))

a: gotoif(b, board_hi <= craft_preservation * (count("plate.circuit", tier * 2)))
   waitwhile(count("plate", tier * 2) < board_lo - craft_preservation * (count("plate.circuit", tier * 2)))
   waitwhile(active("refinery"))
   produce("plate", tier * 2, board_hi - craft_preservation * (count("plate.circuit", tier * 2)), "refinery")
   waitwhile(active("refinery"))

b: craft1_state = craft1_state + 8
```

</td>
</tr>
</table>

`GGZhY3RvcnlfY3JhZnRfMS4xX2JvYXJkcwAAAAAAAAAADQAAABBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BAhib2FyZF9sbxFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoRYXJpdGhtZXRpYy5kb3VibGUMZG91YmxlLmZsb29yEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAA0YSpBCGNvbnN0YW50BAEvEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAACRACGNvbnN0YW50BANwb3cDaTJkDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BANtb2QIY29uc3RhbnQDAAAAAAAAJEAQbG9jYWwuZG91YmxlLnNldAhjb25zdGFudAQIYm9hcmRfaGkRYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlDGRvdWJsZS5mbG9vchFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAABFAqQQhjb25zdGFudAQBLxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAAAkQAhjb25zdGFudAQDcG93A2kyZA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQDbW9kCGNvbnN0YW50AwAAAAAAACRADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AggAAAARY29tcGFyaXNvbi5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfbG8IY29uc3RhbnQEAjw9EWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAARZ2VuZXJpYy53YWl0d2hpbGURY29tcGFyaXNvbi5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFcGxhdGUOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAACGNvbnN0YW50BAE8EWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQECGJvYXJkX2xvCGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQNcGxhdGUuY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAARZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQIcmVmaW5lcnkPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAVwbGF0ZQ5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAARYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfbG8IY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BA1wbGF0ZS5jaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQIcmVmaW5lcnkRZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQIcmVmaW5lcnkOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCDQAAABFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAhib2FyZF9oaQhjb25zdGFudAQCPD0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BA1wbGF0ZS5jaXJjdWl0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAARZ2VuZXJpYy53YWl0d2hpbGURY29tcGFyaXNvbi5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFcGxhdGUOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBPBFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAhib2FyZF9sbwhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDXBsYXRlLmNpcmN1aXQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAABFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAhyZWZpbmVyeQ9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBXBsYXRlDmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAARYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQIYm9hcmRfaGkIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BA1wbGF0ZS5jaXJjdWl0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQECHJlZmluZXJ5EWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQECHJlZmluZXJ5Dmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAxjcmFmdDFfc3RhdGUOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEDGNyYWZ0MV9zdGF0ZQhjb25zdGFudAQBKwhjb25zdGFudAIIAAAA`

<table>
<tr>
  <td>factory_craft_1.1_wires</td><td>0 0 13</td><td>

```
:global int craft1_state
:global int tier
:global double craft_preservation
:global double count
:local double circuit_hi
:local double circuit_lo

   circuit_lo = count * 2.0
   circuit_hi = count * (double.floor(224420.0 / (10.0 ^ i2d(tier))) % 10.0)

   gotoif(a, circuit_lo <= craft_preservation * count("circuit", (tier * 2) - 1))
   waitwhile(circuit_lo > count("cable", tier * 2 - 1) - craft_preservation * count("circuit", tier * 2 - 1))
   waitwhile(active("assembler"))
   produce("cable", (tier * 2) - 1, circuit_lo - craft_preservation * count("circuit", (tier * 2) - 1), "assembler")
   waitwhile(active("assembler"))

a: gotoif(b, circuit_hi <= craft_preservation * count("circuit", tier * 2))
   waitwhile(circuit_hi > count("cable", tier * 2) - craft_preservation * count("circuit", tier * 2))
   waitwhile(active("assembler"))
   produce("cable", tier * 2, circuit_hi - craft_preservation * count("circuit", tier * 2), "assembler")
   waitwhile(active("assembler"))

b: craft1_state = craft1_state + 16
```

</td>
</tr>
</table>

`F2ZhY3RvcnlfY3JhZnRfMS4xX3dpcmVzAAAAAAAAAAANAAAAEGxvY2FsLmRvdWJsZS5zZXQIY29uc3RhbnQECmNpcmN1aXRfbG8RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqCGNvbnN0YW50AwAAAAAAAABAEGxvY2FsLmRvdWJsZS5zZXQIY29uc3RhbnQECmNpcmN1aXRfaGkRYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqEWFyaXRobWV0aWMuZG91YmxlDGRvdWJsZS5mbG9vchFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAIGULQQhjb25zdGFudAQBLxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAAAkQAhjb25zdGFudAQDcG93A2kyZA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQDbW9kCGNvbnN0YW50AwAAAAAAACRADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AggAAAARY29tcGFyaXNvbi5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQKY2lyY3VpdF9sbwhjb25zdGFudAQCPD0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAABFnZW5lcmljLndhaXR3aGlsZRFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2xvCGNvbnN0YW50BAE+EWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAAAhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEB2NpcmN1aXQOYXJpdGhtZXRpYy5pbnQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAAAhjb25zdGFudAQBLQhjb25zdGFudAIBAAAAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQECWFzc2VtYmxlcg9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWNhYmxlDmFyaXRobWV0aWMuaW50DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0IY29uc3RhbnQCAQAAABFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2xvCGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQHY2lyY3VpdA5hcml0aG1ldGljLmludA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAEtCGNvbnN0YW50AgEAAAAIY29uc3RhbnQECWFzc2VtYmxlchFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAlhc3NlbWJsZXIOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCDQAAABFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2hpCGNvbnN0YW50BAI8PRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEB2NpcmN1aXQOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAABFnZW5lcmljLndhaXR3aGlsZRFjb21wYXJpc29uLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2hpCGNvbnN0YW50BAE+EWFyaXRobWV0aWMuZG91YmxlE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAdjaXJjdWl0DmFyaXRobWV0aWMuaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEqCGNvbnN0YW50AgIAAAARZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQJYXNzZW1ibGVyD2ZhY3RvcnkucHJvZHVjZQhjb25zdGFudAQFY2FibGUOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEASoIY29uc3RhbnQCAgAAABFhcml0aG1ldGljLmRvdWJsZRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BApjaXJjdWl0X2hpCGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQHY2lyY3VpdA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBKghjb25zdGFudAICAAAACGNvbnN0YW50BAlhc3NlbWJsZXIRZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQJYXNzZW1ibGVyDmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAxjcmFmdDFfc3RhdGUOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEDGNyYWZ0MV9zdGF0ZQhjb25zdGFudAQBKwhjb25zdGFudAIQAAAA`

### Insulated cables

<table>
<tr>
  <th>Script name</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft_1.2</td><td>0 0 11</td><td>

```
:global double count
:global int tier
:global double craft_preservation
:local double target_cables
:local double target_plates

; Insulated cable

target_cables = count * double.max(double.max(1.0, i2d(tier) - 2.0), double.max(10.0 - (5.0 * ((i2d(tier) - 8.0) ^ 2.0)), double.ceil(((i2d(tier) - 1.0) ^ 1.5) - 11.0)))
target_plates = count * double.max(0.0, ((2.0 * i2d(tier)) - 4.0) - double.max(0.0, 2.0 - ((i2d(tier) - 8.0) * (i2d(tier) - 9.0))))
; can craft other things from this point?

   gotoif(a, craft_preservation * count("cable", tier) >= target_cables)
   waitwhile(active("refinery"))
   produce("ingot", tier, double.ceil((target_cables - craft_preservation * count("cable", tier)) / 2.0), "refinery")

a: gotoif(b, craft_preservation * count("plate.rubber", 1) >= target_plates)
   waitwhile(active("presser"))
   produce("rubber", 1, target_plates - craft_preservation * count("plate.rubber", 1), "presser")

b: waituntil(count("cable", tier) >= target_cables)
   waituntil(count("plate.rubber", 1) >= target_plates)

craft("cable.insulated", tier, count)
```

</td>
</tr>
</table>

`EWZhY3RvcnlfY3JhZnRfMS4yAAAAAAAAAAALAAAAEGxvY2FsLmRvdWJsZS5zZXQIY29uc3RhbnQEDXRhcmdldF9jYWJsZXMRYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqCmRvdWJsZS5tYXgKZG91YmxlLm1heAhjb25zdGFudAMAAAAAAADwPxFhcml0aG1ldGljLmRvdWJsZQNpMmQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQDAAAAAAAAAEAKZG91YmxlLm1heBFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAAAkQAhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAAAUQAhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZQNpMmQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQDAAAAAAAAIEAIY29uc3RhbnQEA3Bvdwhjb25zdGFudAMAAAAAAAAAQAtkb3VibGUuY2VpbBFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZQNpMmQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQDAAAAAAAA8D8IY29uc3RhbnQEA3Bvdwhjb25zdGFudAMAAAAAAAD4Pwhjb25zdGFudAQBLQhjb25zdGFudAMAAAAAAAAmQBBsb2NhbC5kb3VibGUuc2V0CGNvbnN0YW50BA10YXJnZXRfcGxhdGVzEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKgpkb3VibGUubWF4CGNvbnN0YW50AwAAAAAAAAAAEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAAABACGNvbnN0YW50BAEqA2kyZA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBLQhjb25zdGFudAMAAAAAAAAQQAhjb25zdGFudAQBLQpkb3VibGUubWF4CGNvbnN0YW50AwAAAAAAAAAAEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAAABACGNvbnN0YW50BAEtEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlA2kyZA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQBLQhjb25zdGFudAMAAAAAAAAgQAhjb25zdGFudAQBKhFhcml0aG1ldGljLmRvdWJsZQNpMmQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS0IY29uc3RhbnQDAAAAAAAAIkAOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCBgAAABFjb21wYXJpc29uLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAI+PRBsb2NhbC5kb3VibGUuZ2V0CGNvbnN0YW50BA10YXJnZXRfY2FibGVzEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQECHJlZmluZXJ5D2ZhY3RvcnkucHJvZHVjZQhjb25zdGFudAQFaW5nb3QOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXILZG91YmxlLmNlaWwRYXJpdGhtZXRpYy5kb3VibGURYXJpdGhtZXRpYy5kb3VibGUQbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQNdGFyZ2V0X2NhYmxlcwhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBWNhYmxlDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAEvCGNvbnN0YW50AwAAAAAAAABACGNvbnN0YW50BAhyZWZpbmVyeQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAIJAAAAEWNvbXBhcmlzb24uZG91YmxlEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQMcGxhdGUucnViYmVyCGNvbnN0YW50AgEAAAAIY29uc3RhbnQEAj49EGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEDXRhcmdldF9wbGF0ZXMRZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQHcHJlc3Nlcg9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBnJ1YmJlcghjb25zdGFudAIBAAAAEWFyaXRobWV0aWMuZG91YmxlEGxvY2FsLmRvdWJsZS5nZXQIY29uc3RhbnQEDXRhcmdldF9wbGF0ZXMIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAxwbGF0ZS5ydWJiZXIIY29uc3RhbnQCAQAAAAhjb25zdGFudAQHcHJlc3NlchFnZW5lcmljLndhaXR1bnRpbBFjb21wYXJpc29uLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVjYWJsZQ5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQCPj0QbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQNdGFyZ2V0X2NhYmxlcxFnZW5lcmljLndhaXR1bnRpbBFjb21wYXJpc29uLmRvdWJsZRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAxwbGF0ZS5ydWJiZXIIY29uc3RhbnQCAQAAAAhjb25zdGFudAQCPj0QbG9jYWwuZG91YmxlLmdldAhjb25zdGFudAQNdGFyZ2V0X3BsYXRlcw1mYWN0b3J5LmNyYWZ0CGNvbnN0YW50BA9jYWJsZS5pbnN1bGF0ZWQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50`

### Pumps
<table>
<tr>
  <th>Script name</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft_1.3</td><td>0 0 13</td><td>

```
:global double count
:global int tier
:global double craft_preservation
:global int craft_state

; Pumps

; each pump needs one motor.  Same count, same tier
   executesync("factory_craft_1.4")
   craft_state = 1
   execute("factory_craft_1.3_rings")
   
; target_plates = count * 2.0
; target_rubber = count * 4.0

   gotoif(a, count * 2.0 <= craft_preservation * count("plate", tier))
   waitwhile(active("presser"))
   produce("ingot", tier, count * 2.0 - craft_preservation * count("plate", tier), "presser")
   
a: gotoif(b, count * 4.0 <= craft_preservation * count("plate.rubber", 1))
   waitwhile(active("presser"))
   produce("rubber", 1, count * 4.0 - craft_preservation * count("plate.rubber", 1), "presser")

b: waituntil(craft_state >= 3)
   waituntil(count * 2.0 <= count("plate", tier))
   waituntil(count * 4.0 <= count("plate.rubber", 1))

craft("pump", tier, count)
```

</td>
</tr>
</table>

`EWZhY3RvcnlfY3JhZnRfMS4zAAAAAAAAAAANAAAAE2dlbmVyaWMuZXhlY3V0ZXN5bmMIY29uc3RhbnQEEWZhY3RvcnlfY3JhZnRfMS40Dmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAtjcmFmdF9zdGF0ZQhjb25zdGFudAIBAAAAD2dlbmVyaWMuZXhlY3V0ZQhjb25zdGFudAQXZmFjdG9yeV9jcmFmdF8xLjNfcmluZ3MOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCBwAAABFjb21wYXJpc29uLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoIY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQEAjw9EWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFcGxhdGUOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIRZ2VuZXJpYy53YWl0d2hpbGUWZmFjdG9yeS5tYWNoaW5lLmFjdGl2ZQhjb25zdGFudAQHcHJlc3Nlcg9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEBWluZ290Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyEWFyaXRobWV0aWMuZG91YmxlEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKghjb25zdGFudAMAAAAAAAAAQAhjb25zdGFudAQBLRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEBXBsYXRlDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAdwcmVzc2VyDmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgoAAAARY29tcGFyaXNvbi5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqCGNvbnN0YW50AwAAAAAAABBACGNvbnN0YW50BAI8PRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQSY3JhZnRfcHJlc2VydmF0aW9uCGNvbnN0YW50BAEqE2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDHBsYXRlLnJ1YmJlcghjb25zdGFudAIBAAAAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEB3ByZXNzZXIPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAZydWJiZXIIY29uc3RhbnQCAQAAABFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoIY29uc3RhbnQDAAAAAAAAEEAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAxwbGF0ZS5ydWJiZXIIY29uc3RhbnQCAQAAAAhjb25zdGFudAQHcHJlc3NlchFnZW5lcmljLndhaXR1bnRpbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLY3JhZnRfc3RhdGUIY29uc3RhbnQEAj49CGNvbnN0YW50AgMAAAARZ2VuZXJpYy53YWl0dW50aWwRY29tcGFyaXNvbi5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqCGNvbnN0YW50AwAAAAAAAABACGNvbnN0YW50BAI8PRNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllchFnZW5lcmljLndhaXR1bnRpbBFjb21wYXJpc29uLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoIY29uc3RhbnQDAAAAAAAAEEAIY29uc3RhbnQEAjw9E2ZhY3RvcnkuaXRlbXMuY291bnQIY29uc3RhbnQEDHBsYXRlLnJ1YmJlcghjb25zdGFudAIBAAAADWZhY3RvcnkuY3JhZnQIY29uc3RhbnQEBHB1bXAOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIRZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50`

<table>
<tr>
  <td>factory_craft_1.3_rings</td><td>0 0 8</td><td>

```
:global int craft_state
:global int tier
:global double craft_preservation
:global double count

; target = count * 2.0

   gotoif(a, craft_preservation * count("rod", tier) >= count * 2.0)
   waitwhile(active("shaper"))
   produce("ingot", tier, double.ceil((count * 2.0 - craft_preservation * count("rod", tier)) / 2.0), "shaper")

a: gotoif(b, craft_preservation * count("ring", tier) >= count * 2.0)
   waitwhile(active("shaper"))
   produce("rod", tier, count * 2.0 - craft_preservation * count("ring", tier), "shaper")

b: waituntil(count("ring", tier) >= count * 2.0)
   craft_state = craft_state + 2
```

</td>
</tr>
</table>

`F2ZhY3RvcnlfY3JhZnRfMS4zX3JpbmdzAAAAAAAAAAAIAAAADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgQAAAARY29tcGFyaXNvbi5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BANyb2QOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAj49EWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKghjb25zdGFudAMAAAAAAAAAQBFnZW5lcmljLndhaXR3aGlsZRZmYWN0b3J5Lm1hY2hpbmUuYWN0aXZlCGNvbnN0YW50BAZzaGFwZXIPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAVpbmdvdA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcgtkb3VibGUuY2VpbBFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoIY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BANyb2QOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAS8IY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQEBnNoYXBlcg5nZW5lcmljLmdvdG9pZghjb25zdGFudAIHAAAAEWNvbXBhcmlzb24uZG91YmxlEWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BBJjcmFmdF9wcmVzZXJ2YXRpb24IY29uc3RhbnQEASoTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQEcmluZw5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQCPj0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqCGNvbnN0YW50AwAAAAAAAABAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEBnNoYXBlcg9mYWN0b3J5LnByb2R1Y2UIY29uc3RhbnQEA3JvZA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllchFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoIY29uc3RhbnQDAAAAAAAAAEAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BARyaW5nDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAR0aWVyCGNvbnN0YW50BAZzaGFwZXIRZ2VuZXJpYy53YWl0dW50aWwRY29tcGFyaXNvbi5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQEcmluZw5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQCPj0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqCGNvbnN0YW50AwAAAAAAAABADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAtjcmFmdF9zdGF0ZQ5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLY3JhZnRfc3RhdGUIY29uc3RhbnQEASsIY29uc3RhbnQCAgAAAA==`

### Motors

<table>
<tr>
  <th>Script name</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft_1.4</td><td>0 0 9</td><td>

```
:global double count
:global int tier
:global double craft_preservation
:global int craft_state

   craft_state = 1
   execute("factory_craft_1.4_coils")
   execute("factory_craft_1.4_rods")

; target_plates = count * 4.0
   gotoif(a, craft_preservation * count("plate", tier) >= count * 4.0)
   waitwhile(active("presser"))
   produce("ingot", tier, count * 4.0 - craft_preservation * count("plate", tier), "presser")

a: waituntil(craft_state >= 7)
   waituntil(count("plate", tier) >= count * 4.0)

craft("motor", tier, count)
```

</td>
</tr>
</table>

`EWZhY3RvcnlfY3JhZnRfMS40AAAAAAAAAAAJAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAtjcmFmdF9zdGF0ZQhjb25zdGFudAIBAAAAD2dlbmVyaWMuZXhlY3V0ZQhjb25zdGFudAQXZmFjdG9yeV9jcmFmdF8xLjRfY29pbHMPZ2VuZXJpYy5leGVjdXRlCGNvbnN0YW50BBZmYWN0b3J5X2NyYWZ0XzEuNF9yb2RzDmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgcAAAARY29tcGFyaXNvbi5kb3VibGURYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQCPj0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEBWNvdW50CGNvbnN0YW50BAEqCGNvbnN0YW50AwAAAAAAABBAEWdlbmVyaWMud2FpdHdoaWxlFmZhY3RvcnkubWFjaGluZS5hY3RpdmUIY29uc3RhbnQEB3ByZXNzZXIPZmFjdG9yeS5wcm9kdWNlCGNvbnN0YW50BAVpbmdvdA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllchFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZRFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQIY29uc3RhbnQEASoIY29uc3RhbnQDAAAAAAAAEEAIY29uc3RhbnQEAS0RYXJpdGhtZXRpYy5kb3VibGURZ2xvYmFsLmRvdWJsZS5nZXQIY29uc3RhbnQEEmNyYWZ0X3ByZXNlcnZhdGlvbghjb25zdGFudAQBKhNmYWN0b3J5Lml0ZW1zLmNvdW50CGNvbnN0YW50BAVwbGF0ZQ5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllcghjb25zdGFudAQHcHJlc3NlchFnZW5lcmljLndhaXR1bnRpbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLY3JhZnRfc3RhdGUIY29uc3RhbnQEAj49CGNvbnN0YW50AgcAAAARZ2VuZXJpYy53YWl0dW50aWwRY29tcGFyaXNvbi5kb3VibGUTZmFjdG9yeS5pdGVtcy5jb3VudAhjb25zdGFudAQFcGxhdGUOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEBHRpZXIIY29uc3RhbnQEAj49EWFyaXRobWV0aWMuZG91YmxlEWdsb2JhbC5kb3VibGUuZ2V0CGNvbnN0YW50BAVjb3VudAhjb25zdGFudAQBKghjb25zdGFudAMAAAAAAAAQQA1mYWN0b3J5LmNyYWZ0CGNvbnN0YW50BAVtb3Rvcg5nbG9iYWwuaW50LmdldAhjb25zdGFudAQEdGllchFnbG9iYWwuZG91YmxlLmdldAhjb25zdGFudAQFY291bnQ=`

<table>
<tr>
  <td>factory_craft</td><td>0 0 9</td><td>

```
```

</td>
</tr>
</table>

` `

<table>
<tr>
  <td>factory_craft</td><td>0 0 9</td><td>

```
```

</td>
</tr>
</table>

` `

### Cubes
<table>
<tr>
  <th>Script name</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft</td><td>0 0 9</td><td>

```
```

</td>
</tr>
</table>

` `


### Dense plates
<table>
<tr>
  <th>Script name</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft</td><td>0 0 9</td><td>

```
```

</td>
</tr>
</table>

` `


### Dust (tier up)
<table>
<tr>
  <th>Script name</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft</td><td>0 0 9</td><td>

```
```

</td>
</tr>
</table>

` `



## TODO

<table>
<tr>
  <th>Script name</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>factory_craft</td><td>0 0 9</td><td>

```
```

</td>
</tr>
</table>

` `

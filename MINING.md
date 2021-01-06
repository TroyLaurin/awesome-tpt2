# Mining scripts

These scripts are slightly modified from discord user Xenos#2117, that dig all layers in the mine in about 13 seconds.

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>Mine ALL</td><td>0</td><td>1 1 16</td><td>

```
:local int id

key.0()

isopen("mine")

	click(vec(0.44 * i2d(width()), 0.75 * i2d(height())))
	click(vec(0.44 * i2d(width()), 0.75 * i2d(height())))
	global.int.set("arg cell", -2)
	execute("launch superdrill")
	execute("launch superdrill")
	execute("launch superdrill")
	executesync("launch superdrill")
	execute("newlayer")
nexttab: click(vec((0.52 + (0.09 * i2d(id % 5))) * i2d(width()), 0.75 * i2d(height())))
	id = id + 1
	gotoif(skipclick, (id % 5) != 0)
	click(vec(0.95 * i2d(width()), 0.75 * i2d(height())))
skipclick: wait(0.6)
	global.int.set("time wasted", global.int.get("time wasted") + 1)
	gotoif(nexttab, id < 12)
	executesync("stop superdrill")
```

</td>
</tr>
</table>

`CE1pbmUgQUxMAQAAAAVrZXkuMAEAAAASdG93bi53aW5kb3cuaXNvcGVuCGNvbnN0YW50BARtaW5lEAAAAA1nZW5lcmljLmNsaWNrDnZlYy5mcm9tQ29vcmRzEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50Aylcj8L1KNw/CGNvbnN0YW50BAEqA2kyZAxzY3JlZW4ud2lkdGgRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAA6D8IY29uc3RhbnQEASoDaTJkDXNjcmVlbi5oZWlnaHQNZ2VuZXJpYy5jbGljaw52ZWMuZnJvbUNvb3JkcxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMpXI/C9SjcPwhjb25zdGFudAQBKgNpMmQMc2NyZWVuLndpZHRoEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAAOg/CGNvbnN0YW50BAEqA2kyZA1zY3JlZW4uaGVpZ2h0Dmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAhhcmcgY2VsbAhjb25zdGFudAL+////D2dlbmVyaWMuZXhlY3V0ZQhjb25zdGFudAQRbGF1bmNoIHN1cGVyZHJpbGwPZ2VuZXJpYy5leGVjdXRlCGNvbnN0YW50BBFsYXVuY2ggc3VwZXJkcmlsbA9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQEEWxhdW5jaCBzdXBlcmRyaWxsE2dlbmVyaWMuZXhlY3V0ZXN5bmMIY29uc3RhbnQEEWxhdW5jaCBzdXBlcmRyaWxsD2dlbmVyaWMuZXhlY3V0ZQhjb25zdGFudAQIbmV3bGF5ZXINZ2VuZXJpYy5jbGljaw52ZWMuZnJvbUNvb3JkcxFhcml0aG1ldGljLmRvdWJsZRFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAOkcD0K16PgPwhjb25zdGFudAQBKxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMK16NwPQq3Pwhjb25zdGFudAQBKgNpMmQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQCaWQIY29uc3RhbnQEA21vZAhjb25zdGFudAIFAAAACGNvbnN0YW50BAEqA2kyZAxzY3JlZW4ud2lkdGgRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAA6D8IY29uc3RhbnQEASoDaTJkDXNjcmVlbi5oZWlnaHQNbG9jYWwuaW50LnNldAhjb25zdGFudAQCaWQOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQCaWQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nZW5lcmljLmdvdG9pZghjb25zdGFudAINAAAADmNvbXBhcmlzb24uaW50DmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEAmlkCGNvbnN0YW50BANtb2QIY29uc3RhbnQCBQAAAAhjb25zdGFudAQCIT0IY29uc3RhbnQCAAAAAA1nZW5lcmljLmNsaWNrDnZlYy5mcm9tQ29vcmRzEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50A2ZmZmZmZu4/CGNvbnN0YW50BAEqA2kyZAxzY3JlZW4ud2lkdGgRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAA6D8IY29uc3RhbnQEASoDaTJkDXNjcmVlbi5oZWlnaHQMZ2VuZXJpYy53YWl0CGNvbnN0YW50AzMzMzMzM+M/Dmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAt0aW1lIHdhc3RlZA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLdGltZSB3YXN0ZWQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nZW5lcmljLmdvdG9pZghjb25zdGFudAIJAAAADmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEAmlkCGNvbnN0YW50BAE8CGNvbnN0YW50AgwAAAATZ2VuZXJpYy5leGVjdXRlc3luYwhjb25zdGFudAQPc3RvcCBzdXBlcmRyaWxs`

<table>
<tr>
  <td>launch superdrill</td><td>0 0 4</td><td>

```
loop: global.int.set("time wasted", global.int.get("time wasted") + 1)
    global.int.set("arg cell", global.int.get("arg cell") + 1)
    execute("minecell")
    gotoif(loop, global.int.get("arg cell") < 30)
```

</td>
</tr>
</table>

`EWxhdW5jaCBzdXBlcmRyaWxsAAAAAAAAAAAEAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAt0aW1lIHdhc3RlZA5hcml0aG1ldGljLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLdGltZSB3YXN0ZWQIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQIYXJnIGNlbGwOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECGFyZyBjZWxsCGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAAPZ2VuZXJpYy5leGVjdXRlCGNvbnN0YW50BAhtaW5lY2VsbA5nZW5lcmljLmdvdG9pZghjb25zdGFudAIBAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAhhcmcgY2VsbAhjb25zdGFudAQBPAhjb25zdGFudAIeAAAA`

<table>
<tr>
  <td>stop superdrill</td><td>1 1 3</td><td>

```
key.9()

isopen("mine")

stop("launch superdrill")
stop("minecell")
stop("newlayer")
```

</td>
</tr>
</table>

`D3N0b3Agc3VwZXJkcmlsbAEAAAAFa2V5LjkBAAAAEnRvd24ud2luZG93Lmlzb3Blbghjb25zdGFudAQEbWluZQMAAAAMZ2VuZXJpYy5zdG9wCGNvbnN0YW50BBFsYXVuY2ggc3VwZXJkcmlsbAxnZW5lcmljLnN0b3AIY29uc3RhbnQECG1pbmVjZWxsDGdlbmVyaWMuc3RvcAhjb25zdGFudAQIbmV3bGF5ZXI=`

<table>
<tr>
  <td>minecell</td><td>0 0 3</td><td>

```
:local int cell

    cell = global.int.get("arg cell") / 2
dig: dig(cell % 4, cell / 4)
    gotoif(dig, cell >= 0)
```

</td>
</tr>
</table>

`
0 0 3
CG1pbmVjZWxsAAAAAAAAAAADAAAADWxvY2FsLmludC5zZXQIY29uc3RhbnQEBGNlbGwOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECGFyZyBjZWxsCGNvbnN0YW50BAEvCGNvbnN0YW50AgIAAAAIbWluZS5kaWcOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAQDbW9kCGNvbnN0YW50AgQAAAAOYXJpdGhtZXRpYy5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAQBLwhjb25zdGFudAIEAAAADmdlbmVyaWMuZ290b2lmCGNvbnN0YW50AgIAAAAOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAQCPj0IY29uc3RhbnQCAAAAAA==`

<table>
<tr>
  <td>newlayer</td><td>0 0 12</td><td>

```
    wait(0.0)
loop: newlayer()
    newlayer()
    newlayer()
    newlayer()
    newlayer()
    newlayer()
    newlayer()
    newlayer()
    newlayer()
    newlayer()
    goto(loop)
```

</td>
</tr>
</table>

`CG5ld2xheWVyAAAAAAAAAAAMAAAADGdlbmVyaWMud2FpdAhjb25zdGFudAMAAAAAAAAAAA1taW5lLm5ld2xheWVyDW1pbmUubmV3bGF5ZXINbWluZS5uZXdsYXllcg1taW5lLm5ld2xheWVyDW1pbmUubmV3bGF5ZXINbWluZS5uZXdsYXllcg1taW5lLm5ld2xheWVyDW1pbmUubmV3bGF5ZXINbWluZS5uZXdsYXllcg1taW5lLm5ld2xheWVyDGdlbmVyaWMuZ290bwhjb25zdGFudAICAAAA`

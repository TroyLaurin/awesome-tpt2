# Mining scripts

These scripts are directly inspired by those from discord user Xenos#2117.  Running these scripts requires nearly 90 simultaneous scripts running so you can't be doing much else when you invoke the launcher.  It also requires a nearly fully upgraded set of servers, the largest script here is 19 actions.  From invocation it will clear the mine in a few seconds.

<table>
<tr>
  <th>Script name</th>
  <th>Impulse</th>
  <th>Lines</th>
  <th>Code</th>
</tr>
<tr>
  <td>paralaunch</td><td>0</td><td>1 1 9</td><td>

```
:global int paracell
:global int unparaleash
:local int sets

key.0()

isopen("mine")

unparaleash = 0

execute("paratab")
loop: execute("paralayer")
      execute("paracell1")
      paracell = 0
      sets = sets + 1
      waituntil(paracell == 3)
gotoif(loop, sets < 5)

unparaleash = 1
```

</td>
</tr>
</table>

`CnBhcmFsYXVuY2gBAAAABWtleS4wAQAAABJ0b3duLndpbmRvdy5pc29wZW4IY29uc3RhbnQEBG1pbmUJAAAADmdsb2JhbC5pbnQuc2V0CGNvbnN0YW50BAt1bnBhcmFsZWFzaAhjb25zdGFudAIAAAAAD2dlbmVyaWMuZXhlY3V0ZQhjb25zdGFudAQHcGFyYXRhYg9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQECXBhcmFsYXllcg9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQECXBhcmFjZWxsMQ5nbG9iYWwuaW50LnNldAhjb25zdGFudAQIcGFyYWNlbGwIY29uc3RhbnQCAAAAAA1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BARzZXRzDmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBHNldHMIY29uc3RhbnQEASsIY29uc3RhbnQCAQAAABFnZW5lcmljLndhaXR1bnRpbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQIcGFyYWNlbGwIY29uc3RhbnQEAj09CGNvbnN0YW50AgMAAAAOZ2VuZXJpYy5nb3RvaWYIY29uc3RhbnQCAwAAAA5jb21wYXJpc29uLmludA1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARzZXRzCGNvbnN0YW50BAE8CGNvbnN0YW50AgUAAAAOZ2xvYmFsLmludC5zZXQIY29uc3RhbnQEC3VucGFyYWxlYXNoCGNvbnN0YW50AgEAAAA=`

<table>
<tr>
  <td>paratab</td><td>0 0 19</td><td>

```
:global int unparaleash
:local int id

click(vec(0.44 * i2d(width()), 0.75 * i2d(height())))
click(vec(0.44 * i2d(width()), 0.75 * i2d(height())))
waituntil(unparaleash == 1)

loop: click(vec((0.52 + 0.09 * 0) * i2d(width()), 0.75 * i2d(height())))
id = id + 1
      click(vec((0.52 + 0.09 * 1) * i2d(width()), 0.75 * i2d(height())))
gotoif(end, id > 2)
      click(vec((0.52 + 0.09 * 2) * i2d(width()), 0.75 * i2d(height())))
id = id ; no-op
      click(vec((0.52 + 0.09 * 3) * i2d(width()), 0.75 * i2d(height())))
id = id ; no-op
      click(vec((0.52 + 0.09 * 4) * i2d(width()), 0.75 * i2d(height())))
  click(vec(0.95 * i2d(width()), 0.75 * i2d(height())))
goto(loop)
end: stop("paralayer")
     stop("paracell1")
     stop("paracell2")
     stop("paracell3")
     stop("paracell4")

```

</td>
</tr>
</table>

`B3BhcmF0YWIAAAAAAAAAABMAAAANZ2VuZXJpYy5jbGljaw52ZWMuZnJvbUNvb3JkcxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMpXI/C9SjcPwhjb25zdGFudAQBKgNpMmQMc2NyZWVuLndpZHRoEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAAOg/CGNvbnN0YW50BAEqA2kyZA1zY3JlZW4uaGVpZ2h0DWdlbmVyaWMuY2xpY2sOdmVjLmZyb21Db29yZHMRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDKVyPwvUo3D8IY29uc3RhbnQEASoDaTJkDHNjcmVlbi53aWR0aBFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAADoPwhjb25zdGFudAQBKgNpMmQNc2NyZWVuLmhlaWdodBFnZW5lcmljLndhaXR1bnRpbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLdW5wYXJhbGVhc2gIY29uc3RhbnQEAj09CGNvbnN0YW50AgEAAAANZ2VuZXJpYy5jbGljaw52ZWMuZnJvbUNvb3JkcxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAOkcD0K16PgPwhjb25zdGFudAQBKgNpMmQMc2NyZWVuLndpZHRoEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAAOg/CGNvbnN0YW50BAEqA2kyZA1zY3JlZW4uaGVpZ2h0DWxvY2FsLmludC5zZXQIY29uc3RhbnQEAmlkDmFyaXRobWV0aWMuaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEAmlkCGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAANZ2VuZXJpYy5jbGljaw52ZWMuZnJvbUNvb3JkcxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAOF61G4HoXjPwhjb25zdGFudAQBKgNpMmQMc2NyZWVuLndpZHRoEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAAOg/CGNvbnN0YW50BAEqA2kyZA1zY3JlZW4uaGVpZ2h0DmdlbmVyaWMuZ290b2lmCGNvbnN0YW50Ag8AAAAOY29tcGFyaXNvbi5pbnQNbG9jYWwuaW50LmdldAhjb25zdGFudAQCaWQIY29uc3RhbnQEAT4IY29uc3RhbnQCAgAAAA1nZW5lcmljLmNsaWNrDnZlYy5mcm9tQ29vcmRzEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50A2ZmZmZmZuY/CGNvbnN0YW50BAEqA2kyZAxzY3JlZW4ud2lkdGgRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAA6D8IY29uc3RhbnQEASoDaTJkDXNjcmVlbi5oZWlnaHQNbG9jYWwuaW50LnNldAhjb25zdGFudAQCaWQNbG9jYWwuaW50LmdldAhjb25zdGFudAQCaWQNZ2VuZXJpYy5jbGljaw52ZWMuZnJvbUNvb3JkcxFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudANI4XoUrkfpPwhjb25zdGFudAQBKgNpMmQMc2NyZWVuLndpZHRoEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50AwAAAAAAAOg/CGNvbnN0YW50BAEqA2kyZA1zY3JlZW4uaGVpZ2h0DWxvY2FsLmludC5zZXQIY29uc3RhbnQEAmlkDWxvY2FsLmludC5nZXQIY29uc3RhbnQEAmlkDWdlbmVyaWMuY2xpY2sOdmVjLmZyb21Db29yZHMRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDKVyPwvUo7D8IY29uc3RhbnQEASoDaTJkDHNjcmVlbi53aWR0aBFhcml0aG1ldGljLmRvdWJsZQhjb25zdGFudAMAAAAAAADoPwhjb25zdGFudAQBKgNpMmQNc2NyZWVuLmhlaWdodA1nZW5lcmljLmNsaWNrDnZlYy5mcm9tQ29vcmRzEWFyaXRobWV0aWMuZG91YmxlCGNvbnN0YW50A2ZmZmZmZu4/CGNvbnN0YW50BAEqA2kyZAxzY3JlZW4ud2lkdGgRYXJpdGhtZXRpYy5kb3VibGUIY29uc3RhbnQDAAAAAAAA6D8IY29uc3RhbnQEASoDaTJkDXNjcmVlbi5oZWlnaHQMZ2VuZXJpYy5nb3RvCGNvbnN0YW50AgQAAAAMZ2VuZXJpYy5zdG9wCGNvbnN0YW50BAlwYXJhbGF5ZXIMZ2VuZXJpYy5zdG9wCGNvbnN0YW50BAlwYXJhY2VsbDEMZ2VuZXJpYy5zdG9wCGNvbnN0YW50BAlwYXJhY2VsbDIMZ2VuZXJpYy5zdG9wCGNvbnN0YW50BAlwYXJhY2VsbDMMZ2VuZXJpYy5zdG9wCGNvbnN0YW50BAlwYXJhY2VsbDQ=`

<table>
<tr>
  <td>paralayer</td><td>0 0 12</td><td>

```
:global int unparaleash

waituntil(unparaleash == 1)

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

`CXBhcmFsYXllcgAAAAAAAAAADAAAABFnZW5lcmljLndhaXR1bnRpbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLdW5wYXJhbGVhc2gIY29uc3RhbnQEAj09CGNvbnN0YW50AgEAAAANbWluZS5uZXdsYXllcg1taW5lLm5ld2xheWVyDW1pbmUubmV3bGF5ZXINbWluZS5uZXdsYXllcg1taW5lLm5ld2xheWVyDW1pbmUubmV3bGF5ZXINbWluZS5uZXdsYXllcg1taW5lLm5ld2xheWVyDW1pbmUubmV3bGF5ZXINbWluZS5uZXdsYXllcgxnZW5lcmljLmdvdG8IY29uc3RhbnQCAgAAAA==`

<table>
<tr>
  <td>paracell1</td><td>0 0 17</td><td>

```
:global int paracell
:global int unparaleash
:local int cell

execute("paracell2")
cell = paracell
gotoif(wait, cell > 1)
child: execute("paracell1")
gotoif(child, paracell < 2)
wait: waituntil(unparaleash == 1)

loop: dig(cell, 0)
    dig(cell, 0)
    dig(cell, 0)
    dig(cell, 0)
    dig(cell, 0)
    dig(cell, 0)
    dig(cell, 0)
    dig(cell, 0)
    dig(cell, 0)
    dig(cell, 0)
goto(loop)
```

</td>
</tr>
</table>

`CXBhcmFjZWxsMQAAAAAAAAAAEQAAAA9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQECXBhcmFjZWxsMg1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BARjZWxsDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAhwYXJhY2VsbA5nZW5lcmljLmdvdG9pZghjb25zdGFudAIGAAAADmNvbXBhcmlzb24uaW50DWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQEAT4IY29uc3RhbnQCAQAAAA9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQECXBhcmFjZWxsMQ5nZW5lcmljLmdvdG9pZghjb25zdGFudAIEAAAADmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAhwYXJhY2VsbAhjb25zdGFudAQBPAhjb25zdGFudAICAAAAEWdlbmVyaWMud2FpdHVudGlsDmNvbXBhcmlzb24uaW50Dmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAt1bnBhcmFsZWFzaAhjb25zdGFudAQCPT0IY29uc3RhbnQCAQAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgAAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIAAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAAAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgAAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIAAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAAAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgAAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIAAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAAAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgAAAAAMZ2VuZXJpYy5nb3RvCGNvbnN0YW50AgcAAAA=`

<table>
<tr>
  <td>paracell2</td><td>0 0 14</td><td>

```
:global int paracell
:global int unparaleash
:local int cell

execute("paracell3")
cell = paracell
wait: waituntil(unparaleash == 1)

loop: dig(cell, 1)
    dig(cell, 1)
    dig(cell, 1)
    dig(cell, 1)
    dig(cell, 1)
    dig(cell, 1)
    dig(cell, 1)
    dig(cell, 1)
    dig(cell, 1)
    dig(cell, 1)
goto(loop)
```

</td>
</tr>
</table>

`CXBhcmFjZWxsMgAAAAAAAAAADgAAAA9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQECXBhcmFjZWxsMw1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BARjZWxsDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAhwYXJhY2VsbBFnZW5lcmljLndhaXR1bnRpbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLdW5wYXJhbGVhc2gIY29uc3RhbnQEAj09CGNvbnN0YW50AgEAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIBAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAQAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgEAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIBAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAQAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgEAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIBAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAQAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgEAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIBAAAADGdlbmVyaWMuZ290bwhjb25zdGFudAIEAAAA`

<table>
<tr>
  <td>paracell3</td><td>0 0 14</td><td>

```
:global int paracell
:global int unparaleash
:local int cell

execute("paracell4")
cell = paracell
wait: waituntil(unparaleash == 1)

loop: dig(cell, 2)
    dig(cell, 2)
    dig(cell, 2)
    dig(cell, 2)
    dig(cell, 2)
    dig(cell, 2)
    dig(cell, 2)
    dig(cell, 2)
    dig(cell, 2)
    dig(cell, 2)
goto(loop)
```

</td>
</tr>
</table>

`CXBhcmFjZWxsMwAAAAAAAAAADgAAAA9nZW5lcmljLmV4ZWN1dGUIY29uc3RhbnQECXBhcmFjZWxsNA1sb2NhbC5pbnQuc2V0CGNvbnN0YW50BARjZWxsDmdsb2JhbC5pbnQuZ2V0CGNvbnN0YW50BAhwYXJhY2VsbBFnZW5lcmljLndhaXR1bnRpbA5jb21wYXJpc29uLmludA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQLdW5wYXJhbGVhc2gIY29uc3RhbnQEAj09CGNvbnN0YW50AgEAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAICAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAgAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgIAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAICAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAgAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgIAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAICAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAgAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgIAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAICAAAADGdlbmVyaWMuZ290bwhjb25zdGFudAIEAAAA`

<table>
<tr>
  <td>paracell4</td><td>0 0 14</td><td>

```
:global int paracell
:global int unparaleash
:local int cell

paracell = paracell + 1
cell = paracell
wait: waituntil(unparaleash == 1)

loop: dig(cell, 3)
    dig(cell, 3)
    dig(cell, 3)
    dig(cell, 3)
    dig(cell, 3)
    dig(cell, 3)
    dig(cell, 3)
    dig(cell, 3)
    dig(cell, 3)
    dig(cell, 3)
goto(loop)
```

</td>
</tr>
</table>

`CXBhcmFjZWxsNAAAAAAAAAAADgAAAA5nbG9iYWwuaW50LnNldAhjb25zdGFudAQIcGFyYWNlbGwOYXJpdGhtZXRpYy5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQECHBhcmFjZWxsCGNvbnN0YW50BAErCGNvbnN0YW50AgEAAAANbG9jYWwuaW50LnNldAhjb25zdGFudAQEY2VsbA5nbG9iYWwuaW50LmdldAhjb25zdGFudAQIcGFyYWNlbGwRZ2VuZXJpYy53YWl0dW50aWwOY29tcGFyaXNvbi5pbnQOZ2xvYmFsLmludC5nZXQIY29uc3RhbnQEC3VucGFyYWxlYXNoCGNvbnN0YW50BAI9PQhjb25zdGFudAIBAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAwAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgMAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIDAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAwAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgMAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIDAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAwAAAAhtaW5lLmRpZw1sb2NhbC5pbnQuZ2V0CGNvbnN0YW50BARjZWxsCGNvbnN0YW50AgMAAAAIbWluZS5kaWcNbG9jYWwuaW50LmdldAhjb25zdGFudAQEY2VsbAhjb25zdGFudAIDAAAACG1pbmUuZGlnDWxvY2FsLmludC5nZXQIY29uc3RhbnQEBGNlbGwIY29uc3RhbnQCAwAAAAxnZW5lcmljLmdvdG8IY29uc3RhbnQCBAAAAA==`

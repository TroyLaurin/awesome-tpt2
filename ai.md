# Awesome AI for TPT2

The game contains an AI component that lets players script many aspects of the game.  Note that this page may contain (links to) spoilers.  Don't click in a section unless you're willing to be spoiled for that section.

See https://www.perfecttower2.com/wiki/AI for a high level description.

## Contents

- [Overview](#overview)
- [Miscellaneous](#miscellaneous)
- [New round](#game-automation)
- [Power plant](#power-plant)
- [Mine](#mine)
- [Factory](#factory)
- [Laboratory](#laboratory)
- [Shipyard](#shipyard)
- [Trading post](#trading-post)
- [Workshop](#workshop)
- [Museum](#museum)
- [Fun](#fun)

## Overview
(game version: 0.6.6b2)
The AI can run up to 100 scripts and hold 100 global variables at once.  If you attempt to run too many scripts the AI will shut down, stopping all running scripts.    Each running script executes a single instruction per tick<sup>[1](#footnote-1)</sup>, in the order that the scripts were launched.  The first line of each script will be executed in the same tick as the script was launched<sup>[2](#footnote-2)</sup>, which can be used to start many scripts in a single tick.  If multiple scripts are launched by the same impulse, the scripts will be launched in the order that they appear in the AI list (defined by the order in which the scripts were created, currently not easily reorder-able).

When importing scripts, they are disabled by default<sup>[3](#footnote-3)</sup>.  After importing, select the script, tick the enabled box and hit save to enable the script to be run, using any defined impulses in the script once the AI is running (F4 by default)

## Miscellaneous

- [External script editor](https://kyromyr.github.io/perfect-tower/) can be easier to use to create complex scripts.
- [In-game auto clicker](https://discord.com/channels/488444879836413975/783731338304946217/783837888986873946)
- [Delta example](https://discord.com/channels/488444879836413975/783731338304946217/785323947884937226)
- [Relative coordinates](https://discord.com/channels/488444879836413975/783731338304946217/786398112440647700) - this was crucial before the game introduced screen size functions
- [In-game macro recorder](https://discord.com/channels/488444879836413975/783731338304946217/786467731137363984)
- [Trying to find the screen resolution](https://discord.com/channels/488444879836413975/783731338304946217/792239175981334538) - before it was available in-game
- [A bunch of relative coordinates](https://discord.com/channels/488444879836413975/783731338304946217/792256274145345608)


## New round
These scripts are useful in the "main" tower defense section of the game

- [Auto skill](https://discord.com/channels/488444879836413975/783731338304946217/783731855387918386)
- [Auto restart](https://discord.com/channels/488444879836413975/783731338304946217/785804221949411368) - one of the early shared scripts, click positions aren't relative so will need to be updated for your screen size.  Gives a good overview of what you may want to automate in town
- [Auto restart](https://discord.com/channels/488444879836413975/783731338304946217/793857420593201162) - a variant on the above that should work on all screen sizes
- [Another idling script](https://discord.com/channels/488444879836413975/783731338304946217/793857420593201162)

## Power plant

## Mine

The mine is a great place to learn scripting.  Most functions can be performed by specific AI actions, it's a good introduction to looping, and there are more advanced things you can do by clicking on the screen.

- [Automine active layer](https://discord.com/channels/488444879836413975/783731338304946217/783732998146490389)
- [Darkrai3333
#1337's automining scripts](https://discord.com/channels/488444879836413975/783731338304946217/783741985198571592)
- [nineforty
#7882's fast two-script autominer](https://discord.com/channels/488444879836413975/783731338304946217/785079688925413417)
- [Xenos
#2117's superdrill script](https://discord.com/channels/488444879836413975/783731338304946217/793398222575370270)
- [Xenos#2117's one-tick miner](https://discord.com/channels/488444879836413975/783731338304946217/794752593695080510) - probably the start of "turbo exec"
- [SharkBite
#8908's paradrill](https://discord.com/channels/488444879836413975/783731338304946217/796662930442551337) - sure it's fast (~3s), but why wouldn't you just use the turbo miner at this point?

## Factory

- [Dust tiering](https://discord.com/channels/488444879836413975/783731338304946217/783732405722808350)
- [More dust tiering](https://discord.com/channels/488444879836413975/783731338304946217/795419856777773057)

## Laboratory

- [Nature experiment](https://discord.com/channels/488444879836413975/783731338304946217/783731613183639624)
- [Another nature experiment](https://discord.com/channels/488444879836413975/783731338304946217/787701079105339405)
- [Electricity experiment](https://discord.com/channels/488444879836413975/783731338304946217/791110848143687731) - fully upgrade a single coil

## Shipyard

- [Random note](https://discord.com/channels/488444879836413975/783731338304946217/791448799821430826) - collect ANY active shipment, then order a specific shipment

## Trading post

## Workshop

- [Module tier buyer](https://discord.com/channels/488444879836413975/783731338304946217/785311908756062229)
- [Turn ingots into dust](https://discord.com/channels/488444879836413975/783731338304946217/786718963521945621)

## Museum

- [Museum powerstone script](https://discord.com/channels/488444879836413975/783731338304946217/783731561962668072)
- [Powerstone buy, combine and equip](https://discord.com/channels/488444879836413975/783731338304946217/784604847508422656)
- [Another powerstone script](https://discord.com/channels/488444879836413975/783731338304946217/784924553394520074)
- [Yet another powerstone script](https://discord.com/channels/488444879836413975/783731338304946217/786584100545757235) - this one runs 95 scripts in parallel
- [More powerstone levelling](https://discord.com/channels/488444879836413975/783731338304946217/789502743098163260) - this one chooses which colour to buy based on what you put in your first 8 equipped slots
- [Turbo exec powerstone levelling](https://discord.com/channels/488444879836413975/783731338304946217/795750425474629674)
## Fun

Scripts that aren't necessarily about playing the base game, but add something fun to the game.

- [Sudokustone teaser](https://discord.com/channels/488444879836413975/783731338304946217/797284972011847710) - sudoku in a random internet game? Awesome!

## Footnotes

1. <a name="footnote-1"></a> https://discord.com/channels/488444879836413975/783026949715001364/788339004012494878
2. <a name="footnote-2"></a> https://discord.com/channels/488444879836413975/783731338304946217/793603392920223785
3. <a name="footnote-3"></a> https://discord.com/channels/488444879836413975/783731338304946217/792901138361745429

# INA219 Current/Power Monitor Board

A 4-layer PCB for high-side current and power sensing using the TI 
INA219, built as a self-directed mixed-signal design exercise. This 
board bridges an analog current-sense front end with a digital I2C 
interface, and was the first project in this series to require real 
attention to signal isolation and layer planning rather than single-layer 
routing.

## Overview

The board measures current through a 4-terminal Kelvin-sense shunt 
resistor placed in the high-side power path (POWER in → shunt → LOAD 
out), and reports current, bus voltage, and power over I2C via the 
INA219. It's designed as an in-line sensor: the board only carries the 
positive/hot side of the current path, with the circuit's return path 
completed externally through the system's own ground wiring.

## Key Design Features

- **INA219AxDCN** current/power monitor IC
- **4-terminal Kelvin-sense shunt resistor** (10mΩ), with dedicated sense 
  taps landing directly on the shunt's own sense pads — not tapped 
  further down the power trace — to avoid measurement error from trace 
  resistance
- **RC filter network** (10Ω + 0.1µF) on the IN+/IN- sense lines per 
  TI's recommended application circuit, to reduce noise reaching the 
  PGA's sensitive input stage
- **4-layer stackup** with a dedicated internal ground plane, chosen 
  specifically to give the analog sense signals and digital I2C lines a 
  clean, consistent return path without them competing for the same 
  copper
- Standard I2C pull-ups (3.3kΩ) and VS decoupling (0.1µF)

## Design Challenges Solved

- **Signal separation to avoid crosstalk**: worked through how to 
  physically separate the analog sense path (shunt → filter → INA219 
  input) from the digital I2C lines, so switching noise on SDA/SCL 
  doesn't couple into the sensitive current measurement
- **Junction planning to minimize vias on the ground plane**: rather than 
  routing every ground connection down through a via, used junctions and 
  layer choice deliberately to keep the internal ground plane as 
  continuous as possible, avoiding unnecessary Swiss-cheesing of the 
  reference plane
- **Kelvin sensing implementation**: correctly separated the 
  high-current power path pins from the low-current sense pins on the 
  shunt resistor, both in schematic and layout, to get an accurate 
  measurement

## Files

- `.kicad_sch` / `.kicad_pcb` — full KiCad source project
- Schematic PDF export
- 3D board render

## Tools

Designed entirely in KiCad 10.

## Status

Schematic and PCB complete.

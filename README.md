# a5800-BONDPogo

Prism BOND Pogo Board Template.

A Pogo board carries the spring pins (pogo pins) that touch the test points of your Device Under
Test (DUT).  It plugs into the four headers of BOND, Sistemi's open source PCBA tester, and each
test point is wired to a BOND header pin.  The BOND driver in Sistemi's
[scripts](https://github.com/sistemicorp/scripts) repo then controls those pins from your Prism
test scripts.

This repo is an Altium Designer project with everything a new Pogo board needs except your test
points: the four BOND headers, already placed where they mate with BOND, a spring pin part, and a
3D model of BOND for the stack up.

Read the documentation section
[Designing the Pogo Board](https://sistemicorp.github.io/scripts/build/html/prism_bond.html#designing-the-pogo-board)
before you start.


## Stack Up

    DUT            test points face down
    ---------
    | | | |        spring pins
    Pogo board     this project
    ---------
    | |   | |      BOND headers TH1-TH4 (Molex 90151-2120)
    BOND           components face away from the Pogo board

The BOND headers face the Pogo board, and the BOND components face away from it, see the
`Stackup.SchDoc` sheet.


## Files

| File | What it is |
|------|------------|
| `a5800-BONDPogo.PrjPcb` | Altium project |
| `Top.SchDoc` | Top sheet, add your test points here |
| `TH1.SchDoc` - `TH4.SchDoc` | One sheet per BOND header: the BOND function and range of each pin |
| `Stackup.SchDoc` | Stack up illustration and notes |
| `a5800-CAD_Pcb.PcbDoc` | PCB layout, with the headers placed and a 3D model of BOND |
| `a5800-SCH_01.pdf` | The template schematic as a PDF |
| `a5800-CAD_3DwBOND_01.step` | STEP model of the Pogo board on BOND |
| `a4402-SCH_05.pdf` | BOND schematic, for the header pin functions and BOND population options |
| `Library/` | Schematic and PCB libraries, the spring pin (`S13-503-SISTEMI`) and headers |
| `Release.outjob`, `*.PCBDwf`, `BoM.BomDoc` | Fabrication and assembly outputs |


## Start Your Pogo Board

Your Pogo board is usually private, and a GitHub fork of a public repo can't be private.  Clone
this repo, rename it, and push it to a new private repo,

    git clone https://github.com/sistemicorp/a5800-BONDPogo.git myproduct-Pogo
    cd myproduct-Pogo
    git remote rename origin sistemi
    git remote add origin git@github.com:<your org>/myproduct-Pogo.git
    git push -u origin master

Create the empty private repo `<your org>/myproduct-Pogo` on GitHub first.  The `sistemi`
remote lets you fetch later changes to this template.

Then, in Altium,

1. Rename the project and the title blocks to your own part number.
2. In `Top.SchDoc`, copy `TP1` once for each DUT test point, and name it after the test point
   and its net, for example `TP1-VDD`, `TP2-IN5`.  Use several GND spring pins, `TPG1`, `TPG2`...
3. Connect each test point to a header net on the `TH1`-`TH4` sheets.  The sheets give each
   pin's BOND function and range.
4. Check the BOND schematic `a4402-SCH_05.pdf` for the population options of your BOND.  BOND
   has some flexibility, and the template may need to change to match your BOND.
5. In the layout, place the spring pins under the DUT test points.  **Do not move the headers
   TH1-TH4**, they must line up with BOND.
6. Add the 3D model of your DUT, and export a STEP of the whole stack up (BOND, Pogo board,
   DUT) for the jig's mechanical design.

The header pins are set up in software with a header definition file,
`pogo_hdr_definition._json`, see the BOND driver in the scripts repo,
`public/prism/drivers/A4401_BOND/README.md`.  Keep it in step with this schematic, pin by pin.

# Changelog of local customizations

## Version 15-Mar-2018 (v3.1.0 r10)

### Updates and mods

* Default to 250000 Baud for platformio builds 
* Replace watchdog (disabled by default)

### Bug fixes

* LA bugfix from @Sebastianv650

## Version 27-Feb-2018 (v3.1.0 r9)

This release incorporates a number of experimental features and structural changes

### Updates and mods

* Integration of @Sebastianv650 Linear Advance v1.5 Details can be found in the [Marlin](http://marlinfw.org/docs/features/lin_advance.html) documentation.
* New internal ISR structuring to prevent temperature / fan ISR from stalling stepper and allowing serial RX to be highest priority. CheckRX is no longer needed. 
* It is now possible to use the USB interface at 250000 Baud without errors provided the host can handle higher data rates. A firmware rebuild is required to enable capability.
* Add Calibration LCD menu item for optionally disabling XY skew correction.
* XY Skew correction menu item only enabled only when skew is 'perfect' (< 0.1deg).

### Bug fixes

* Fix HOME-Z to correct location (calibration point). Fixes dinging the heat-bed after ``Setup Wizard``
* Remove attempted recovery from RX buffer full (just drop chars). This includes a fix to append 'ok' after resend request when line number or checksum error detected. Addresses Prusa-Firmware issue [#331](https://github.com/prusa3d/Prusa-Firmware/issues/331)
* Use CRITICAL_SECTION in LCD menu and buttons sync.
* Eliminate excessive LCD redraw and flicker.

## Version 20-Dec-2017 (v3.1.0 r4)
### Updates and mods
- Allow BAUDRATE override by build opts (default 115200).
- Remove unused source files (qr_solve, vector_3).
- Use SDK version of LiquidCrystal library (faster draw).
- Adjust extra filament load extrusion to 15mm.
- Add private build variant configuration version number.

## Bug fixes
- Init serial before lcd to elminate random garbage output.
- Remove redundant splash screen init.

## Version 22-Nov-2017
### Updates and mods
- Add extra 25mm to manual (LCD) filament load extrusion.
- Merge latest Prusa3D firmware release v3.1.0 changes.

### Bug fixes
- Fix M110 G-code compatibility

## Version 9-Nov-2017
### New features

- Add 8-point Mesh Bed Correction offsets (+/- 500um) support (optional: _MBC_8POINT_)<br/>
Points labeled **a** thru **h** starting at the front/home position. Supported by G80 command and persisted to EEPROM via LCD menus.
```
       B A C K
    +-----------+
    | f   g   h |
    | d   X   e |
    | a   b   c |
    +-----------+
      F R O N T
```

- Replace LiquidCrystal display driver with recent version (experimental)
- Add ability to build single language version (see ``langtool.pl``). This feature saves ~32K program space.
- Add custom G-code to dump and restore EEPROM (optional: _EEPROM_SAVE_RESTORE_)
```
   M767: Dump EEPROM to serial (suitable for restore) or to SD File
     Ex: M767 S --> dump to SD card 'eesave.hex'
         M767 P<addrs> --> Write M768 P<addrs> V<value> to SERIAL
         M767 X --> dump entire EEPROM to SERIAL
   M768: Set EEPROM location from value
     Ex: M768 P<addrs> V<value>
         M768 R --> restore from SD card
```
- Allow embedded colons (':') in G-code. Ex: "`M117 Time is 11:56`" now works!
- Reduce surface artifacts caused by XY skew compensation (experimental). Ref: [Issue #47](https://github.com/prusa3d/Prusa-Firmware/issues/147)
- Clarify temperature and PID calibration menu items. Setup Wizard display name changed.

### Bug fixes

- Complete cleanup of build warnings.
- Removal of dead code and unreferenced variables.
- Fixed a couple of array out-of-bounds references.

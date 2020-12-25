![Lint](https://github.com/sbidy/wiz_light/workflows/Lint/badge.svg) 
# :bulb: wiz_light - V 0.2
A Home Assistant integration for WiZ Light bulbs and fixtures produced by Philips, SLV and others. The Wiz Plug **currently in beta** is supported as "switch" in Home Assistant.

## :muscle: Change Log
**Testing/Contribution Required:** I released the integration V0.2 --> check [Releases](https://github.com/sbidy/wiz_light/releases/tag/v0.2)
Please give feedback if any thing is missing or broken.

Issue [#62](https://github.com/sbidy/wiz_light/issues/62) should now be fixed. The update to `pywizlight` version 0.3.8 is important!

Added a better handling of different bulb types via a YAML file with dynamic matching.

## :warning: Discussions
If you have questions or other comments please use the **new** [Discussions Board](https://github.com/sbidy/wiz_light/discussions).

## :blue_heart: Kudos and contributions
Thank you [@angadsingh](https://github.com/angadsingh) for make such incredible improvements!!

Thanks to [@simora](https://github.com/simora) for create a HA Switch <-> WiZ Plug integration

Thanks to [@fabaff](https://github.com/fabaff) for the CLI tool and some code rework!

Bug fixes:
 - Fixes https://github.com/sbidy/wiz_light/issues/6: make the whole component truly async using non-blocking UDP
 - Light control now works even when lights are set to a rhythm.

Features:
 - Supports switching the light to rhythm mode! (rhythm is defined as a scene for HA)
 - Implements a pattern of sending multiple command UDP datagrams until response is received
 - Consolidates getPilot and setPilot calls using a PilotBuilder and PilotParser. Removes unnecessary UDP calls for each and every attribute (color, temperature, brightness, scene, etc.) and makes a combined getPilot/setPilot call
 - Enhanced debug logging for UDP

This component has a dependency on `pywizlight` which will be installed automatically by Home Assistant.

## Bulbs
| Bulb Type | Dimmer | Color Temp | Effects | RGB | Tested? | Example Product |
|-----------|--------|------------|---------|-----|-----|-----|
| ESP01_SHDW_01 | ✔️ |   |   |   |  | |
| ESP01_SHRGB1C_31 | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | • Philips 555623 recessed <br /> • Philips 556167 A19 Frosted Full Colour and Tunable White|
| ESP01_SHTW1C_31 | ✔️ | ✔️ |   |   | ✔️ | • Philips 555599 recessed |
| ESP56_SHTW3_01 | ✔️ |  ✔️  | ✔️  |   | ✔️ | |
| ESP01_SHRGB_03 | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | |
| ESP01_SHDW1_31 | ✔️ |  |  |  |  |  |
| ESP06_SHDW1_01 | ✔️ |  |  |  |  |  |
| ESP15_SHTW1_01I | ✔️ | ✔️ |  |  |  |
| ESP03_SHRGB1C_01 | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | • Philips Color &. Tunable-White A19 <br />• WiZ A60 E27 EAN 8718699787059 <br />• WiZ G95 E27 EAN 8718699786359|
| ESP03_SHRGB1W_01 | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | • Philips Color &. Tunable-White A21 <br />• WiZ A67 E27 EAN 8718699786199|
| ESP06_SHDW9_01 | ✔️ |  |  |  | ✔️ | • Philips Soft White A19 |
| ESP03_SHRGBP_31 | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | • Trio Leuchten WiZ LED |
| ESP17_SHTW9_01 | ✔️ | ✔️ |  |  | ✔️ | • WiZ Filament Bulb EAN 8718699786793 |
| ESP03_SHRGB3_01ABI | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ | 

More bulbs can be found in the `bulblibrary.yaml`

If you have new bulbs to contribute please open a new issue using the *Bulb Test Report* template. You can find the required information by running the following command using `nc` on a linux host.

`echo '{"method":"getSystemConfig","params":{}}' | nc -u -w 1 <YOU BULB IP> 38899`

## Working features 
 - Brightness
 - Color (RGB)
 - White Color Temperature
 - On/Off, Toggle
 - Effects
 - Setting a rhythm as a scene
 - Switch integration (thanks to @simora)

## Next improvement:
- Testing with other hardware -- **Contribution required !!**
- Config Flow Support

## Pull request in HA core
https://github.com/home-assistant/core/pull/44522

## Install for testing 

1. Loggon to your HA or HASS with SSH
2. Got to the HA `custom_components` directory within the HA installation path (if this is not available - create this directory).
3. Run `cd custom_components`
4. Run `git clone https://github.com/sbidy/wiz_light` within the `custom_components` directory
5. Run `mv wiz_light/custom_components/wiz_light/* wiz_light/` to move the files in the correct diretory
6. Restart your HA/HASS service in the UI with `<your-URL>/config/server_control`
7. Add the bulbs to your `configuration.yaml` - You can not add the bulbs in the HA UI!! (configFlow is missing)

As an alternative you can use the HACS platform for installation - see [HACS Website](https://hacs.xyz)

Questions? Check out the github project [pywizlight](https://github.com/sbidy/pywizlight)

## Testing
See `test.py` for how the underlying API works

## HA config
To enable the platform integration after installation add 
```
light:
  - platform: wiz_light
    name: <Name of the device>
    host: <IP of the bulb>
  - platform: wiz_light
    name: <Name of the device#2>
    host: <IP of the bulb#2>
```
If you want to use the integration as switch
```
switch:
  - platform: wiz_light
    name: <Name of the device>
    host: <IP of the socket>
```

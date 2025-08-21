https://github.com/olavinjapa/TangNano9K-Galaxian/releases

# 🚀 Galaxian Arcade Core for Tang Nano 9K — FPGA Game Hardware

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/olavinjapa/TangNano9K-Galaxian/releases)  
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE) [![Language](https://img.shields.io/badge/Verilog-%2F-VHDL-orange)](#)

![Galaxian Arcade Screenshot](https://upload.wikimedia.org/wikipedia/en/5/5c/Galaxian_arcade_game.png)
![Tang Nano 9K Board](https://raw.githubusercontent.com/sipeed/TangNano/master/images/tang_nano_9k.jpg)

Tags: 9k, arcade, fpga, galaxian, game, gowin, mist, mister, nano, pinballwiz, pinballwizz, sipeed, tang, tangnano, tangnano9k, verilog, vhdl

About
- A synthesis of the classic Galaxian arcade game compiled for the Sipeed Tang Nano 9K FPGA board.
- The core reproduces arcade video, sound, and inputs with minimal resources.
- Target users: FPGA hobbyists, retro-game fans, MiST/MiSTer community, and hardware tinkerers.

Features
- Cycle-accurate sprite handling for Galaxian-style waves.
- 256-color palette support via simplified palette RAM.
- Composite video or VGA output (depending on build/config).
- Audio: square/PCM channels for arcade sound effects and music.
- GPIO input for joystick and buttons, plus optional arcade joystick adapter.
- Small resource footprint to fit Tang Nano 9K (Gowin GW1N-9).
- Build scripts for Verilog and optional VHDL modules.
- Example MiST-compatible wrapper for integration with MiST/MiSTer frontends.

Supported hardware
- Primary: Sipeed Tang Nano 9K (GW1N-9 FPGA).
- Compatible boards: other Gowin GW1N-9/16 boards with similar pinout.
- Optional: MiST, MiSTer using the MiST-compatible wrapper core.

Releases and downloads
- Visit the releases page and download the provided asset. The release contains a top-level FPGA bitstream (.bit or .bin) plus a README and pinout CSV.
- After downloading the release file, flash it to your Tang Nano 9K and execute the core on the board.
- Release page: https://github.com/olavinjapa/TangNano9K-Galaxian/releases

Release file details (typical)
- TangNano_Galaxian_vX.Y.bin — FPGA flash image for Tang Nano 9K
- TangNano_Galaxian_vX.Y.bit — bitstream for programmer
- pinout.csv — recommended pin mapping for audio, VGA/composite, and buttons
- build/ — build logs and intermediate files
- docs/ — schematics and reference images

Quick start — download, flash, play
1. Download the release asset from the Releases page above.
2. Connect Tang Nano 9K to your PC via USB.
3. Use Gowin Programmer or openFPGALoader to flash.
   - Gowin Programmer (GUI): load the .bit/.bin, connect, and program.
   - openFPGALoader (CLI): openFPGALoader -b gw1n -p TangNano_Galaxian_vX.Y.bin
4. Connect display:
   - VGA: use simple level-shifter or VGA adapter on given pins.
   - Composite: connect RCA to composite output pin.
5. Connect controls:
   - Joystick: map to GPIO pins listed in pinout.csv.
   - Buttons: map to BTN0..BTN3 for Start/Fire/Coin/Service.
6. Power board and press reset. The game boots to the attract screen.

Hardware pinout (example)
- VGA HSync: PIN_A1
- VGA VSync: PIN_B1
- VGA R/G/B: PIN_C1 / PIN_D1 / PIN_E1 (TTL level, use resistor network)
- Composite video: PIN_F1 (NTSC/PAL filter recommended)
- Audio L/R: PIN_G1 / PIN_H1 (line-level)
- Joystick Up/Down/Left/Right: PIN_J1 / PIN_K1 / PIN_L1 / PIN_M1
- Buttons: BTN0 (Start), BTN1 (Fire), BTN2 (Coin), BTN3 (Service)
- See pinout.csv in release for a complete, board-specific mapping.

Build from source
Prerequisites
- Gowin toolchain (Gowin IDE or gowin_rtk tools)
- openFPGALoader (optional)
- make, Python 3
- Verilog simulator (for testbench runs)
- Git

Repository layout (source)
- cores/galaxian/ — main Verilog/VHDL implementation
- boards/tang_nano9k/ — board pin files and constraints
- tools/ — build and packaging scripts
- docs/ — design notes and schematics
- sim/ — testbenches and waveforms
- assets/ — sample ROM images and sprites for testing

Build steps
1. Clone the repo:
   - git clone https://github.com/olavinjapa/TangNano9K-Galaxian.git
2. Enter the board folder:
   - cd boards/tang_nano9k
3. Run the build script:
   - make TARGET=tang_nano9k
4. The build produces a .bit and .bin in build/output/.
5. Flash using Gowin programmer or openFPGALoader as shown above.

ROMs and legal
- The core requires the original Galaxian ROM set to run. This repo includes test ROMs or homebrew replacements for development.
- Do not distribute copyrighted ROMs. You must provide your own legally obtained ROM dump.
- For development, use included demo assets that mimic ROM behavior.

Video and audio modes
- The core supports two video modes:
  - VGA mode: 640x480 (scaled), RGB TTL pins, HSync/VSync.
  - Composite mode: NTSC/PAL composite on single pin; uses a simple DAC and filter.
- Audio uses a small PWM DAC or simple R-2R resistor network. Output is line-level and needs coupling capacitor.

Controls mapping
- Joystick (4-way or 8-way): mapped to GPIO pins. Constrain to TTL switch inputs.
- Buttons:
  - BTN0 = Start
  - BTN1 = Fire
  - BTN2 = Coin
  - BTN3 = Service
- If you use arcade encoder (e.g., USB encoder), map encoder outputs to corresponding GPIO pins.

MiST / MiSTer compatibility
- The repository includes a MiST wrapper to expose the core to MiST frontends.
- Compile the MiST wrapper and integrate the core using the MiST build flow.
- Use the included MiST descriptor file to allow frontend selection and ROM mapping.

Common commands
- openFPGALoader flash:
  - openFPGALoader -b gw1n build/output/TangNano_Galaxian_vX.Y.bin
- Gowin programmer script:
  - gowin_prog -b gw1n -f build/output/TangNano_Galaxian_vX.Y.bit
- Run simulation:
  - cd sim && make run

Testing and verification
- The repo includes a testbench that runs sprite fetch, collision, and audio streams in simulation.
- Use waveform viewer (GTKWave) to inspect signal timing and bus behavior.
- Test on hardware using both video modes and check timing with an oscilloscope if available.

Performance and resource use
- The core fits within 9K LUTs with margin for small features.
- Typical resource usage:
  - LUTs: ~70%
  - BRAM: low
  - DSP: none
- Clock runs at 27 MHz for composite timing or 50/60 MHz for VGA depending on build variant.

Troubleshooting
- No video on screen:
  - Check pinout mapping in pinout.csv and match board silk.
  - Verify clock oscillator and power rails on Tang Nano 9K.
- Sound missing:
  - Check PWM DAC wiring and audio coupling capacitor.
- Controls not responding:
  - Verify pull-up/pull-down resistors on inputs and check debouncing logic.

Credits and references
- Original Galaxian arcade design by Namco.
- FPGA techniques inspired by open-source arcade cores and the MiST community.
- Board images and hardware references courtesy of Sipeed and Gowin.
- Community contributors: see CONTRIBUTORS.md for details.

Contributing
- Fork the repo and open a pull request for fixes and features.
- Report issues in GitHub Issues with steps to reproduce and hardware used.
- Follow the code style in cores/ and add tests for any behavior changes.

License
- MIT License. See LICENSE file.

Changelog
- See Releases page for binary downloads and release notes:
  - https://github.com/olavinjapa/TangNano9K-Galaxian/releases

Useful links
- Tang Nano 9K product page (manufacturer)
- Gowin FPGA tools and documentation
- MiST and MiSTer hardware community pages
- Classic Galaxian history and schematics on arcade archives

Contact and support
- Open an issue for bugs or help.
- For hardware queries, include board version, serial, and pinout.csv used.

Screenshots and media
- Check the docs/screenshots folder in the repo for in-game photos and wiring diagrams.
- Community demos and videos appear in Releases and the project wiki.

How to credit this core
- Use the repository name and link in any public write-up.
- If you use the binary release, include a link to the release page.

End of file
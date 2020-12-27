# Configurations
Pre-tested Configurations for Marlin Firmware bugfix-2.0.x

Marlin Firmware is configured using two files:

- `Configuration.h` contains core configuration options like machine geometry.
- `Configuration_adv.h` contains optional settings for advanced and low level features.

For Graphical LCD these files may also be included:

- `_Bootscreen.h` provides the bitmap for a custom Boot Screen.
- `_Statusscreen.h` provides bitmaps to customize the Status Screen.

See the [Configuration page](https://marlinfw.org/docs/configuration/configuration.html) for more information about configuration and individual configuration options.

# Using these files.
Before you go too far, these configuration files are for compiling Marlin 2.0.x for a Creality Ender 5 with a v4.2.2 board outfitted with a BL Touch 3.1 that is not using a pin 27 adapter board. The zlimit is disconnected, and the BLTouch Z stop is connected to the zlimit port on the board.

Also worth noting, when I first installed this firmware, I was thrown off by the fact that the autohome (G28) feature was used to determine where (220, 220, 0) is. I had gotten used to G28 finding (0,0,0). But I think this work better, since before everything in Cura was kind of backwards.

## Assumptions
I assume you have some familiarity with git and that you are set up to compile marlin using VSCode. 

## Last build
I last build this against commit e2480d40d138aa822f59ac1b317d539238b77a3e of the bugfix-2.0.x branch of the main Marlin repo.

## Rock & Roll

   1. Make a directory structure: mkdir -p ${HOME}/Projects/Marlin && cd ${HOME}/Projects/Marlin
   1. Clone the Marlin Repo: `git clone https://github.com/MarlinFirmware/Marlin.git MarlinFW"
   1. Clone my configs: `git clone git clone git@github.com:kgleason/Configurations.git`
   1. Get to the correct branches:
       * `cd MarlinFW && git checkout bugfix-2.0.x && cd ..`
       * `cd Configurations && git checkout bugfix-2.0.x && cd ..`
   1. Copy in the config files:
       * `cp Configurations/platformio.ini MarlinFW/`
       * `cp Configurations/config/examples/Creality/Ender-5\ Pro/CrealityV422/*.h MarlinFW/Marlin`
   1. At this point, you can try building fron HEAD. It will probably work. But if you want to use the last thing commit that this has bee tested against, copy the last build number from above, and do this: `cd MarlinFW; git checkout e2480d40d138aa822f59ac1b317d539238b77a3e`
   1. Open the MarlinFW folder in VS Code. And build. If it succeeeds, the brilliant. Keep reading. If it fails, the try reverting the last know good build commit (see the previous step).
   1. Make it your own:
       * Search MarlinFW/Marlin/Configuration.h for CUSTOM_MACHINE_NAME and change it to whatever you want to see on the printer's screen.
       * Search MarlinFW/Marlin/Configuration.h for STRING_CONFIG_H_AUTHOR and change "Dust" or "Kirk" to whatever you want. I'm not sure where this shows up, so I didn't change it.
       * Search MarlinFW/Marlin/Version.h for SHORT_BUILD_VERSION and change it to whatever you want. The folks at Marlin leave it at 2.0.x by default, and it shows up on the boot splash. But I like to change it for each build so that I know I have the new firmware loaded quickly when I update it. So I use "2.0.MMDDB" where:
           * MM: Is the build month
           * DD: Is the 2 digit build date.
           * B: Is the number of times I've built.
        So you can see that on Dec 27, I built 2 different times. And the second one worked.

# To Do
I really should script some of this out. For example, cloning and changing to the right branch and everything. 

I'd also like to find a way to make PlatformIO increment the SHORT_BUILD_VERSION for me automatically on each build. 
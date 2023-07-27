# Modifications used for the release of **[Launcher Heroes](http://launcherheroes.com/)**:

Newly Added Functions:
- xboxone_update_recent_players(user_id, user_id_recent) - this has since been added to the offical, release, but I have not tested their version. My version only takes one user id, the offical version supports multiple as an array.
  
Reenabled Functions (these functions were not setup when this extension was presumable ported from the Xbox export. I have set them up to be usable):
- xboxone_matchmaking_start(user_id)
- xboxone_matchmaking_send_invites(from_user, session_to_invite_into, text_to_add_to_invite)
- xboxone_matchmaking_create(user_id, visibility, template, hopper, sdatemplate)
- xboxone_matchmaking_set_joinable_session(user_id, session_id)
- xboxone_matchmaking_join_invite(user_id, invitation_id, invitation_host, sda_template_name)
- xboxone_matchmaking_session_leave(session_id)
  
Misc Changes:
- xboxone_gamertag_for_user(user_id) - Uses DisplayName instead of ModernGamertag
- Created my own logger since I couldn't get the existing logging functionality to work with GameMaker. You will need to uncomment the code and set a text file to write your logs to. Modify the function DebugConsoleOutput() in GDKX.cpp if you need logging, otherwise ignore this.

# GameMaker Studio 2 - GDK Extension

An Extension for GameMaker Studio 2 (GMS2) that gives GMS2 Windows Target support for the GDK allowing them to be released on the Microsoft Store and use XBox Live functionality (for those developers that have access through id@xbox, see [this link](https://www.xbox.com/developers/id) for more information on id@xbox).

For more information on how to use the GDK Extension check our [tech blog](https://www.yoyogames.com/en/blog/gdk-extension) and for extra details on configuring the Partner Center check out our [zendesk article](https://help.yoyogames.com/hc/en-us/articles/4411044955793).

**NOTE**: Only Windows x64 Target is supported by the GDK, ensure that your GMS2 project has the x64 option selected in **Options &#8594; Windows &#8594; General**

# Table Of Contents

- [GameMaker Studio 2 - GDK Extension](#gamemaker-studio-2---gdk-extension)
- [Table Of Contents](#table-of-contents)
	- [Contents of this repository](#contents-of-this-repository)
	- [Building this Extension](#building-this-extension)
	- [Running the GMS2 Project](#running-the-gms2-project)

--- 

## Contents of this repository

This repository contains the source code for the DLL that implements the GDK functionality that is exposed to GameMaker games, it is written in C++. It also contains an example GMS2 project that contains the extension definition and illustrates how to use the extension.

---

## Building this Extension


1. Install VS2019 - see https://visualstudio.microsoft.com/downloads/ 
2. Install GDK - see https://github.com/microsoft/GDK/releases/tag/October_2021_Republish
3. Install CMAKE - see https://cmake.org/download/
4. Clone this repository (NOTE: This repository has submodules)
5. Open the Visual Studio 2019
6. Open the Solution in DLL/GDKExtension.sln
7. Go to (Project Properties --> C/C++ -> General -> Additional Include Directories) and add the path: `C:\ProgramData\GameMakerStudio2\Cache\runtimes\<current-runtime>\yyc\include\` (may be different in you system)
8. Select the Debug|Gaming.Desktop.x64 or Release|Gaming.Desktop.x64
9. Build

**NOTE**: Output from this build will be copied into the GMS2 GDK project

---

## Running the GMS2 Project

Open the GMS2 Project in this repository from GDK_Project_GMS2/GDK_Project_GMS2.yyp file.

> **NOTE**: Full documentation is in the project included files.

---

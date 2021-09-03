# GameMaker Studio 2 - GDK Extension

An Extension for GameMaker Studio 2 (GMS2) that gives GMS2 Windows Target support for the GDK allowing them to be released on the Microsoft Store and use XBox Live functionality (for those developers that have access through id@xbox, see INSERTLINK for more information on id@xbox).

**NOTE**: Only Windows x64 Target is supported by the GDK, ensure that your GMS2 project has the x64 option selected in **Options &#8594; Windows &#8594; General**

# Table Of Contents

* [Contents of this repository](#Contents-of-this-repository)
* [Building this Extension](#Building-this-Extension)
* [Running the GMS2 Project](#Running-the-GMS2-Project)
* [GML Function reference](#GML-Function-reference)

--- 

## Contents of this repository

This repository contains the source code for the DLL that implements the GDK functionality that is exposed to GameMaker games, it is written in C++. It also contains an example GMS2 project that contains the extension definition and illustrates how to use the extension.

---

## Building this Extension


	1. Install VS2019 - https://visualstudio.microsoft.com/downloads/ 
	2. Install see https://github.com/microsoft/GDK (clone repository then run the installer)
	3. Install CMAKE - see https://cmake.org/download/
	3. Clone this repository (NOTE: This repository has submodules)
	4. Open the Visual Studio 2019
	5. Open the Solution in DLL/GDKExtension.sln
	6. Select the Debug|Gaming.Desktop.x64 [or Release|Gaming.Desktop.x64 configurations COMING SOON]
	7. Build

NOTE: Output from this build will be copied into the GMS2 GDK  project

---

## Running the GMS2 Project

Open the GMS2 Project in this repository from GDK_Project_GMS2/GDK_Project_GMS2.yyp file.

---
---

# GML Function reference

**NOTE**: these functions should work the same as those in the xbox one and xbox series x/s targets and we have adopted the same naming so game code can be shared with the extension.

---

## gdk_init  - Initialise the GDK

**Usage**: gdk_init(scid)

**Description**: Must be called before any other GDK extension function, recommend using a Controller persistent object that is created or placed in the first room and this call is in the create event.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **scid** The Service Configuration ID (SCID) presented in the MicrosoftGame.config file.

**Returns**: N/A

---

## gdk_update - Update the GDK

**Usage**: gdk_update()

**Description**: This should be called each frame while GDK extension is active, recommend using a Controller persistent object that is created or placed in the first room and this call is in the step event.

**Returns**: N/A

---

## gdk_quit - Quit the GDK

**Usage**: gdk_quit()

**Description**: This should be called on close down and no GDK extension functions should be called after it, recommend using a Controller persistent object that is created or placed in the first room and this call is in the destroy event.

**Returns**: N/A

---

## xboxone_show_account_picker (TODO)

**Usage**: xboxone_show_account_picker( arg0, arg1 )

**Description**: This function launches the system’s account picker which will associate the selected user with the specified pad. The "mode" argument is either 0 or 1 – if 0 is specified no guest accounts can be selected, while 1 allows guest accounts. This function returns an asynchronous event ID value, and when the account picker closes an **asynchronous dialog event** will be triggered with details of the result of the operation.

**Triggers**: Async Dialog Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"id"**: The event ID.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"type"**: A string containing "xboxone_accountpicker".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"padindex"**: Contains the pad index passed in to the function.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*id*} **"user"**: Contains the User ID (a pointer) returned by the account picker (or null pointer if cancelled).

**Returns**: {*real*} The asynchronous event ID value.

---

## xboxone_get_user_count

**Usage**: xboxone_get_user_count()

**Description**: With this function you can find the total number of users currently signed in to the system. The returned value will be an integer value.

**Returns**: {*integer*} The total number of users currently signed in to the system.

**Code Sample**:
```gml
	for (var i = 0; i < xboxone_get_user_count(); i++) {
		user_id[i] = xboxone_get_user(i);
	}
```
---

## xboxone_get_user

**Usage**: xboxone_get_user(index)

**Description**: With this function you can retrieve the user ID pointer for the indexed user. If the user does not exist, the function will return the constant pointer_null instead. You can find the number of users currently logged in with the function [`xboxone_get_user_count()`](#xboxone_get_user_count).

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **index** The index (an integer) to get the User ID from.

**Returns**: {*integer*} The total number of users currently signed in to the system.

**Code Sample**:
```gml
	for (var i = 0; i < xboxone_get_user_count(); i++;) {
		user_id[i] = xboxone_get_user(i);
	}
```
---

## xboxone_get_activating_user

**Usage**: xboxone_get_activating_user()

**Description**: With this function you can retrieve the user ID pointer for the user that launched the game from the console. Note that this is rarely what you want to do in a game, because this user can logout while the game is running. 

**Returns**: {*pointer*} The user ID pointer.

**Code Sample**:
```gml
	global.main_user = xboxone_get_activating_user();
```
---

## xboxone_fire_event - xboxone_fire_event(event_name, ...)
## xboxone_get_stats_for_user - xboxone_get_stats_for_user(user_id, scid)
## xboxone_stats_setup - xboxone_stats_setup(user_id, scid, title_id)

---

## xboxone_stats_set_stat_real

**Usage**: xboxone_stats_set_stat_real(user_id, stat_name, stat_value)

**Description**: This function can be used to set the value of a stat for the given user ID. You supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), then the stat string to set (if the stat string does not already exist then a new stat will be created and set to the given value) and a value (a real) to set the stat to. Note that the stat name must be alphanumeric only, with no symbols or spaces.

When setting the stat value, any previous value will be overridden, therefore it is up to you to determine if the stat value should be updated or not (ie. check that the high score is actually the highest) by comparing to the current stat value with the new one before setting it.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID pointer to set the stat for.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **stat_name** The statistic to set (a string).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **stat_value** The value to set the stat to (a real).

**Returns**: {*real*} -1 if there was an error, or any other value if the function was successfully called.

**Code Sample**:
```gml
    xboxone_stats_set_stat_real(p_user_id, "TestReal", 123.45);
```
---

## xboxone_stats_set_stat_int

**Usage**: xboxone_stats_set_stat_int(user_id, stat_name, stat_value)

**Description**: This function can be used to set the value of a stat for the given user ID. You supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), then the stat string to set (if the stat string does not already exist then a new stat will be created and set to the given value) and a value (an integer) to set the stat to. Note that the stat name must be alphanumeric only, with no symbols or spaces.

When setting the stat value, any previous value will be overridden, therefore it is up to you to determine if the stat value should be updated or not (ie. check that the high score is actually the highest) by comparing to the current stat value with the new one before setting it.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID pointer to set the stat for.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **stat_name** The statistic to set (a string).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*int*} **stat_value** The value to set the stat to (an integer).

**Returns**: {*real*} -1 if there was an error, or any other value if the function was successfully called.

**Code Sample**:
```gml
    xboxone_stats_set_stat_int(p_user_id, "TestInt", int64(22));
```
---

## xboxone_stats_set_stat_string

**Usage**: xboxone_stats_set_stat_string(user_id, stat_name, stat_value)

**Description**: This function can be used to set the value of a stat for the given user ID. You supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), then the stat string to set (if the stat string does not already exist then a new stat will be created and set to the given value) and a value (a string) to set the stat to. Note that the stat name must be alphanumeric only, with no symbols or spaces.

When setting the stat value, any previous value will be overridden, therefore it is up to you to determine if the stat value should be updated or not (ie. check that the high score is actually the highest) by comparing to the current stat value with the new one before setting it.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID pointer to set the stat for.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **stat_name** The statistic to set (a string).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **stat_value** The value to set the stat to (a string).

**Returns**: {*real*} -1 if there was an error, or any other value if the function was successfully called.

**Code Sample**:
```gml
    xboxone_stats_set_stat_string(p_user_id, "TestString", "YoYo Games");
```
--- 

## xboxone_stats_delete_stat

**Usage**: xboxone_stats_delete_stat(user_id, stat_name)

**Description**: This function can be used to delete a stat from the stat manager for the given user ID. You supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), then the stat string to delete. This clears the stat value and removed it from the stat manager, meaning it will no longer be returned by the functions [`xboxone_stats_get_stat_names()`](#xboxone_stats_get_stat_names) or  [`xboxone_stats_get_stat()`](#xboxone_stats_get_stat).

The function will will return -1 if there was an error or the user ID is invalid, or any other value if the function was successfully called.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID pointer to get the stat for.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **stat_name** The stat to be deleted (a string).

**Returns**: {*real*} -1 if there was an error, or any other value if the function was successfully called.

**Code Sample**:
```gml
    for (var i = 0; i < xboxone_get_user_count(); i++) {
		user_id[i] = xboxone_get_user(i);
		xboxone_stats_delete_stat(user_id[i], "highScore");
	}
```
---

## xboxone_stats_get_stat

**Usage**: xboxone_stats_get_stat(user_id, stat_name)

**Description**: This function can be used to retrieve a single stat value from the stat manager for a given user. You supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), and then the stat string as defined when you created it using the one of the `xboxone_stats_set_stat_*` functions. The return value can be either a string or a real (depending on the stat being checked) or the GML constant undefined if the stat does not exist or there has been an error.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID pointer to get the stat for.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **stat_name** The stat to get the value of (a string).

**Returns**: {*real/string/undefined*} The value of the given stat, `undefined` if the stat does not exist.

**Code Sample**:
```gml
    if (game_over == true) {
		if (xboxone_stats_get_stat(p_user_id, "PercentDone") < 100) {
			var _val = (global.LevelsFinished / global.LevelsTotal) * 100;
			xboxone_stats_set_stat_real(p_user_id, "PercentDone", _val);
		}
	}
```
---

## xboxone_stats_get_stat_names

**Usage**: xboxone_stats_get_stat_names(user_id)

**Description**: This function can be used to retrieve all the defined stats from the stat manager for a given user. You supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), and the function will returns an array of strings containing the statistics for the user. If an error occurs or the user has no stats the array will still be returned but will be empty.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID pointer to get the stats for.

**Returns**: {*array*} Array with all the stat names for the given user.

**Code Sample**:
```gml
    var _stat_str = xboxone_stats_get_stat_names(user_id);
    for (var i = 0; i < array_length(_stat_str); i++;) {
        xboxone_stats_delete_stat(user_id, _stat_str[i]);
    }
```
---

## xboxone_stats_add_user

**Usage**: xboxone_stats_add_user(user_id)

**Description**: This function can be used to add a given user to the statistics manager. This must be done before using any of the other stats functions to automatically sync the game with the xbox live server and retrieve the latest values. You supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), and the function will return -1 if there was an error or the user ID is invalid, or any other value if the function was successfully called.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID pointer to get the stat for.

**Triggers**: Social Asynchronous Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*constant*} **"id"**: Will hold the constant `achievement_stat_event`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"event"**: Will hold the string "LocalUserAdded".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **"userid"**: The user ID associated with the request.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"error"**: 0 if successful, some other value if there has been an error.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"errorMessage"**: A string with an error message, if any is available (this key is only present when there is an error)

**Returns**: {*real*} -1 if there was an error, any other value if the function was successfully called.

---

## xboxone_stats_remove_user

**Usage**: xboxone_stats_remove_user(user_id)

**Description**: This function can be used to remove (unregister) a given user from the statistics manager, performing a flush of the stat data to the live server. According to the XBox documentation the game does not have to remove the user from the stats manager, as the XBox OS will periodically flush the stats anyway.

To use the function, you supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), and the function will will return -1 if there was an error or the user ID is invalid, or any other value if the function was successfully called.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID pointer to get the stat for.

**Triggers**: Social Asynchronous Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*constant*} **"id"**: Will hold the constant `achievement_stat_event`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"event"**: Will hold the string "LocalUserRemoved".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **"userid"**: The user ID associated with the request.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"error"**: 0 if successful, some other value if there has been an error.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"errorMessage"**: A string with an error message (only avaiable when there is an error)

**Returns**: {*real*} -1 if there was an error, any other value if the function was successfully called.

**Notes**: 
* Removing the user can return an error if the statistics that have been set on the user are invalid (such as the stat names containing non-alpha numeric characters).
* If you want to flush the stats data to the live server at any time without removing the user, you can use the function [`xboxone_stats_flush_user()`](#xboxone_stats_flush_user).

**Code Sample**:
```gml
	for (var i = 0; i < array_length(user_id); i++) {
		xboxone_stats_remove_user(user_id[i]);
	}
```
---


## xboxone_stats_flush_user

**Usage**: xboxone_stats_flush_user(user_id, high_priority)

**Description**: This function can be used to flush the stats data for a given user from the statistics manager, to the live server, ensuring that the server is up to date with the current values. According to XBox documentation, developers should be careful not to call this too often as the XBox will rate-limit the requests, and the XBox OS will also automatically flush stats approximately every 5 minutes automatically anyway.

To use the function, you supply the user ID as returned by the function [`xboxone_get_user()`](#xboxone_get_user), and then give a priority value (0 for low priority and 1 for high priority). The function will will return -1 if there was an error or the user ID is invalid, or any other value if the function was successfully called.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID of the user to flush the data of.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*boolean*} **high_priority** Whether or not flush is high priority.

**Triggers**: Social Asynchronous Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*constant*} **"id"**: Will hold the constant `achievement_stat_event`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"event"**: Will hold the string "StatisticUpdateComplete".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **"userid"**: The user ID associated with the request.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"error"**: 0 if successful, some other value if there has been an error.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"errorMessage"**: A string with an error message (only avaiable when there is an error)

**Returns**: {*real*} -1 if there was an error, any other value if the function was successfully called.

**Code Sample**:
```gml
	for (var i = 0; i < array_length(user_id); i++) {
		xboxone_stats_flush_user(user_id[i], 0);
	}
```
---

## xboxone_stats_get_leaderboard

**Usage**: xboxone_stats_get_leaderboard(user_id, stat, num_entries, start_rank, start_at_user, ascending)

**Description**: This function can be used to retrieve a global leaderboard of ranks for a given statistic. You supply the user ID (as returned by the function [`xboxone_get_user`](#xboxone_get_user)), the stat string (as defined when you registered it as a "Featured Stat"), and then you specify a number of details about what leaderboard information you want to retrieve. Note that you can only retrieve a global leaderboard for int or real stats, but not for string stats.

IMPORTANT: Stats used in global leaderboards must be registered as "Featured Stats" in the XDP/Windows Dev Center otherwise an error will be returned. If you want local (social) leaderboards, then please see the function xboxone_stats_get_social_leaderboard.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID of the user to get the leaderboard for.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **stat** The stat (as string) to create the global leaderboard from.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **num_entries** The number of entries from the global leaderboard to retrieve.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **start_rank** The rank in the leaderboard to start from (use 0 if "start_at_user" is set to true).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*boolean*} **start_at_user** Set to true to start at the user ID rank, false otherwise (set to false if "start_rank" is anything other than 0).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*boolean*} **ascending** Set to true for ascending order and false for descending order.

**Triggers**: Social Asynchronous Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*constant*} **"id"**: Will hold the constant `achievement_leaderboard_info`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"event"**: Will hold the string "GetLeaderboardComplete".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **"userid"**: The user ID associated with the request.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"error"**: 0 if successful, some other value if there has been an error.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"errorMessage"**: A string with an error message (only avaiable when there is an error)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"displayname"**: The unique ID for the leaderboard as defined on the provider dashboard.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"numentries"**: The number of entries in the leaderboard that you have received.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"PlayerN"**: The name of the player, where "N" is an integer value corresponding to their position within the leaderboard entries list.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **"PlayeridN"**: The unique user id of the player, "N".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"RankN"**: The rank of the player "N" within the leaderboard.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"ScoreN"**: The score of the player "N".

**Returns**: N/A

**Code Sample**:

The following is an extended example of how this function can be used. To start with you'd call it in some event like Room Start or Create:

```gml
	xboxone_stats_get_leaderboard(user_id, "GlobalTime", 20, 1, false, true);
```

The above code would be called to get a list of all global leaderboard positions for the game, and will generate a Social Asynchronous Event call back which we would deal with in the following way:

```gml
	if (async_load[? "id"] == achievement_stat_event) {
		if (async_load[? "event"] == "GetLeaderboardComplete") {
			global.numentries = async_load[? "numentries"];
			for (var i = 0; i < numentries; i++) {
				global.playername[i] = async_load[? "Player" + string(i)];
				global.playerid[i] = async_load[? "Playerid" + string(i)];
				global.playerrank[i] = async_load[? "Rank" + string(i)];
				global.playerscore[i] = async_load[? "Score" + string(i]);
			}
		}
	}
```
The above code checks the returned ds_map in the Social Asynchronous Event and if its "id" matches the constant being checked, it then checks to see if the event has been triggered by returned leaderboard data before looping through the map and storing all the different values in a number of global arrays.

---

## xboxone_stats_get_social_leaderboard

**Usage**: xboxone_stats_get_social_leaderboard(user_id, stat, num_entries, start_rank, start_at_user, ascending, favourites_only)

**Description**: This function can be used to retrieve a social leaderboard of ranks for a given statistic. You supply the user ID (as returned by the function [`xboxone_get_user`](#xboxone_get_user)), the stat string (as defined when you created it using the `xboxone_stats_set_stat_*` functions), and then you specify a number of details about what leaderboard information you want to retrieve. Note that you can only retrieve a social leaderboard for int or real stats, but not for string stats, and that if you flag the "favourites_only" argument as true, then the results will only contain data for those friends that are marked by the user as "favourites".

IMPORTANT: Stats used in social leaderboards do not need to be registered as "Featured Stats" in the XDP/Windows Dev Center.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The user ID of the user to get the leaderboard for.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **stat** The stat (as string) to create the global leaderboard from.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **num_entries** The number of entries from the global leaderboard to retrieve.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **start_rank** The rank in the leaderboard to start from (use 0 if "start_at_user" is set to true).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*boolean*} **start_at_user** Set to true to start at the user ID rank, false otherwise (set to false if "start_rank" is anything other than 0).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*boolean*} **ascending** Set to true for ascending order and false for descending order.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*favourites_only*} **ascending** Set to true to show only friends that are marked as "favourites" or false otherwise.

**Triggers**: Social Asynchronous Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*constant*} **"id"**: Will hold the constant `achievement_leaderboard_info`.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"event"**: Will hold the string "GetLeaderboardComplete".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **"userid"**: The user ID associated with the request.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"error"**: 0 if successful, some other value if there has been an error.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"errorMessage"**: A string with an error message (only avaiable when there is an error)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"displayname"**: The unique ID for the leaderboard as defined on the provider dashboard.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"numentries"**: The number of entries in the leaderboard that you have received.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"PlayerN"**: The name of the player, where "N" is an integer value corresponding to their position within the leaderboard entries list.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **"PlayeridN"**: The unique user id of the player, "N".

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"RankN"**: The rank of the player "N" within the leaderboard.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **"ScoreN"**: The score of the player "N".

**Returns**: N/A

**Code Sample**:

The following is an extended example of how this function can be used. To start with you'd call it in some event like Room Start or Create:

```gml
	xboxone_stats_get_social_leaderboard(user_id, "GlobalTime", 20, 1, false, true);
```

The above code would be called to get a list of all global leaderboard positions for the game, and will generate a Social Asynchronous Event call back which we would deal with in the following way:

```gml
	if (async_load[? "id"] == achievement_stat_event) {
		if (async_load[? "event"] == "GetLeaderboardComplete") {
			global.numentries = async_load[? "numentries"];
			for (var i = 0; i < numentries; i++) {
				global.playername[i] = async_load[? "Player" + string(i)];
				global.playerid[i] = async_load[? "Playerid" + string(i)];
				global.playerrank[i] = async_load[? "Rank" + string(i)];
				global.playerscore[i] = async_load[? "Score" + string(i]);
			}
		}
	}
```
The above code checks the returned ds_map in the Social Asynchronous Event and if its "id" matches the constant being checked, it then checks to see if the event has been triggered by returned leaderboard data before looping through the map and storing all the different values in a number of global arrays.

--- 


## xboxone_achievements_set_progress - 

**Usage**: xboxone_achievements_set_progress(user_id, achievement, progress)


--- 

## xboxone_read_player_leaderboard - xboxone_read_player_leaderboard(ident, player, numitems, friendfilter)
--- 
## xboxone_set_rich_presence - xboxone_set_rich_presence(user_id, is_user_active, rich_presence_string)
--- 
## xboxone_check_privilege - xboxone_check_privilege(user_id, privilege_id, attempt_resolution)

--- 

## gdk_save_group_begin

**Usage**: gdk_save_group_begin(container_name)

**Description**: This function is called when you want to begin the saving out of multiple buffers to multiple files. The "container_name" is a string and will be used as the directory name for where the files will be saved, and should be used as part of the file path when loading the files back into the IDE later (using any of the buffer_load() functions). This function is only for use with the [`gdk_save_buffer()`](#gdk_save_buffer) function and you must also finish the save definition by calling [`gdk_save_group_end()`](#gdk_save_group_end) function otherwise the files will not be saved out.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **container_name** The name of the container (as a string).

**Returns**: N/A

**Code Sample**:
```gml
	gdk_save_group_begin("SaveGame");
	save1 = gdk_save_buffer(buff1, "Player_Save1.sav", 0, 16384);
	save2 = gdk_save_buffer(buff2, "Player_Save2.sav", 0, 16384);
	save3 = gdk_save_buffer(buff3, "Player_Save3.sav", 0, 16384);
	save4 = gdk_save_buffer(buff4, "Player_Save4.sav", 0, 16384);
	gdk_save_group_end();
```
--- 

## gdk_save_buffer

**Usage**: gdk_save_buffer(buffer_idx, filename, offset, size)

**Description**: With this function you can save part of the contents of a buffer to a file, ready to be read back into memory using the [`gdk_save_buffer()`](#gdk_save_buffer) function (or any of the other functions for loading buffers). The "offset" defines the start position within the buffer for saving (in bytes), and the "size" is the size of the buffer area to be saved from that offset onwards (also in bytes).

This function works asynchronously, and so the game will continue running while being saved, and all files saved using this function will be placed in a *"default"* folder. This folder does not need to be included in the filename as it is added automatically by GameMaker. For example the filename path `"Data\Player_Save.sav"` would actually be saved to `"default\Data\Player_Save.sav"`. However, if you then load the file using the function buffer_load_async(), you do not need to supply the "default" part of the path either.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **buffer_idx** The index of the buffer to save.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **filename** The place where to save the buffer to (path + filename + extension).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **offset** The start position within the buffer for saving (in bytes).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **size** The size of the buffer area to be saved from the offset onwards (in bytes).

**Triggers**: Asynchronous Save/Load Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **"id"**: Will hold the unique identifier of the asynchronous request.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"error"**: 0 if successful, some other value if there has been an error (error code).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"status"**: 1 if successful, 0 if failed.

**Returns**: {*real*} -1 if there was an error, any other value if the function was successfully called.

**Code Sample**:
```gml
	saveid = gdk_save_buffer(buff, "Player_Save.sav", 0, 16384);
```
then in the asynchronous Save/Load event we can check if the task was successful or not, like this:
```gml
	if (async_load[? "id"] == saveid) {
    	if (async_load[? "status"]== false) {
        	show_debug_message("Save failed!");
        }
		else {
			show_debug_message("Save succeeded!");
		}
    }
```
--- 

## gdk_save_group_end

**Usage**: gdk_save_group_end()

**Description**: This function finishes the definition of a buffer save group. You must have previously called the function [`gdk_save_group_begin()`](#gdk_save_group_begin) to initiate the group, then call the function [`gdk_save_buffer()`](#gdk_save_buffer) for each file that you wish to save out. Finally you call this function, which will start the saving of the files. ?

<!-- ?? The function will return a unique ID value for the save, which can then be used in the Asynchronous Save / Load event to parse the results from the async_load DS map. ?? -->

**Triggers**: Asynchronous Save/Load Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **"id"**: Will hold the unique identifier of the asynchronous request.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"error"**: 0 if successful, some other value if there has been an error (error code).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"status"**: 1 if successful, 0 if failed.

<!-- **Returns**: {*real*} -1 if there was an error, any other value if the function was successfully called. -->

**Returns**: N/A

**Code Sample**:
```gml
	buffer_async_group_begin("SaveGame");
	save1 = buffer_save_async(buff1, "Player_Save1.sav", 0, 16384);
	save2 = buffer_save_async(buff2, "Player_Save2.sav", 0, 16384);
	save3 = buffer_save_async(buff3, "Player_Save3.sav", 0, 16384);
	save4 = buffer_save_async(buff4, "Player_Save4.sav", 0, 16384);
	buffer_async_group_end();
```
--- 

## xboxone_set_savedata_user

**Usage**: xboxone_set_savedata_user(user_id)

**Description**: This function specifies that future file operations which operate in the save game area (i.e. all file writes, and reads from files that aren't in the bundle area) will be associated with the specified user.This can be called as often as necessary to redirect save data to the appropriate user, or you can use the constant `pointer_null` to save to the generic machine storage area.

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*pointer*} **user_id** The User ID pointer to set for future saving.

**Returns**: N/A

**Code Sample**:
```gml
	if xboxone_get_savedata_user() != user_id[0] {
   		xboxone_set_savedata_user(user_id[0]);
    }
```
--- 

## xboxone_get_savedata_user

**Usage**: xboxone_get_savedata_user()

**Description**: This function returns the user ID pointer (or the constant `pointer_null`) currently associated with file saving. See [`xboxone_set_savedata_user()`](#xboxone_set_savedata_user) for further details.

**Returns**: {*pointer/pointer_null*} The user ID currently being used for save data.

**Code Sample**:
```gml
	if xboxone_get_savedata_user() != user_id[0] {
   		xboxone_set_savedata_user(user_id[0]);
    }
```
---




## gdk_load_buffer

**Usage**: gdk_load_buffer(buffer_idx, filename, offset, size)

**Description**: With this function you can load a file that you have created previously using the [`gdk_save_buffer()`](#gdk_save_buffer) function into a buffer. The "offset" defines the start position within the buffer for loading (in bytes), and the "size" is the size of the buffer area to be loaded from that offset onwards (also in bytes). You can supply a value of -1 for the size argument and the entire buffer will be loaded. Note that the function will load from a "default" folder, which does not need to be included as part of the file path you provide. This folder will be created if it doesn't exist or when you save a file using [`gdk_save_buffer()`](#gdk_save_buffer).

**Params**:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **buffer_idx** The index of the buffer to load.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*string*} **filename** The name of the file to load.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **offset** The offset within the buffer to load to (in bytes).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **size** The size of the buffer area to load (in bytes).

**Triggers**: Asynchronous Save/Load Event

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*integer*} **"id"**: Will hold the unique identifier of the asynchronous request.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"error"**: 0 if successful, some other value if there has been an error (error code).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"status"**: 1 if successful, 0 if failed.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"file_size"**: The total size of the file being loaded (in bytes).

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{*real*} **"load_size"**: The amount of bytes loaded into the buffer.

**Returns**: {*real*} -1 if there was an error, otherwise the id of the asynchronous request.

**Code Sample**:
```gml
	loadid = gdk_load_buffer(buff, "Player_Save.sav", 0, 16384);
```
then in the asynchronous Save/Load event we can check if the task was successful or not, like this:
```gml
	if (async_load[? "id"] == saveid) {
    	if (async_load[? "status"]== false) {
        	show_debug_message("Load failed!");
        }
		else {
			show_debug_message("Load succeeded!");
		}
    }
```
--- 


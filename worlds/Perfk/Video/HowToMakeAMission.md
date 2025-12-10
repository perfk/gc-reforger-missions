# How to Make an Arma Reforger Mission with Global Conflicts

This document contains a guide on how to make a simple mission with Reforger Lobby. While primarily written for the Global Conflicts community, almost everything is applicable to general Reforger Lobby mission making, and can be used by everyone.

If you have any questions about it reach out to us on 

---

## Part I: Project Setup

### Download the Workbench

Arma Reforger does not have an integrated advanced mission editor, so any complex missions are created using the Workbench. Yes, it is also a powerful modding tool, but we will almost exclusively stick to the World Editor, which is not hard to use.

To download it, enable tools in your Steam Library, then download "Arma Reforger Tools".

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_01" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Add Existing Mods to the Workbench

Since we will be utilizing Reforger Lobby and other mods, you will have to add these mods as Workbench projects, so the Workbench can find them.

To do this, start the Workbench Launcher, click "Add Project" in the bottom left and select "Scan for Projects". Browse to and select your Arma Reforger mod folder (by default, that's Documents/MyGames/ArmaReforger/addons).

The Workbench will now add any mods you downloaded as projects. If you want to use any mods downloaded in the future, this step will have to be repeated.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_02" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Set Up the GC Missions Project

This guide will show you how to set up and use the **GC Missions mod/repository** for contributing missions to the Global Conflicts community.

Your missions will be added to the existing GC Missions mod, which is already configured with all necessary dependencies. This guide will walk you through cloning the repository and setting it up in Workbench.

**For non-GC users:** If you're creating missions for a different community, you'll need to create your own mod project instead. Make sure to add "Reforger Lobby" (or if using the TilW Mission Framework, "Reforger Lobby Community") as a dependency. The rest of this guide's mission-making steps will still apply.

Once you have the project set up, open it in Workbench.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_03" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Part II: Mission Setup

### Create a Subworld

In Reforger, a gamemode or mission is created as a subworld of a terrain. A subworld is a world which is loaded on top of its parent world.

To create a subworld, open the World Editor, then search for and load the terrain world you would like to create a mission in.

**Available Terrain Files:**

**Vanilla Terrains:**
- `Eden.ent` - Everon (official vanilla terrain)
- `Arland.ent` - Arland (official vanilla terrain)

**Modded Terrains:**
- [Add terrain name] - `[TerrainFile.ent]` - [Brief description]
- [Add terrain name] - `[TerrainFile.ent]` - [Brief description]
- [Add terrain name] - `[TerrainFile.ent]` - [Brief description]

Afterwards, click the "New World" button in the top left and select "Subscene" to create a subworld.

Save the world. At GC, we use this folder structure: `ModProject/worlds/AuthorName/MissionName/MissionName.ent`

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_04" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Add Essential Entities

In Enfusion, entities are placed within layers for the purpose of organization. A fresh world will only have the "default" layer, which we will use for essential gamemode related entities.

**⚠️ IMPORTANT:** Do NOT rename or move the "default" layer. Renaming or moving this layer will cause issues with your mission. Always keep it as "default".

In the bottom of the World Editor, you will find the Resource Browser. Search for the following prefabs and drag them into the world:
- `PerceptionManager.et`
- `PS_GameMode_Lobby.et`

You also need an AI World, which contains navmeshes. The AI World you use depends on your terrain:

**For vanilla terrains:**
- Everon: `SCR_AIWorld_Eden.et`
- Arland: `SCR_AIWorld_Arland.et`

**For modded terrains:**
Most modded terrains have their own navmesh files. They are typically named following the pattern `SCR_AIWorld_{terrainname}.et`:
- Example: `SCR_AIWorld_Kunar.et` for Kunar terrain
- Example: `SCR_AIWorld_Ruha.et` for Ruha terrain

Search for the terrain name in the Resource Browser to find the appropriate AI World prefab. If no terrain-specific AI World exists, use the generic `SCR_AIWorld.et` prefab.

If you get a long error when dragging in the AI World, just hit enter to ignore it.

**Configuring the AI World:**

**If you used a terrain-specific AI World** (like `SCR_AIWorld_Eden.et`, `SCR_AIWorld_Kunar.et`, etc.), the navmeshes are already properly configured. You can skip this section and just verify it looks correct.

**If you used the generic `SCR_AIWorld.et`** (because no terrain-specific AI World exists), you need to manually configure the navmesh files:

1. In the "Hierarchy" window on the left, select the AIWorld entity you just added
2. Look at the "Object Properties" tab on the right. In the top part, you will find a list of components
3. Select the first NavmeshWorldComponent, and in the bottom part, go to "Navmesh Settings", then "Navmesh Files Config"
4. Click the dots next to "Navmesh File" and search for your world (e.g. Eden or Arland) and select the GM_ variant (without vehicles or LowRes)
5. In the second NavmeshWorldComponent, do the same with the GM_Terrain_vehicles navmesh
6. For the third NavmeshWorldComponent, set it to the lowres navmesh for your terrain

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_05" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Setting Ingame Weather

On the gamemode entity, find the SCR_TimeAndWeatherHandlerComponent, it will allow you to edit ingame time and weather.

Editor time and weather can be adjusted when clicking the Cloud button in the World Editor Toolbar, this does not affect your mission when in game though.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_06" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Part III: Mission Design

### Add Player Group Prefabs

To create slots for your mission, you are going to add player groups to the world.

In the top of the Hierarchy window, right click your subworld, select "Create Layer" and name it something like "Players". Double click the new layer to make it active.

It is possible to create your own custom player groups, but for your first project, it is easier to use existing ones. Reforger Lobby comes with various player groups for the US and USSR factions. If you are using it as a dependency, GC Units also has various ready-to-use group prefabs, both from Vanilla and RHS factions.

Group prefabs are found in a mods `Prefabs/Groups/` folder. Player groups end with `_P.et` - if they don't, they are usually regular AI groups and won't work as player slots.

Choose some player groups and drag them into the world at a suitable location.

**Testing player groups:**
If you want to check if your player groups are working, click the arrow next to the green play button in the top bar, make sure "Play from Camera position" is unchecked, then press the green button itself. This will run the world, hopefully showing you a working Lobby.

**Naming groups:**
Player groups can be given a name in their Object Properties (Custom Name Set attribute).

For group order, you can also add a numerical callsign to groups by adding a PS_GroupCallsignAssigner component to the entity (Add Component button in the bottom of Object Properties if there isn't one already on the group), selecting it in the top and configuring it below.

The callsigns behind the numbers are already set up in the faction manager by default, just put in the numbers unless you need to change them for something entirely different for some reason.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_07" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Add AI Group Prefabs

Similarly, we are also going to add AI groups to the world. These are also found in `Prefabs/Groups/` folders, but do not end with `_P`.

For better organization, it is again recommended to create an "AI" layer first.

**AI Behavior:**
By default, AI will just remain in its position until they perceive a danger such as nearby gunfire, then enter combat and move towards it. This makes sense for some AI groups, but usually most of your AI should be assigned a waypoint so they act in more specific ways.

**Adding Waypoints:**
For that, again create a "Waypoints" layer and drag in waypoint prefabs from `ArmaReforger/Prefabs/AI/Waypoints`. Several types are available, for example Defend waypoints.

After dragging the waypoint into the world, rename it (right click in hierarchy, or top right of object properties) to something like "AIWP_Defend_01" so you can later reference it by name. You can also change parameters, most importantly radius.

Finally, go into your AI groups object properties, add an entry to the Static Waypoints array and enter the waypoint entities name.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_08" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Add Any Objects You'd Like

Fortifications, vehicles, turrets, you name it - they are found in the Prefabs directory. By default, these will not be part of the navmesh, but that's usually no problem.

Freeze zones for TVTs can be added using `E_EditorRestrictionZone`.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_09" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Part IV: End Conditions

There are a few ways in which you can create end conditions, from the TilW Mission Framework, the Scenario Framework (a complex vanilla mission framework which takes more time to properly learn, and is not supported by Reforger Lobby).

But unless you have a good reason not to, I would currently strongly recommend using the **TilW Mission Framework**, which is quite powerful, allowing you to add objectives and define even complex end conditions with ease.

Requiring zero scripting, it supports several objective types, and even inter-mission events like QRF spawns that trigger based on specified conditions.

A tutorial for it can be found in this separate document.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_10" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Part V: Briefing and Markers

### Briefing

Create a "Briefing" layer, then drag some `EditableMissionDescription` prefabs from ReforgerLobby into the world.

In its properties, set Title (e.g. Situation, Mission etc.) and Text Data - click the adjacent three dots to open a small text editor window.

Order will adjust the order in which the entries show up in the list (e.g. Situation=1, Mission=2).

**Faction-specific briefings:**
If the mission is a TVT / COTVT, you will need twice the amount of prefabs, and specify faction keys in the Visible For Factions array. Otherwise, you can just check Show For Any Faction.

Unless you check Empty Faction Visibility, the entry will only show up after you slotted a faction. It is recommended to keep this disabled for the briefing, but add a short "Description" entry (summary, ratio etc.) that will only be visible before slotting.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_11" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Markers

The map marker prefab is `EditableMarker.et` from PSCore, the display settings work in a very similar way to the description prefab.

**Selecting marker icons:**
The marker is selected by specifying an icon imageset, then referencing one of its quads by name. The prefabs default imageset is `icons_mapMarkersUI`.

Opening it will show you over 100 different markers, mostly relating to movement. There are also other imagesets you can choose instead, most notably GroupFlagsOpfor / GroupFlagsBlufor / GroupFlagsIndfor, which have unit symbols.

On the right side of an opened imageset, you can switch to the "Quads" tab, which shows you all the icon names sequentially. Scroll through them until you find the one you want, then copy the name and paste it into the quad name attribute on the prefab.

You can also change size, color or set a hover description. Don't forget to configure faction visibility like you did for the briefing prefabs.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_12" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### AO Limit

The TilW Mission Framework has an easy method for AO limits, more info can be found in its guide under "Other Things".

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_13" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Part VI: Testing

There are multiple ways you can test your mission.

### Play Mode

Clicking the green Play Button in the World Editor's toolbar will run the current world in Singleplayer.

Make sure the "Play from Camera position" in the options (found when clicking the thing to the right of the button) is not checked, because this bypasses the lobby.

There's also an option for Full Screen vs. Viewport.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_14" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Play Mode + PeerTool

In the Play Mode options, you can also enable "Server Localhost + Peertool". This will simulate multiplayer using a listen server (where the server is hosted by a player's game and not dedicated).

You can set up PeerTool under Plugins > Settings > PeerTool.

Set Executable to your main ArmaReforgerSteam.exe installation, then create a Peer Client with `-addonsDir "C:\path\to\mods\folder\,D:\path\to\projects\directory\" -addons ThisMod` as parameter.

If you click the Play Button with "Server Localhost + Peertool" checked, it will launch both a hosting player and a separate window with the client.

This testing method ensures everything works in Multiplayer and is especially helpful with TVTs - but most COOP functionality can already be tested without Peer Tool.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_15" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

### Dedicated Server Tool

While PeerTool is perfectly fine for the vast majority of Multiplayer testing, listen servers do differ from dedicated servers in the way that dedicated servers do not have a client.

While this is usually not necessary for common mission testing, you could also use the Dedicated Server tool to test things in a dedicated environment.

You can find the tool under Plugins, it is configured in a similar way to PeerTool. Notable differences are that you have to download the Arma Reforger Server application from Steam and set a DedicatedServerPluginCLI_Server config in the tool that points to your world.

The server can be started using the button in the bottom right, so not via play mode.

**Other noteworthy info:**
If a client fails to join the server, that is often due to a configuration issue, not necessarily something with the mission. If you're not sure which it is, try running BIs MpTest.ent world - if it doesn't work there either, it's indeed a configuration issue.

General script debugging can be done via printing to the Workbench console (found in the bottom of the main window) or via breakpoints.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_16" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Part VII: Header Creation

For your mission/world to show up as a scenario ingame and actually be playable, you have to add a mission header aka. scenario config for that world.

For that, create a Missions folder in your mod project (for GC, create a subfolder with your name in the existing Missions folder). Right click into the folder, and create a Config file, select SCR_MissionHeader as the class, and set its name to your missions name.

Open the newly created config and edit the following parameters:

**World**
- Select the subworld you created for your mission

**Name**
- Mission name including type and player count (min, max)
- e.g. COOP (5-13) Gull Bay Guns

**Author**
- Your name

**Description**
- Short mission description

**Player Count**
- Maximum player count (don't worry about spectator slots, this is overridden by the server config anyway)

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_17" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Part VIII: Publishing

Your mission is now complete, and you are ready to publish.

If you are creating missions for the GC Community, you would create a GitHub PR.

If you are making missions for another community and want to publish it yourself, here is a detailed guide on how to do so. As said earlier, you should avoid publishing missions as individual mods and ideally instead have just one mod that contains all your missions.

**Video Tutorial:**

<iframe width="100%" height="400" src="https://www.youtube.com/embed/VIDEO_ID_18" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## Other Common Questions

### How do you make a mission with multiple objectives?

Depends on the framework - if you are using the TilW Mission Framework, look into its document.

If you're not using a framework for some reason and need it scripted manually, it's usually not very straightforward and may involve a setup where there are multiple independent triggers that can be completed, and then one central script that checks if all of them are completed.

---

### How can I change player group callsigns?

Other than group names, these can not be explicitly set on a group basis, as they are handled by the faction manager.

Go into the groups PS_GroupCallsignAssigner component (add one if there isn't one), in there you can set company, platoon and squad number, which will be translated to a callsign by the faction manager.

If required, you can also edit the factions Callsign Info in the faction manager (a child of the Reforger Lobby gamemode) to change callsign names.

---

### How can I make AI enter a vehicle?

The TilW Mission Framework has a component that will spawn AI in a pre-placed vehicle, also allowing you to select specific seat types like only the gun.

AI driving is done by assigning a regular waypoint (e.g. move) to a group riding in a vehicle.

---

## Additional Resources

Here is a general list of useful resources:

- [Modding – Arma Reforger - Bohemia Interactive Community](https://community.bistudio.com/wiki/Arma_Reforger:Modding)
- [World Editor – Arma Reforger - Bohemia Interactive Community](https://community.bistudio.com/wiki/Arma_Reforger:World_Editor)
- [Script Editor – Arma Reforger - Bohemia Interactive Community](https://community.bistudio.com/wiki/Arma_Reforger:Script_Editor)
- [Scripting Tutorials – Arma Reforger - Bohemia Interactive Community](https://community.bistudio.com/wiki/Arma_Reforger:Scripting_Tutorials)
- [Scripting Guidelines – Arma Reforger - Bohemia Interactive Community](https://community.bistudio.com/wiki/Arma_Reforger:Scripting_Guidelines)
- [From SQF to Enforce Script – Arma Reforger - Bohemia Interactive Community](https://community.bistudio.com/wiki/Arma_Reforger:SQF_to_Enforce_Script)
- TilW Mission Framework
- Official RL Manual (not as detailed as this guide, but still helpful)
- Reforger Modding FAQ (mostly about general modding and terrain creation, but also has a couple of things that may help with mission making)
- #enfusion-mission-makers and #enfusion-scripting channels on ARMA Discord
- You can open existing mods / missions and check how they work

If you have any issues or questions, DM me (Til) on Discord - I'm happy to help.

---

*Last updated: 2025-12-09*

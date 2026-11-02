# Co-op campaign: bMultiPlayer audit

Every remaining use of `bMultiPlayer` in the working tree (line numbers drift as code changes).

Dispositions:
- **keep**: really means a multiplayer-ruleset game; stays as is
- **campaign**: should be `isCampaignGame()` / `!isCampaignGame()`, so co-op behaves like the single-player campaign
- **networked**: should be `isNetworkedGame()`, about connection/sync and true in co-op
- **coop**: needs a co-op specific rule (`isCoopGame()`)
- **n/a**: debug, test, replay or frontend code not reachable in a co-op campaign
- **TODO**: not yet looked at

| File:line | Code | Disposition | Note |
|---|---|---|---|
| lib/gamelib/gtime.cpp:390 | `if (NetPlay.bComms && bMultiPlayer)` | keep | log line |
| lib/gamelib/gtime.cpp:405 | `if (NetPlay.bComms && bMultiPlayer)` | keep | log line |
| src/action.cpp:1871 | `bool ObjDepartedAlly = (bMultiPlayer && NetPlay.players[structureAtPos->player].difficulty == AIDifficulty::HUMAN && !NetPlay.players[structureAtPos->player].allocated);` | campaign | departed-ally rule; slot based, wrong in co-op |
| src/action.cpp:2447 | `if (bMultiPlayer` | campaign | EW against transporters is a multiplayer rule |
| src/ai.cpp:688 | `if (bMultiPlayer)` | campaign | EW against transporters is a multiplayer rule |
| src/ai.cpp:1217 | `if (bMultiPlayer && psDroid->isVtol() && isHumanPlayer(psDroid->player))` | campaign | idle human VTOLs are a multiplayer rule; isHumanPlayer is slot based |
| src/campaigninfo.cpp:52 | `if (!bMultiPlayer && !bInTutorial)` | campaign | campaign name is tracked in co-op too |
| src/clparse.cpp:1148 | `bMultiPlayer = false;` | n/a | command line start-up |
| src/clparse.cpp:1257 | `bMultiPlayer = true;` | n/a | command line start-up |
| src/clparse.cpp:1308 | `bMultiPlayer = true;` | n/a | command line start-up |
| src/combat.cpp:487 | `bMultiMessages = bMultiPlayer;` | networked | scripts must be able to send messages |
| src/configuration.cpp:1084 | `if (!bMultiPlayer)` | keep | favourite colour is a single player setting |
| src/configuration.cpp:1092 | `if (bMultiPlayer && NetPlay.bComms)` | keep | hosting game name |
| src/configuration.cpp:1258 | `if (bMultiPlayer && !NetPlay.bComms)` | keep | hosting game name |
| src/configuration.cpp:1273 | `if (bMultiPlayer && NetPlay.bComms)` | keep | hosting game name |
| src/data.cpp:94 | `if (!bMultiPlayer)` | keep | data hash is computed for networked sessions |
| src/design.cpp:633 | `ASSERT_OR_RETURN(false, !(bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator), "Spectators can't open design mode");` | keep | spectator check |
| src/design.cpp:829 | `if (bMultiPlayer && !widgAddButton(psWScreen, &sButInit))` | keep | design store is a multiplayer feature; co-op shares designs through sendTemplateUpdate instead |
| src/design.cpp:3045 | `if (bMultiPlayer)` | keep | design store is a multiplayer feature; co-op shares designs through sendTemplateUpdate instead |
| src/design.cpp:3096 | `if (bMultiPlayer)` | keep | design store is a multiplayer feature; co-op shares designs through sendTemplateUpdate instead |
| src/design.cpp:3383 | `if (bMultiPlayer && wasStored)` | keep | design store is a multiplayer feature; co-op shares designs through sendTemplateUpdate instead |
| src/design.cpp:3822 | `if (bMultiPlayer)` | keep | design store is a multiplayer feature; co-op shares designs through sendTemplateUpdate instead |
| src/design.cpp:3828 | `if (bMultiPlayer)` | keep | design store is a multiplayer feature; co-op shares designs through sendTemplateUpdate instead |
| src/design.cpp:3882 | `if (bMultiPlayer)` | keep | design store is a multiplayer feature; co-op shares designs through sendTemplateUpdate instead |
| src/design.cpp:3962 | `if (!bMultiPlayer && !bInTutorial)` | keep | the design screen pauses the game only in single player; a networked game cannot pause |
| src/design.cpp:3977 | `if (!bMultiPlayer && !bInTutorial)` | keep | the design screen pauses the game only in single player; a networked game cannot pause |
| src/display.cpp:880 | `if (!bMultiPlayer && establishSelection(selectedPlayer) == SC_DROID_SUPERTRANSPORTER)` | campaign | never let the user control the supertransporter in the campaign (breaks scripts) |
| src/display.cpp:968 | `bool ObjDepartedAlly = (ObjUnderMouse && bMultiPlayer && NetPlay.players[ObjUnderMouse->player].difficulty == AIDifficulty::HUMAN && !NetPlay.players[ObjUnderMouse->player].allocated);` | campaign | departed-ally cursor: in co-op a slot index is not the game player of the object |
| src/display.cpp:1018 | `else if (bMultiPlayer && item == MT_TRANDROID)` | campaign | multiplayer ruleset |
| src/display.cpp:1120 | `arnMPointers[item][selection] == CURSOR_MOVE && bMultiPlayer)` | campaign | alt+move disembark is multiplayer only |
| src/display.cpp:1154 | `if (bMultiPlayer && bLasSatStruct)` | campaign | Las-Sat targeting is multiplayer only |
| src/display.cpp:1946 | `const bool isSpectator = bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator;` | keep | spectator check |
| src/display.cpp:1974 | `if (bMultiPlayer && bLasSatStruct)` | campaign | Las-Sat targeting is multiplayer only |
| src/display.cpp:2002 | `if (bMultiPlayer && !bEffectiveRightClickOrders)` | campaign | RMB interface is the multiplayer input scheme; co-op plays like the campaign |
| src/display.cpp:2116 | `const bool isSpectator = bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator;` | keep | spectator check |
| src/display.cpp:2136 | `if (bMultiPlayer && bLasSatStruct)` | campaign | Las-Sat targeting is multiplayer only |
| src/display.cpp:2242 | `if (bMultiPlayer && bLasSatStruct)` | campaign | Las-Sat targeting is multiplayer only |
| src/display.cpp:2554 | `if (bMultiPlayer)` | campaign | RMB opens the transporter interface in multiplayer; the campaign opens it elsewhere |
| src/display.cpp:2573 | `else if (bMultiPlayer && isHumanPlayer(psDroid->player))` | campaign | names other humans droids; isHumanPlayer is slot based, so it would match enemy factions in co-op |
| src/display3d.cpp:1856 | `\|\| (bMultiPlayer && psStats->weaponSubClass == WSC_LAS_SAT);` | campaign | Las-Sat effect is multiplayer only |
| src/display3d.cpp:1913 | `(bMultiPlayer && psStats->weaponSubClass == WSC_LAS_SAT))` | campaign | Las-Sat effect is multiplayer only |
| src/display3d.cpp:3449 | `const bool isSpectator = bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator;` | keep | spectator check |
| src/display3d.cpp:3697 | `const bool isSpectator = bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator;` | keep | spectator check |
| src/display3d.cpp:3999 | `if (!psDroid->isTransporter() \|\| bMultiPlayer)` | campaign | transporters cannot be box-selected in the campaign |
| src/droid.cpp:309 | `if ((psDroid->isVtol() \|\| (psDroid->isTransporter() && bMultiPlayer)) && (psDroid->sMove.Status == MOVEINACTIVE))` | campaign | multiplayer ruleset |
| src/droid.cpp:392 | `if (bMultiPlayer && !bMultiMessages)` | networked | multimessages are re-enabled so the destroy message goes out |
| src/droid.cpp:1017 | `if (bMultiPlayer)` | campaign | droid shields are a multiplayer ruleset |
| src/droid.cpp:1889 | `ASSERT(!bMultiPlayer \|\| worldOnMap(world.map, pos.x, pos.y), "the build locations are not on the map");` | campaign | droids may finish building while on an away mission |
| src/droid.cpp:2638 | `if (!bMultiPlayer)` | campaign | campaign experience tweak option |
| src/droid.cpp:3689 | `if (bMultiPlayer` | campaign | unit limits are a multiplayer ruleset |
| src/droid.cpp:3703 | `if (electronic \|\| !bMultiPlayer)` | campaign | campaign gifts recreate the droid |
| src/droid.cpp:3711 | `if (psD->player == selectedPlayer && to != selectedPlayer && !bMultiPlayer)` | campaign | campaign score tracking |
| src/droid.cpp:3963 | `if (psDroid->isTransporter() && !bMultiPlayer)` | campaign | never control the transporter in the campaign |
| src/frontend.cpp:260 | `bMultiPlayer = lastSaveMP;` | keep | sets up or resets the session (frontend) |
| src/frontend.cpp:420 | `bMultiPlayer = false;` | keep | sets up or resets the session (frontend) |
| src/frontend.cpp:471 | `bMultiPlayer = true;` | keep | sets up or resets the session (frontend) |
| src/frontend.cpp:477 | `bMultiPlayer = true;` | keep | sets up or resets the session (frontend) |
| src/frontend.cpp:568 | `bMultiPlayer = true;` | keep | sets up or resets the session (frontend) |
| src/frontend.cpp:576 | `bMultiPlayer = false;` | keep | sets up or resets the session (frontend) |
| src/frontend.cpp:602 | `bMultiPlayer = false;` | keep | sets up or resets the session (frontend) |
| src/frontend.cpp:625 | `bMultiPlayer = true;` | keep | sets up or resets the session (frontend) |
| src/frontend.cpp:651 | `bMultiPlayer = true;` | keep | sets up or resets the session (frontend) |
| src/game.cpp:2415 | `bMultiPlayer = true;` | n/a | replay load, replays are not supported for co-op |
| src/game.cpp:2821 | `bMultiPlayer = saveGameData.multiPlayer;` | keep | restores the flag from the save |
| src/game.cpp:2895 | `if (bMultiPlayer && alliancesSharedVision(game.alliance) && alliances[i][j] == ALLIANCE_FORMED)` | keep | alliancesSharedVision is false in a campaign; game.type is not loaded yet at this point so isCampaignGame() would be wrong |
| src/game.cpp:2947 | `bMultiMessages	= bMultiPlayer;` | keep | follows the saved flag |
| src/game.cpp:2953 | `if (bMultiPlayer)` | keep | multiplayer stats for the lobby players; harmless in co-op |
| src/game.cpp:3006 | `if (bMultiPlayer && alliancesSharedVision(game.alliance))` | keep | teams from shared-vision alliances, which a campaign never has |
| src/game.cpp:3039 | `if (bMultiPlayer)` | coop | legacy load: multiplayer clears every template, the campaign only player 0; co-op takes the multiplayer branch. Revisit if designs misbehave across levels |
| src/game.cpp:4723 | `bMultiPlayer	= saveGameData.multiPlayer;` | keep | restores the flag from the save |
| src/game.cpp:4724 | `bMultiMessages	= bMultiPlayer;` | keep | follows the saved flag |
| src/game.cpp:4729 | `if (bMultiPlayer)` | keep | multiplayer stats for the lobby players; harmless in co-op |
| src/game.cpp:4853 | `bMultiPlayer = save.value("multiplayer").toBool();` | keep | restores the flag from the save |
| src/game.cpp:5099 | `save.setValue("multiplayer", bMultiPlayer);` | keep | writes the flag to the save |
| src/game.cpp:5266 | `saveGame.multiPlayer = bMultiPlayer;` | keep | writes the flag to the save |
| src/gamehistorylogger.cpp:365 | `if (!bMultiPlayer)` | campaign | history log is skipped for the campaign, co-op included |
| src/gamehistorylogger.cpp:427 | `if (!bMultiPlayer)` | campaign | history log is skipped for the campaign, co-op included |
| src/gamehistorylogger.cpp:453 | `if (!bMultiPlayer)` | campaign | history log is skipped for the campaign, co-op included |
| src/gamehistorylogger.cpp:488 | `if (!bMultiPlayer)` | campaign | history log is skipped for the campaign, co-op included |
| src/gamestate_savegame.cpp:34 | `#include "multiplay.h"      // game, ingame, bMultiPlayer, MULTIPLAYERGAME, MULTISTRUCTLIMITS` | n/a | comment |
| src/gamestate_savegame.cpp:131 | `if (bMultiPlayer && pl.ai >= 0 && static_cast<size_t>(pl.ai) < ais.size())` | keep | saves AI scripts of multiplayer slots; co-op has no AI slots |
| src/gamestate_savegame.cpp:186 | `if (header.aiScripts.empty() \|\| !bMultiPlayer)` | keep | campaign has no AI slots |
| src/gamestate_savegame.cpp:256 | `j["multiplayer"] = bMultiPlayer;` | keep | session identity written to the save |
| src/gamestate_savegame.cpp:381 | `bMultiPlayer = j.at("multiplayer").get<bool>();` | keep | session identity read from the save |
| src/gamestate_savegame.cpp:568 | `bMultiPlayer = true;` | n/a | unit test |
| src/gamestate_savegame.cpp:644 | `bMultiPlayer = false;` | n/a | unit test |
| src/gamestate_savegame.cpp:689 | `check(bMultiPlayer == true, "multiplayer not restored");` | n/a | unit test |
| src/gamestate_savegame.cpp:1920 | `if (!bMultiPlayer \|\| localControllerSlot() >= MAX_PLAYERS)` | keep | only a multiplayer session has stats to restore |
| src/gamestate_serialize.cpp:5017 | `if (bMultiPlayer)` | campaign | the away-mission transporter mode exists in the campaign, co-op included |
| src/hci.cpp:974 | `if (!bMultiPlayer)` | keep | intelmap pauses the game only in single player; a co-op game can never pause |
| src/hci.cpp:1248 | `bool selectedPlayerIsSpectator = (bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator) \|\| (selectedPlayer >= MAX_PLAYERS);` | keep | spectators do not exist in co-op (slot 0 is never one) |
| src/hci.cpp:1598 | `if ((testPlayerHasLost() \|\| testPlayerHasWon()) && !bMultiPlayer && intMode != INT_MISSIONRES && !dbgInputManager.debugMappingsAllowed())` | campaign | campaign loss/win ends the level for each peer; mission-failed flow still untested |
| src/hci.cpp:1742 | `if (!bMultiPlayer)` | keep | tutorial-only console handling |
| src/hci.cpp:1789 | `if (!bMultiPlayer \|\| !NetPlay.bComms)` | keep | equals !isNetworkedGame(): a networked game cannot pause |
| src/hci.cpp:1810 | `if (!bMultiPlayer \|\| !NetPlay.bComms)` | keep | equals !isNetworkedGame(): a networked game cannot pause |
| src/hci.cpp:1915 | `bool isSpectator = (bMultiPlayer && selectedPlayer < NetPlay.players.size() && NetPlay.players[selectedPlayer].isSpectator);` | keep | spectator check, none in co-op |
| src/hci.cpp:2410 | `if (bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator) { return false; }` | keep | spectator check, none in co-op |
| src/hci.cpp:2417 | `if (bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator) { return false; }` | keep | spectator check, none in co-op |
| src/hci.cpp:2424 | `if (bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator) { return false; }` | keep | spectator check, none in co-op |
| src/hci.cpp:2431 | `if (bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator) { return false; }` | keep | spectator check, none in co-op |
| src/hci.cpp:2583 | `if (bMultiPlayer && selectedPlayer < NetPlay.players.size() && NetPlay.players[selectedPlayer].isSpectator)` | keep | spectator check, none in co-op |
| src/hci.cpp:2594 | `if (bMultiPlayer && selectedPlayer < NetPlay.players.size() && NetPlay.players[selectedPlayer].isSpectator)` | keep | spectator check, none in co-op |
| src/hci/research.cpp:286 | `if (bMultiPlayer)` | campaign | ally research icons and progress bars are a multiplayer interface |
| src/hci/research.cpp:330 | `if (bMultiPlayer)` | campaign | ally research icons and progress bars are a multiplayer interface |
| src/hci/research.cpp:524 | `if (bMultiPlayer)` | campaign | ally research icons and progress bars are a multiplayer interface |
| src/hci/research.cpp:563 | `if (bMultiPlayer)` | campaign | ally research icons and progress bars are a multiplayer interface |
| src/ingameop.cpp:193 | `if (!((bMultiPlayer && NetPlay.bComms) \|\| bInTutorial \|\| NETisReplay()))` | networked | save and load are hidden in a networked game |
| src/ingameop.cpp:195 | `if (bMultiPlayer)` | keep | skirmish branch of the non-networked case |
| src/ingameop.cpp:226 | `if (NetPlay.isHost && bMultiPlayer && NetPlay.bComms)` | networked | host quit |
| src/ingameop.cpp:269 | `if (bMultiPlayer && (NetPlay.bComms \|\| NETisReplay()))` | networked | warning text |
| src/init.cpp:1818 | `if (bMultiPlayer)` | networked | session setup and shutdown |
| src/init.cpp:1895 | `bMultiMessages = bMultiPlayer;` | keep | session setup and shutdown |
| src/init.cpp:1949 | `if (bMultiPlayer)` | networked | session setup and shutdown |
| src/init.cpp:2065 | `if (bMultiPlayer && !coopLevelChangeInProgress())` | networked | session setup and shutdown |
| src/input/debugmappings.cpp:49 | `const bool bIsTrueMultiplayerGame = bMultiPlayer && NetPlay.bComms;` | networked | debug mappings are shared in a networked game |
| src/input/gamepadbindings.cpp:137 | `if (bMultiPlayer)` | keep | multiplayer menu |
| src/intelmap.cpp:184 | `#define PAUSE_DISPLAY_CONDITION (!bMultiPlayer)` | keep | the intelligence screen pauses the game only in single player; a networked game cannot pause |
| src/intelmap.cpp:469 | `if (bMultiPlayer && multiMenuForm)` | keep | multiplayer menu form on the intelligence screen; consistent with the multiplayer menu in co-op |
| src/intelmap.cpp:591 | `if (bMultiPlayer && !multiMenuForm)` | keep | multiplayer menu form on the intelligence screen; consistent with the multiplayer menu in co-op |
| src/intelmap.cpp:785 | `if (bMultiPlayer && strongSelf->multiMenuForm)` | keep | multiplayer menu form on the intelligence screen; consistent with the multiplayer menu in co-op |
| src/intelmap.cpp:1234 | `if (!bMultiPlayer)` | keep | pauses only in single player; a networked game cannot pause |
| src/intelmap.cpp:1254 | `if (!bMultiPlayer)` | keep | pauses only in single player; a networked game cannot pause |
| src/keybind.cpp:127 | `return bMultiPlayer && NetPlay.bComms;` | networked | this is isNetworkedGame() |
| src/keybind.cpp:1295 | `if (bMultiPlayer && (intMode != INT_INTELMAP))` | keep | the multiplayer menu lists the players; useful in co-op |
| src/keybind.cpp:1925 | `if (!bMultiPlayer)	// can't do it in multiplay` | keep | a networked game cannot pause |
| src/keybind.cpp:2609 | `if (!bMultiPlayer)` | keep | beacons are wanted in co-op, and they go over the network |
| src/keybind.cpp:2729 | `const char *filename = bMultiPlayer ? QUICKSAVE_SKI_FILENAME : QUICKSAVE_CAM_FILENAME;` | coop | quicksave file names: co-op uses the skirmish ones today; should use the campaign ones. Quicksave in co-op still untested |
| src/keybind.cpp:2730 | `const char *quickSaveFolder = bMultiPlayer ? QUICKSAVE_SKI_FOLDER : QUICKSAVE_CAM_FOLDER;` | coop | quicksave file names: co-op uses the skirmish ones today; should use the campaign ones. Quicksave in co-op still untested |
| src/keybind.cpp:2764 | `const char *filename = bMultiPlayer ? QUICKSAVE_SKI_FILENAME : QUICKSAVE_CAM_FILENAME;` | coop | quicksave file names: co-op uses the skirmish ones today; should use the campaign ones. Quicksave in co-op still untested |
| src/keybind.cpp:2766 | `if (PHYSFS_exists(filename) \|\| PHYSFS_exists(bMultiPlayer ? QUICKSAVE_SKI_JSON_FILENAME : QUICKSAVE_CAM_JSON_FILENAME))` | coop | quicksave file names: co-op uses the skirmish ones today; should use the campaign ones. Quicksave in co-op still untested |
| src/levels.cpp:1352 | `if (bMultiPlayer && game.type != LEVEL_TYPE::CAMPAIGN) // co-op campaigns use the campaign's own messages and scripts` | keep | already excludes the co-op campaign |
| src/levels.cpp:1422 | `if (bMultiPlayer && NetPlay.bComms)` | networked | ignores --autogame in a networked game |
| src/loadsave.cpp:255 | `if (!bMultiPlayer \|\| !NetPlay.bComms)` | networked | a networked game cannot pause |
| src/loadsave.cpp:526 | `if (!bMultiPlayer \|\| !NetPlay.bComms)` | networked | a networked game cannot pause |
| src/loadsave.cpp:685 | `if (!bMultiPlayer)` | coop | save file naming for the campaign; the co-op saves seen so far are named correctly, so check |
| src/loadsave.cpp:1160 | `const char *dir = bMultiPlayer ? SAVEGAME_SKI_AUTO : SAVEGAME_CAM_AUTO;` | coop | autosave folder: co-op autosaves would go to the skirmish folder; should be the campaign one. Autosave in co-op is untested |
| src/loadsave.cpp:1165 | `freeAutoSaveSlot(bMultiPlayer ? SAVEGAME_LOC_SKI_AUTO : SAVEGAME_LOC_CAM_AUTO);` | coop | autosave folder: co-op autosaves would go to the skirmish folder; should be the campaign one. Autosave in co-op is untested |
| src/loop.cpp:202 | `if (bMultiPlayer && !NetPlay.isHostAlive && NetPlay.bComms && !NetPlay.isHost)` | networked | host quit handling |
| src/loop.cpp:249 | `if (bMultiPlayer)` | keep | handles the network messages |
| src/loop.cpp:319 | `else if (bMultiPlayer \|\| saveMidMission())` | keep | saves from the menu |
| src/loop.cpp:755 | `bool selectedPlayerIsSpectator = bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator;` | keep | spectator check |
| src/loop.cpp:756 | `bool multiplayerHostDisconnected = bMultiPlayer && !NetPlay.isHostAlive && NetPlay.bComms && !NetPlay.isHost; // do not fast-forward after the host has disconnected` | networked | host disconnected |
| src/main.cpp:846 | `if (bMultiPlayer)` | keep | failed level load shuts the session down |
| src/main.cpp:898 | `if (!bMultiPlayer && getCamTweakOption_AutosavesOnly())` | coop | forced autosave of the campaign; autosave in co-op is untested |
| src/main.cpp:1026 | `debug(LOG_INFO, "Loading saved game \"%s\" (multiplayer: %d, comms: %d, host: %d, controller slot %u)", saveGameName, (int)bMultiPlayer, (int)NetPlay.bComms, (int)NetPlay.isHost, realSelectedPlayer);` | keep | log line |
| src/main.cpp:1033 | `debug(LOG_INFO, "Cold load finished (multiplayer: %d, comms: %d, controller slot %u, shared player %u, gameTime %u)", (int)bMultiPlayer, (int)NetPlay.bComms, realSelectedPlayer, selectedPlayer, gameTime);` | keep | log line |
| src/main.cpp:1241 | `if (bMultiPlayer)` | n/a | test helper |
| src/mission.cpp:517 | `if (bMultiPlayer)` | networked | co-op quit from the result screen ends the network session |
| src/mission.cpp:2572 | `const bool multiplayerHostQuit = bMultiPlayer && NetPlay.bComms && !NetPlay.isHost && !NetPlay.isHostAlive;` | networked | host gone: a client cannot continue |
| src/mission.cpp:2639 | `if (bMultiPlayer)` | coop | co-op shows the multiplayer stats tabs on the result screen (works today); revisit with the mission-failed flow |
| src/mission.cpp:2690 | `bool multiplayerHostQuit = bMultiPlayer && NetPlay.bComms && !NetPlay.isHost && !NetPlay.isHostAlive;` | networked | host gone: a client cannot continue |
| src/mission.cpp:2693 | `if (result \|\| bMultiPlayer)` | coop | co-op takes the multiplayer branch (Continue and Quit always shown). A lost mission should only offer Quit; pending the mission-failed flow |
| src/mission.cpp:2696 | `if ((!testPlayerHasWon() \|\| bMultiPlayer) && !multiplayerHostQuit)` | coop | co-op takes the multiplayer branch (Continue and Quit always shown). A lost mission should only offer Quit; pending the mission-failed flow |
| src/mission.cpp:2707 | `if (bMultiPlayer \|\| (testPlayerHasWon() && !bMultiPlayer))` | coop | co-op takes the multiplayer branch (Continue and Quit always shown). A lost mission should only offer Quit; pending the mission-failed flow |
| src/mission.cpp:2847 | `if (bMultiPlayer)` | keep | only reached outside co-op (co-op uses coopLaunchNextLevel) |
| src/mission.cpp:3202 | `if (!bMultiPlayer)` | keep | a networked game cannot pause; co-op shares this |
| src/mission.cpp:3215 | `if (!bMultiPlayer)` | keep | a networked game cannot pause; co-op shares this |
| src/move.cpp:949 | `if (!isHumanPlayer(psDroid->player) && bMultiPlayer)` | campaign | AI frustration is multiplayer only; isHumanPlayer is slot based so it would match enemy factions in co-op |
| src/move.cpp:961 | `if ((bMultiPlayer \|\| psDroid->player == selectedPlayer \|\| psDroid->lastFrustratedTime == gameTime)` | campaign | campaign parking rule |
| src/move.cpp:1721 | `if (!bMultiPlayer && getCamTweakOption_heavilyDamagedPenalty() && objectBelowHealthLevel(psDroid, HEAVY_DAMAGE_LEVEL))` | campaign | campaign tweak option |
| src/move.cpp:2093 | `if (psDroid->isTransporter() && !bMultiPlayer)` | campaign | campaign transporter height |
| src/move.cpp:2406 | `if (!bMultiPlayer && !bInTutorial)` | campaign | Beta and Gamma oil drum bonus |
| src/multibot.cpp:335 | `bool debugAllowed = dbgInputManager.debugMappingsAllowed() \|\| !bMultiPlayer;` | networked | debug droid adding must be allowed by cheat mode in any networked game |
| src/multibot.cpp:728 | `return !bMultiPlayer` | keep | attack validation, alliances only matter in multiplayer |
| src/multibot.cpp:1038 | `if (!dbgInputManager.debugMappingsAllowed() && bMultiPlayer)` | networked | debug droid removal is refused in any networked game |
| src/multigifts.cpp:97 | `if (bMultiPlayer && (to < NetPlay.players.size()))` | keep | spectator check |
| src/multigifts.cpp:154 | `if (bMultiPlayer && to == selectedPlayer)` | keep | audio alert; harmless in co-op |
| src/multigifts.cpp:267 | `if (bMultiPlayer && psStats->curCount[to] >= psStats->upgrade[to].limit)` | campaign | structure limits are a multiplayer ruleset |
| src/multigifts.cpp:569 | `if (bMultiPlayer && alliances[p1][p2] != ALLIANCE_FORMED && allowNotification)` | campaign | campaign scripts form alliances silently |
| src/multigifts.cpp:579 | `if (bMultiPlayer && alliancesSharedVision(game.alliance))	// this is for shared vision only` | campaign | shared vision is a multiplayer alliance setting |
| src/multigifts.cpp:591 | `if (bMultiPlayer && alliancesSharedVision(game.alliance))` | campaign | shared vision is a multiplayer alliance setting |
| src/multigifts.cpp:676 | `if (!bMultiPlayer)` | keep | spectator check |
| src/multigifts.cpp:831 | `if (!dbgInputManager.debugMappingsAllowed() && bMultiPlayer)` | keep | debug-only feature adding, rejected in any networked game |
| src/multigifts.cpp:853 | `if (fromPlayer < MAX_PLAYERS && bMultiPlayer)` | coop | multiplayer artifact pickup (removes the artifact, leaks research). The campaign leaves it to the script (eventPickup). Co-op takes the multiplayer branch today and the artifact grab works in playtests; revisit together with the script research grant |
| src/multiint.cpp:635 | `if (bMultiPlayer && game.type == LEVEL_TYPE::SKIRMISH)` | keep | skirmish AI loading |
| src/multiint.cpp:2440 | `const bool bIsTrueMultiplayerGame = bMultiPlayer && NetPlay.bComms;` | networked | this is isNetworkedGame() |
| src/multiint.cpp:2900 | `if (NetPlay.isHost && bMultiPlayer && NetPlay.bComms)` | networked | this is isNetworkedGame() |
| src/multiint.cpp:6183 | `bMultiPlayer = true;` | keep | sets up the multiplayer session |
| src/multiint.cpp:6223 | `bMultiPlayer = true;` | keep | sets up the multiplayer session |
| src/multiint.cpp:6281 | `bMultiPlayer = true;` | keep | sets up the multiplayer session |
| src/multiint.cpp:6920 | `bMultiPlayer = true;` | keep | sets up the multiplayer session |
| src/multiopt.cpp:625 | `bMultiPlayer = true;` | keep | session setup, teardown or player checks |
| src/multiopt.cpp:690 | `if (bMultiPlayer && NetPlay.bComms)` | keep | session setup, teardown or player checks |
| src/multiopt.cpp:859 | `bMultiPlayer					= false;	// Back to single player mode` | keep | session setup, teardown or player checks |
| src/multiplay.cpp:107 | `bool						bMultiPlayer				= false;	// true when more than 1 player.` | keep | the definitions |
| src/multiplay.cpp:108 | `bool						bMultiMessages				= false;	// == bMultiPlayer unless multimessages are disabled` | keep | the definitions |
| src/multiplay.cpp:137 | `if (!bMultiPlayer \|\| !NetPlay.bComms \|\| !NetPlay.isHost)` | networked | equals isNetworkedGame(): kick routines apply to co-op too |
| src/multiplay.cpp:258 | `if (!bMultiPlayer \|\| !NetPlay.bComms \|\| !NetPlay.isHost)` | networked | equals isNetworkedGame(): kick routines apply to co-op too |
| src/multiplay.cpp:354 | `if (!bMultiPlayer \|\| !NetPlay.bComms \|\| !NetPlay.isHost)` | networked | equals isNetworkedGame(): kick routines apply to co-op too |
| src/multiplay.cpp:437 | `if (!bMultiPlayer)` | keep | multimessages switch |
| src/multiplay.cpp:935 | `return !bMultiPlayer \|\| game.type == LEVEL_TYPE::CAMPAIGN;` | keep | the definitions |
| src/multiplay.cpp:940 | `return bMultiPlayer && NetPlay.bComms;` | keep | the definitions |
| src/multiplay.cpp:1917 | `if (!dbgInputManager.debugMappingsAllowed() && bMultiPlayer && !isCoopScriptGrant)` | keep | multiplayer rejects non-debug research messages; co-op script grants are exempted explicitly |
| src/multiplay.cpp:2123 | `if (!researchAvailable(index, player, ModeImmediate) && bMultiPlayer)` | networked | message validation, applies to co-op |
| src/multiplay.cpp:2211 | `if (!bMultiPlayer \|\| !NetPlay.bComms)` | networked | quick-chat mode needs a network game |
| src/multiplay.cpp:2542 | `if (!dbgInputManager.debugMappingsAllowed() && bMultiPlayer)` | keep | debug-only feature removal message: rejected in any networked game |
| src/multistat.cpp:811 | `if (bMultiPlayer)` | keep | local statistics, never saved for a campaign |
| src/multistruct.cpp:98 | `bool debugAllowed = dbgInputManager.debugMappingsAllowed() \|\| !bMultiPlayer;` | networked | debug structure adding must be allowed by cheat mode in any networked game |
| src/multistruct.cpp:191 | `if (!dbgInputManager.debugMappingsAllowed() && bMultiPlayer)` | networked | debug structure removal is refused in any networked game |
| src/multistruct.cpp:595 | `\|\| !(validTemplateForFactory(pT, psStruct, false) \|\| player == scavengerPlayer() \|\| !bMultiPlayer))` | campaign | campaign production is not restricted to the factory templates, as in single player |
| src/order.cpp:2692 | `// can't use bMultiPlayer since multimsg could be off.` | n/a | comment |
| src/order.cpp:2896 | `bool ObjDepartedAlly = (bMultiPlayer && NetPlay.players[psObj->player].difficulty == AIDifficulty::HUMAN && !NetPlay.players[psObj->player].allocated);` | campaign | departed-ally rule; slot based, wrong in co-op |
| src/ordersource.cpp:250 | `const bool bIsTrueMultiplayerGame = bMultiPlayer && NetPlay.bComms;` | networked | this is isNetworkedGame() |
| src/projectile.cpp:365 | `if (bMultiPlayer && psDamage && psDamage->psDest && psDamage->psDest->player == scavengerPlayer())` | campaign | scavenger experience rule is multiplayer only |
| src/projectile.cpp:387 | `if (psObj->psDest != nullptr && psObj->psDest->type == OBJ_DROID && bMultiPlayer)` | campaign | quality factor is multiplayer only, so as not to unbalance the campaign |
| src/projectile.cpp:673 | `else if (!(bMultiPlayer && psStats->weaponSubClass == WSC_LAS_SAT))` | campaign | Las-Sat sound is multiplayer only |
| src/projectile.cpp:856 | `if (bMultiPlayer && psStats->weaponSubClass == WSC_LAS_SAT &&` | campaign | Las-Sat delay is multiplayer only |
| src/projectile.cpp:1179 | `if (bMultiPlayer && psObj->psSource != nullptr && psCurr->type != OBJ_FEATURE)` | keep | multiplayer statistics, local only |
| src/projectile.cpp:1374 | `if (bMultiPlayer && psObj->psSource)` | keep | multiplayer statistics, local only |
| src/projectile.cpp:1836 | `if (bMultiPlayer)` | keep | multiplayer statistics, local only |
| src/qtscript.cpp:637 | `if (binding == wzapi::ScriptBinding::PlayerAI && bMultiPlayer && NetPlay.bComms` | campaign | a campaign AI script must load for a player whose slot belongs to a human |
| src/qtscript.cpp:1510 | `if (bMultiPlayer && NetPlay.bComms)` | networked | autogame scripts need cheat mode in a networked game |
| src/radar.cpp:572 | `\|\| (bMultiPlayer && alliancesSharedVision(game.alliance)` | campaign | shared vision is a multiplayer alliance setting |
| src/radar.cpp:635 | `\|\| (bMultiPlayer && alliancesSharedVision(game.alliance)` | campaign | shared vision is a multiplayer alliance setting |
| src/researchtree/researchtreelayout.cpp:175 | `if (viewer >= MAX_PLAYERS \|\| (bMultiPlayer && NetPlay.players[viewer].isSpectator))` | keep | spectator check |
| src/researchtree/researchtreelayout.cpp:183 | `if (!bMultiPlayer \|\| game.alliance == NO_ALLIANCES \|\| !aiCheckAlliances(viewer, target))` | campaign | no team view in a campaign |
| src/researchtree/researchtreelayout.cpp:259 | `const bool watching = viewer >= MAX_PLAYERS \|\| (bMultiPlayer && NetPlay.players[viewer].isSpectator);` | keep | spectator check |
| src/researchtree/researchtreelayout.cpp:260 | `const bool shared = bMultiPlayer && alliancesSharedResearch(game.alliance);` | campaign | shared research is a multiplayer alliance setting |
| src/researchtree/researchtreelayout.cpp:261 | `const bool teamsAreFixed = bMultiPlayer && game.alliance != NO_ALLIANCES && alliancesFixed(game.alliance);` | campaign | fixed teams are a multiplayer alliance setting |
| src/screens/chatscreen.cpp:52 | `return bMultiPlayer && selectedPlayer < MAX_CONNECTED_PLAYERS && !ingame.hostChatPermissions[selectedPlayer];` | coop | permissions belong to the connection slot, not the shared game player |
| src/screens/chatscreen.cpp:332 | `bool skipSendingMessage = (bMultiPlayer && (selectedPlayer >= MAX_CONNECTED_PLAYERS \|\| !ingame.hostChatPermissions[selectedPlayer])) \|\| isLocalQuickChatOnlyMode();` | coop | permissions belong to the connection slot, not the shared game player |
| src/screens/chatscreen.cpp:589 | `bool quickChatEnabled = (bMultiPlayer && selectedPlayer < MAX_PLAYERS && !NetPlay.players[selectedPlayer].isSpectator);` | coop | quick chat for co-op; the slot is the one that is or is not a spectator |
| src/screens/researchtreescreen.cpp:447 | `\|\| (bMultiPlayer && selectedPlayer < NetPlay.players.size() && NetPlay.players[selectedPlayer].isSpectator);` | keep | spectator check |
| src/structure.cpp:1212 | `\|\| psStruct->player == scavengerPlayer() \|\| !bMultiPlayer,` | campaign | campaign templates are not required to be researched |
| src/structure.cpp:1773 | `if (bMultiPlayer && pStructureType->psWeapStat[0]->weaponSubClass == WSC_LAS_SAT)` | campaign | Las-Sat reload is a multiplayer ruleset |
| src/structure.cpp:1790 | `if (bMultiPlayer && pStructureType->psWeapStat[0]->weaponSubClass == WSC_LAS_SAT)` | campaign | Las-Sat reload is a multiplayer ruleset |
| src/structure.cpp:2885 | `if (bMultiPlayer)` | networked | desync avoidance: the commander group is not synchronised |
| src/structure.cpp:3133 | `if (bMultiPlayer && psStructure->resistance < (int)structureResistance(psStructure->pStructureType, psStructure->player))` | campaign | electronic warfare resistance is a multiplayer ruleset |
| src/structure.cpp:3523 | `if (bMultiPlayer && psStructure->resistance < (int)structureResistance(psStructure->pStructureType, psStructure->player))` | campaign | electronic warfare resistance is a multiplayer ruleset |
| src/structure.cpp:3631 | `if (bMultiPlayer && psStructure->resistance < (int)structureResistance(psStructure->pStructureType, psStructure->player))` | campaign | electronic warfare resistance is a multiplayer ruleset |
| src/structure.cpp:4096 | `if (bMultiPlayer)` | campaign | electronic warfare resistance is a multiplayer ruleset |
| src/structure.cpp:4130 | `if (bMultiPlayer && ONEINTEN && !bMission && pDisplayModel && pDisplayModel->points.size() >= 2)` | campaign | EW flashing effect, multiplayer ruleset |
| src/structure.cpp:4463 | `if (!bMultiPlayer && !dbgInputManager.debugMappingsAllowed() && !TEST_TILE_VISIBLE(player, mapTile(world.map, b.map.x + i, b.map.y + j)))` | campaign | campaign rule; the debug exemption is local, so it must not apply when peers have to agree |
| src/structure.cpp:4540 | `if (packThis == PACKABILITY_NORMAL && bMultiPlayer && game.type == LEVEL_TYPE::SKIRMISH && !isHumanPlayer(player))` | keep | skirmish AI only (also checks game.type == SKIRMISH) |
| src/structure.cpp:4996 | `if (bMultiPlayer && !isCampaignGame())` | campaign | already isCampaignGame()-gated |
| src/structure.cpp:5839 | `if (!bMultiPlayer && isTransporter(psTemplate))` | campaign | transporters cannot be built in the campaign |
| src/structure.cpp:5960 | `if (bMultiPlayer)` | campaign | assert for the multiplayer ruleset |
| src/structure.cpp:6030 | `if (bMultiPlayer)` | campaign | EW target resistance, multiplayer ruleset |
| src/structure.cpp:6089 | `if (!bMultiPlayer)` | campaign | the campaign gives no EW rewards |
| src/structure.cpp:7077 | `if (bMultiPlayer)` | campaign | structure takeover, multiplayer ruleset |
| src/structure.cpp:7208 | `if (!bMultiPlayer)` | campaign | campaign branch of the takeover |
| src/structure.cpp:7248 | `if (bMultiPlayer)` | campaign | EW resistance, multiplayer ruleset |
| src/structure.cpp:7281 | `if ((psStruct->selected \|\| (bMultiPlayer && !isHumanPlayer(psStruct->player)))` | campaign | Las-Sat auto-fire for AI players, multiplayer ruleset |
| src/template.cpp:287 | `if (!bMultiPlayer \|\| !isHumanPlayer(player))` | campaign | multiplayer design restrictions do not apply to the campaign |
| src/template.cpp:1046 | `\|\| (bMultiPlayer && !playerBuiltHQ && (psCurr->droidType != DROID_CONSTRUCT && psCurr->droidType != DROID_CYBORG_CONSTRUCT))` | campaign | needing an HQ to build most units is a multiplayer rule |
| src/template.cpp:1059 | `else if (bMultiPlayer && (iCapacity == SIZE_HEAVY))` | campaign | super heavy bodies are for the multiplayer super transporter |
| src/titleui/widgets/lobbyplayerrow.cpp:702 | `bool trueMultiplayerMode = (bMultiPlayer && NetPlay.bComms) \|\| (!NetPlay.isHost && ingame.localJoiningInProgress);` | networked | lobby row layout |
| src/titleui/widgets/optionsform.cpp:854 | `const bool bIsTrueMultiplayerGame = bMultiPlayer && NetPlay.bComms;` | networked | this is isNetworkedGame() |
| src/transporter.cpp:199 | `if (bMultiPlayer && psCurrTransporter && isDead((BASE_OBJECT *)psCurrTransporter))` | keep | closes the interface of a dead transporter; harmless in the campaign |
| src/visibility.cpp:698 | `return viewer == ally \|\| (bMultiPlayer && alliancesSharedVision(game.alliance) && aiCheckAlliances(viewer, ally));` | campaign | shared vision is a multiplayer alliance setting |
| src/wrappers.cpp:310 | `bMultiPlayer = true;` | keep | sets up a session (frontend) |
| src/wrappers.cpp:346 | `bMultiPlayer = true;` | keep | sets up a session (frontend) |
| src/wrappers.cpp:463 | `if (bMultiPlayer)` | keep | end time and blind-mode identity reveal of a networked game |
| src/wrappers.cpp:497 | `if (bMultiPlayer)` | campaign | no profile stats for a campaign |
| src/wrappers.cpp:505 | `if (bMultiPlayer && isFirstCallForThisGame) // make sure we only accumulate one loss (even if this is called more than once, for example when losing initially, and then when the game fully ends)` | campaign | no profile stats for a campaign |
| src/wrappers.cpp:511 | `if (bMultiPlayer && isFirstCallForThisGame && !NETisReplay() && selectedPlayer < MAX_CONNECTED_PLAYERS)` | campaign | no profile stats for a campaign (it would write the shared player slot into a helper profile) |
| src/wrappers.cpp:522 | `if (bMultiPlayer && NetPlay.players[selectedPlayer].isSpectator)` | keep | spectator check |
| src/wzapi.cpp:372 | `return !bMultiPlayer \|\| !isGlobalScript(context) \|\| myResponsibility(player);` | keep | co-op is handled by the branch above |
| src/wzapi.cpp:390 | `if (!bMultiPlayer \|\| isGlobalScript(context))` | keep | global campaign scripts run on every peer, so applying directly is right |
| src/wzapi.cpp:399 | `formAlliance(player1, player2, false, bMultiPlayer, true);` | campaign | audio of a new alliance is a multiplayer effect |
| src/wzapi.cpp:442 | `SCRIPT_ASSERT({}, context, !bMultiPlayer \|\| !isGlobalScript(context), "sendAllianceRequest() cannot be used by global scripts in multiplayer");` | keep | campaign scripts do not request alliances |
| src/wzapi.cpp:447 | `if (bMultiPlayer)` | keep | campaign scripts do not request alliances |
| src/wzapi.cpp:895 | `bMultiPlayer = false;` | n/a | multiplayer rules setup only |
| src/wzapi.cpp:906 | `bMultiPlayer = true;` | n/a | multiplayer rules setup only |
| src/wzscriptdebug.cpp:287 | `result["bMultiPlayer"] = bMultiPlayer;` | n/a | debug output |

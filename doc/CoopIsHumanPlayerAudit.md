# Co-op campaign: isHumanPlayer audit

`isHumanPlayer(p)` asks whether connection slot `p` holds a human. In a co-op campaign the helper sits in slot 1, so `isHumanPlayer(1)` is true although game player 1 is an enemy faction. Uses are classified as:

- **keep**: the number is a connection slot (lobby, chat, ping, readiness, votes); correct as is
- **game player**: the number is a faction; converted to `isHumanGamePlayer()`, which in co-op is true only for the shared player
- **open**: a slot/faction mix-up that needs splitting, not fixed yet

| File:line | Code | Disposition | Note |
|---|---|---|---|
| lib/gamelib/gtime.cpp:506 | `if (!mySlotResponsibility(player) \|\| !isHumanPlayer(player))` | keep | game time per connection slot |
| lib/netplay/netplay.cpp:2122 | `ASSERT_OR_RETURN(nullopt, isHumanPlayer(playerIdx) && !NetPlay.players[playerIdx].isSpectator, "playerIdx is not a currently-connected human player: %" PRIu32 "", playerIdx);` | keep | spectator slot moves |
| lib/netplay/netplay.cpp:2181 | `ASSERT_OR_RETURN(SpectatorToPlayerMoveResult::FAILED, isHumanPlayer(playerIdx) && NetPlay.players[playerIdx].isSpectator, "playerIdx is not a currently-connected spectator: %" PRIu32 "", playerIdx);` | keep | spectator slot moves |
| src/ai.cpp:721 | `else if ((isHumanGamePlayer(psDroid->player) && (psStruct->pStructureType->type != REF_WALL && psStruct->pStructureType->type != REF_WALLCORNER))` | game player | wall targeting of AI factions |
| src/ai.cpp:722 | `\|\| !isHumanGamePlayer(psDroid->player))` | game player | wall targeting of AI factions |
| src/ai.cpp:1217 | `if (!isCampaignGame() && psDroid->isVtol() && isHumanGamePlayer(psDroid->player))` | game player | idle human VTOLs (also multiplayer only) |
| src/astar.cpp:1199 | `if (!isHumanGamePlayer(type.owner) && type.moveType == FMT_MOVE)` | game player | danger map for AI factions; must be the same on every peer |
| src/chat.cpp:113 | `if (isHumanPlayer(receiver))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/chat.cpp:135 | `if (!isHumanPlayer(responsiblePlayer))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/chat.cpp:152 | `if (!isHumanPlayer(receiver))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/chat.cpp:183 | `if (isHumanPlayer(receiver) && NetPlay.players[receiver].isSpectator && receiver != localControllerSlot())` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/display.cpp:2573 | `else if (!isCampaignGame() && isHumanGamePlayer(psDroid->player))` | game player | names the droid of another human faction |
| src/fpath.cpp:711 | `if (!isHumanGamePlayer(psDroid->player) && moveType == FMT_MOVE)` | game player | AI factions blast through obstacles; must be the same on every peer |
| src/hci/chatoptions.cpp:338 | `if (!isHumanPlayer(playerIdx))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:416 | `bool isHuman = isHumanPlayer(playerIdx.value());` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:458 | `if (!isHumanPlayer(playerIdx.value()))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:497 | `bool isHuman = isHumanPlayer(playerIdx);` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:617 | `if (!isHumanPlayer(playerIdx))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:777 | `bool isHuman = isHumanPlayer(playerIdx);` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:794 | `if (!isHumanPlayer(playerIdx))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:857 | `if (!isHumanPlayer(playerIdx.value()))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:878 | `bool isHuman = isHumanPlayer(playerIdx.value());` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:976 | `if (!isHumanPlayer(i))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:1145 | `if (isHumanPlayer(player)) // is an active (connected) human` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/chatoptions.cpp:1159 | `if (isHumanPlayer(player) // is an active (connected) human` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/quickchat.cpp:229 | `if (isHumanPlayer(player) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/quickchat.cpp:917 | `if (isHumanPlayer(playerIdx) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/quickchat.cpp:3277 | `bool firstIsOrWasHuman = isHumanPlayer(playerTeammates.front()) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/quickchat.cpp:3280 | `bool thisIsOrWasHuman = isHumanPlayer(playerIdx) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/teamstrategy.cpp:166 | `if (isHumanPlayer(player))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/teamstrategy.cpp:973 | `if (isHumanPlayer(player) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/hci/teamstrategy.cpp:1176 | `if (isHumanPlayer(player) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/mission.cpp:2821 | `if (isHumanPlayer(i))` | keep | co-op handshake over connection slots |
| src/mission.cpp:2867 | `coopAckPending[i] = i != NetPlay.hostPlayer && isHumanPlayer(i);` | keep | co-op handshake over connection slots |
| src/mission.cpp:2959 | `if (coopAckPending[i] && !isHumanPlayer(i))` | keep | co-op handshake over connection slots |
| src/move.cpp:949 | `if (!isHumanGamePlayer(psDroid->player) && !isCampaignGame())` | game player | AI frustration of a faction |
| src/move.cpp:2434 | `if ((!isHumanGamePlayer(psDroid->player) && psDroid->order.type != DORDER_RECOVER) \|\| psDroid->isVtol() \|\| psDroid->isTransporter())  // VTOLs or transporters can't pick up features!` | game player | feature pickup: enemy factions in co-op must not pick up oil drums or artifacts |
| src/multibot.cpp:373 | `player, isHumanGamePlayer(player) ? "Human" : "AI");` | game player | log text: a faction |
| src/multibot.cpp:876 | `queue.index, info.droidId, isHumanGamePlayer(info.player) ? "Human" : "AI", info.player);` | game player | log text: a faction |
| src/multiint.cpp:1722 | `return player == otherPlayer \|\| isHostOrAdmin() \|\| !isHumanPlayer(otherPlayer);` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:2283 | `&& (isHostOrAdmin() \|\| !isHumanPlayer(i)) // host/admin can move a player to any slot, player can only move to empty slots` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:2455 | `bool canChangeSpectatorStatus = !locked.spectators && NetPlay.bComms && isHumanPlayer(player) && (player != NetPlay.hostPlayer) && (NetPlay.isHost \|\| player == selectedPlayer);` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:2893 | `if (!isHumanPlayer(i)) { continue; }` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:2915 | `if (!isHumanPlayer(i)) { continue; }` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:3289 | `if (!isHumanPlayer(i) \|\| isPlayerHostOrAdmin(responsibleIdx))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:3295 | `if (NetPlay.players[player].ready && isHumanPlayer(player) && ingame.JoiningInProgress[player])` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:3299 | `if (NetPlay.players[i].ready && isHumanPlayer(i) && ingame.JoiningInProgress[i])` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:3322 | `if (NetPlay.players[player].ready && isHumanPlayer(player) && ingame.JoiningInProgress[player])` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:5422 | `if (isHumanPlayer(i))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:5897 | `if (!isHumanPlayer(i) && !(game.mapHasScavengers && NetPlay.players[i].position == scavengerSlot()))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:6496 | `if (!isHumanPlayer(player))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:6516 | `if (!isHumanPlayer(player))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:8241 | `ASSERT_OR_RETURN(, isHumanPlayer(receiver), "Invalid receiver: %" PRIu32 "", receiver);` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multiint.cpp:8970 | `if (isHumanPlayer(i) && ingame.JoiningInProgress[i])` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/multijoin.cpp:321 | `&& isHumanPlayer(i)											// ... is a human` | keep | team members that stay (connection slots) |
| src/multimenu.cpp:158 | `else if (isHumanPlayer(player))		// Human player, no alliance` | keep | player list rows are connection slots; the alliance column mixes in the faction (selectedPlayer), revisit when the multiplayer menu is used in co-op |
| src/multimenu.cpp:849 | `if (isHumanPlayer(player) \|\| (game.type == LEVEL_TYPE::SKIRMISH && player < game.maxPlayers && NetPlay.players[player].difficulty != AIDifficulty::DISABLED))` | keep | player list rows are connection slots; the alliance column mixes in the faction (selectedPlayer), revisit when the multiplayer menu is used in co-op |
| src/multimenu.cpp:1060 | `const bool isHuman = isHumanPlayer(playerWidget.player);` | keep | player list rows are connection slots; the alliance column mixes in the faction (selectedPlayer), revisit when the multiplayer menu is used in co-op |
| src/multiopt.cpp:881 | `if (isHumanPlayer(i))` | keep | chat permissions per connection slot |
| src/multiopt.cpp:933 | `if (!isHumanPlayer(i))` | keep | chat permissions per connection slot |
| src/multiplay.cpp:163 | `if (!isHumanPlayer(i) \|\| !NetPlay.players[i].heartbeat)` | keep | lag kick loop over connection slots |
| src/multiplay.cpp:284 | `if (!isHumanPlayer(i) \|\| !NetPlay.players[i].heartbeat)` | keep | desync kick loop over connection slots |
| src/multiplay.cpp:387 | `if (!isHumanPlayer(i))` | keep | lobby not-ready kick over connection slots |
| src/multiplay.cpp:544 | `if (isHumanPlayer(i) && ingame.JoiningInProgress[i])` | keep | joining counter over connection slots |
| src/multiplay.cpp:604 | `if (ingame.DataIntegrity[index] == false && isHumanPlayer(index) && index != NetPlay.hostPlayer)` | keep | data integrity check over connection slots |
| src/multiplay.cpp:889 | `return isHumanPlayer(player);` | keep | isHumanGamePlayer(): outside co-op the two are the same |
| src/multiplay.cpp:902 | `if (isHumanPlayer(player))` | open | whosResponsible and myResponsibility are used both with connection slots (message routing, quick chat recipients) and with factions (who sends orders for an enemy faction). Treating the host as responsible for everything broke routing to the helper, so the original stays; in co-op a helper in slot 1 is still counted responsible for enemy faction 1. Split into a slot function and a faction function |
| src/multiplay.cpp:1126 | `if (player == NetPlay.hostPlayer \|\| !isHumanPlayer(player) \|\| NetPlay.players[player].isSpectator \|\| !NetPlay.players[player].heartbeat)` | keep | data check wait over connection slots |
| src/multiplay.cpp:1264 | `if (!isHumanPlayer(player) \|\| !NetPlay.players[player].heartbeat)` | keep | message sender is a connection slot |
| src/multiplay.cpp:2202 | `if (isHumanPlayer(playerIdx))` | keep | mute: identity of a connection slot |
| src/multiplay.cpp:3066 | `if (isHumanPlayer(i) && ingame.JoiningInProgress[i])` | keep | ready status of a connection slot |
| src/multistruct.cpp:267 | `if (isHumanGamePlayer(player) && gameTime - psStruct->asWeaps[0].lastFired <= firePause)` | game player | fire pause applies to the human faction |
| src/multisync.cpp:111 | `if (isHumanPlayer(i) && (i != realSelectedPlayer) && (ingame.PingTimes[i] < PING_LIMIT))` | keep | ping over connection slots |
| src/multisync.cpp:266 | `if (isHumanPlayer(i)` | keep | ping over connection slots |
| src/multisync.cpp:273 | `else if (!isHumanPlayer(i)` | keep | ping over connection slots |
| src/multisync.cpp:287 | `if (isHumanPlayer(i)` | keep | ping over connection slots |
| src/multivote.cpp:774 | `if (isHumanPlayer(i))` | keep | votes per connection slot |
| src/multivote.cpp:1150 | `if (isHumanPlayer(player) // is an active (connected) human player` | keep | votes per connection slot |
| src/order.cpp:1456 | `if (!isHumanGamePlayer(psDroid->player))` | game player | invalid orders to an AI faction are only logged |
| src/researchtree/researchtreelayout.cpp:214 | `if (player >= game.maxPlayers \|\| (!isHumanPlayer(player) && NetPlay.players[player].ai < 0))` | keep | seats of the research tree viewer; no seats in a campaign |
| src/screens/netpregamescreen.cpp:130 | `if (isHumanPlayer(player))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/screens/netpregamescreen.cpp:317 | `if (isHumanPlayer(targetPlayer) && targetPlayer != NetPlay.hostPlayer && strongParent->status == LoadingStatus::Loading)` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/screens/netpregamescreen.cpp:322 | `if (isHumanPlayer(i))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/screens/netpregamescreen.cpp:470 | `if (isHumanPlayer(player) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/screens/netpregamescreen.cpp:499 | `if (isHumanPlayer(player) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/screens/netpregamescreen.cpp:1032 | `if (isHumanPlayer(i))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/spectatorwidgets.cpp:892 | `if (isHumanPlayer(player) // is an active (connected) human player` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/stdinreader.cpp:501 | `if (!isHumanPlayer(i))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/stdinreader.cpp:1039 | `if (!isHumanPlayer(i))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/structure.cpp:1692 | `player, isHumanGamePlayer(player) ? "Human" : "AI", getStatsName(pStructureType), map.x, map.y,` | game player | log text: a faction |
| src/structure.cpp:4540 | `if (packThis == PACKABILITY_NORMAL && bMultiPlayer && game.type == LEVEL_TYPE::SKIRMISH && !isHumanPlayer(player))` | keep | skirmish AI only |
| src/structure.cpp:7281 | `if ((psStruct->selected \|\| (!isCampaignGame() && !isHumanGamePlayer(psStruct->player)))` | game player | Las-Sat auto-fire for a faction |
| src/template.cpp:287 | `if (isCampaignGame() \|\| !isHumanGamePlayer(player))` | game player | designs of AI factions are not checked |
| src/titleui/widgets/lobbyplayerrow.cpp:157 | `if (!NetPlay.players[j].fileSendInProgress() && (isHumanPlayer(j) \|\| NetPlay.players[j].difficulty != AIDifficulty::DISABLED) && !NetPlay.players[j].isSpectator)` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/titleui/widgets/lobbyplayerrow.cpp:632 | `if (NetPlay.players[i].ai >= 0 && i != player && !isHumanPlayer(i) && !isSpectatorOnlySlot(i))` | keep | connection slot (lobby, chat, ping, readiness): keep |
| src/wzapi.cpp:146 | `return NetPlay.isHost && myResponsibility(m_player) && !isHumanGamePlayer(m_player) && (NetPlay.players[m_player].ai >= 0 \|\| m_player == scavengerPlayer());` | game player | is the script the host AI of a faction |

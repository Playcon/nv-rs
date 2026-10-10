# Dead Money coverage matrix

Every quest (`QUST`) in `DeadMoney.esm`, what it needs from the engine, and how far it has been
played in the nv-rs viewer. Written from the plugin's records with nvinspect and from what the
earlier Dead Money work recorded (`docs/DEAD_MONEY.md` on the Dead Money branches); no engine
changes. Names and behaviour are described in my own words; no game text is quoted, and nothing
decompiled is included.

**57 quests**: 4 PLAYED, 19 PARTIAL, 33 NOT PLAYED, 1 NEVER STARTED.
**57 quests**: 4 PLAYED, 21 PARTIAL, 31 NOT PLAYED, 1 NEVER STARTED.
**57 quests**: 4 PLAYED, 20 PARTIAL, 32 NOT PLAYED, 1 NEVER STARTED.
**57 quests**: 4 PLAYED, 26 PARTIAL, 26 NOT PLAYED, 1 NEVER STARTED.
**57 quests**: 4 PLAYED, 22 PARTIAL, 30 NOT PLAYED, 1 NEVER STARTED.
**57 quests**: 4 PLAYED, 20 PARTIAL, 32 NOT PLAYED, 1 NEVER STARTED.
**57 quests**: 4 PLAYED, 22 PARTIAL, 30 NOT PLAYED, 1 NEVER STARTED.

## How to read it

* **PLAYED**: reached its end in the viewer, with its stages moved by the game's own scripts,
  triggers and dialogue (a test character may set the start).
* **PARTIAL**: some of it was played; the note says how far and what was helped along with a console
  line (`--run "SetStage ..."`).
* **NOT PLAYED**: not tried; the note says what blocks it.
* **NEVER STARTED**: the data has the quest but nothing starts it, so the original never plays it
  either (leftover content); the note says how that was found and what to check.
* **[G]** marks a guess (nothing in the data or the code settles it). **[C]** marks something to check
  against the original game. Both are things for the maintainer to confirm.
* "Needs" is the systems the quest's scripts and objectives point at. It comes from reading the quest
  record and its scripts' function names; it is not a trace of every call, so a quest may use a system
  not listed.
* Stage counts are the numbers in the quest record (`INDX`). Where an Elijah bark quest lists many
  numbered lines, they are spoken lines in the record's objective list, not journal objectives.
* Evidence for the PLAYED and PARTIAL rows: the maintainer's bunker launch test (narrator voice,
  slides, arrival in the Villa, the hologram talking), and the headless and viewer runs recorded in
  `docs/DEAD_MONEY.md` ("D3", "Playing Act 1", "Act 2 groundwork"). Those runs used `--run` console
  lines for what the player's hands would do, which is why most of Act 1 is PARTIAL and not PLAYED.
* All 199 script functions Dead Money calls are carried out (nvinspect), so none of the NOT PLAYED
  rows is blocked on a missing function; the blockers are places not visited, features not built
  (radio station playback, casino games, the hacking game, the Cloud's look) or untested behaviour.

## Matrix

### Story quests

The quests with stages and objectives the player sees.

| Quest (editor ID) | Stages | Needs | Status | Notes |
| --- | --- | --- | --- | --- |
| Sierra Madre Grand Opening! (`NVDLC01MQ00`) | 10, 100 | radio, trigger volume, gas and knock-out, fade, slideshow, inventory removal, door lock | **PLAYED** | From stage 10, which the test character gives (the 30 second wait after Radio New Vegas comes on is not played). The bunker trigger sets stage 100 itself. Confirmed by the maintainer's bunker launch: the narrator spoke, the slides changed, the Villa was reached. |
| MQ01 Master Quest (`NVDLC01MQ01`) | 5, 100 | companions, radio stations, holograms, toxic damage, gear | **PARTIAL** | Both stages were reached and its end messages ran, but the three recruits that finish it were helped along with console stage lines in the runs recorded. Its start-up work (enabling the radio stations, the hologram vendors, the holorifle) ran without errors but wasn't looked at one by one. |
| Find Collar 8: Dog (`NVDLC01MQ01a`) | 10, 20, 30, 40, 100, 255 | dialogue, Pip-Boy note (voice tape), basement trigger, companion recruit | **PARTIAL** | Played: Dog's greeting moves the quest to the basement stage; picking up the tape moves it on. Not played: the tape in the Pip-Boy (the code path is unit-tested only), the recruit stage (set by a console line). Dog's cage key and the later Dog/God switch are in other quests. |
| Find Collar 14: Dean Domino (`NVDLC01MQ01b`) | 10, 20, 100, 255 | dense world district, sitting in a chair (explosive), dialogue, companion recruit, ghost-people ambush enabled afterwards | **PARTIAL** | Played: the district loads (after the graphics fix), the player sits in his chair and his real greeting plays. The recruit stage was set by a console line in the recorded runs. [C] Whether the original needs anything between the greeting and the recruit isn't known here. |
| Find Collar 12: Christine (`NVDLC01MQ01c`) | 10, 20, 30, 40, 100, 255 | dialogue (no voice, stage directions), companion recruit, clinic basement terminal and power, Auto-Doc wing, shielded speakers | **PARTIAL** | Played: her silent conversation, stage 100, the crew notice, her following the player. Later checked (see DEAD_MONEY.md on the stack, 'The clinic basement and the Auto-Doc wing'): the basement terminal's power-off item and the Auto-Doc scene run correctly (rules level, and the scene in the viewer). Still not played by hand: the terminal screen in the viewer and the walk between the two; the scene stops at Christine standing in the pod because a package's End action (her 'start conversation') isn't dispatched yet. [C] Whether the normal route needs the basement before the conversation can be had isn't checked. |
| Return to the Fountain (`NVDLC01MQ01d`) | 10, 100 | dialogue | **PARTIAL** | Started by the master quest when all three are recruited and finished by Elijah's Act 2 greeting, which worked; but the recruits before it were helped along (see MQ01a to c). |
| Trigger the Gala Event (`NVDLC01MQ02`) | 10, 50, 100, 255 | companions in place, climbing (ladder, bell tower), control panel, elevator, timed door, traps, radio, autosave, teleport | **PARTIAL** | Played from `characters/dead-money-gala.txt`: stage 50's script (XP, autosave, the post-Gala ghost people, the companions moved to their places, the gauntlet's enables) and the event's 33 second fireworks timer, run by script lines. Not played: the climb to the bell tower (ladders and stairs are untried [G]), the panel's own talk (E on a talking activator with no greeting opens nothing: [C] what the original does), the ghost gauntlet, the entry to the casino. |
| Fires in the Sky (`NVDLC01MQ02a`) | 5, 10, 30, 40, 100, 255 | companion escort and wait, terminal, items (harvester remains), Villa ghost people | **PARTIAL** | Played: Elijah's Act 2 greeting starts it; telling Dog to wait (he stays, after the teammate-wait fix) and to follow again; walking him into the Gala trigger (it completes its objective, shows the next, and Dog walks up and starts his talk, after three engine fixes: see DEAD_MONEY.md 'Act 2'); the first replies. Run as script lines, not by hand: one ending reply (stage 100, XP) and the substation terminal's Lock Gate item (stage 100). Not played: picking the replies through to an ending, the feeding path (two slabs of remains, stage 40), the terminal screen, Dog walking to his Gala position. [G] The lines a topic names through its INFC list are said like its own. [C] How the original ends this conversation by hand. |
| Strike Up the Band (`NVDLC01MQ02b`) | 10, 30, 40, 100, 255 | companion escort and wait, security holograms, switches, rooftop, items | **PARTIAL** | Played from `characters/dead-money-act2-dean.txt`: the rooftop trigger, Dean's talk and replies (`--say`), stage 30, both holograms switched on (the terminals' items as script lines; the holograms killed a ghost), stage 40, and the ending's result lines to completion. Not played: the last replies by hand (a second conversation needs his Act 1 chair variables), the terminal screens, his waiting. |
| Mixed Signals (`NVDLC01MQ02c`) | 10, 20, 30, 50, 60, 70, 100, 255 | companion escort and wait, electrical box repair, elevator, remote terminal | **PARTIAL** | Started as MQ02a. Not played: the switching station, repairing the electrical box, the elevator, the remote maintenance terminal. |
| Strike Up the Band (`NVDLC01MQ02b`) | 10, 30, 40, 100, 255 | companion escort and wait, security holograms, switches, rooftop, items | **PARTIAL** | Started as MQ02a. Not played: escorting Dean to the rooftop, turning on both security holograms, his waiting. Holograms as enemies are untried (see HoloSupport). |
| Mixed Signals (`NVDLC01MQ02c`) | 10, 20, 30, 50, 60, 70, 100, 255 | companion escort and wait, electrical box repair, elevator, remote terminal | **PARTIAL** | Played from `characters/dead-money-act2-christine.txt`: the station interior, Christine's trigger and talk (`--say`), the electrical box repair (message buttons, Repair 60, XP, stage 30 objective), the elevator panel, and the ending's result lines to completion. Not played: the toxic cloud and the terminals' screens, the ghost gauntlet, going up with her, the later replies by hand. |
| Heist of the Centuries (`NVDLC01MQ03`) | 10, 20, 30, 40, 50, 100, 255 | casino interior, receptionist terminal and music sequence puzzle, vault with locks and voice code, elevators, sneaking, a boss fight, collar explosion timer, music, fade to credits | **NOT PLAYED** | Blocked by Act 2 and the Gala for a normal start; the casino itself is not a blocker any more: its interior loads in about 5 seconds and its power switch works (see MQ03a), from the test character `characters/dead-money-casino.txt`. Never tried: the vault, the music-sequence terminal, the fight with Elijah, the optional sneak-out. Repeating stages allowed (flag 0x08). |
| Put the Beast Down (`NVDLC01MQ03a`) | 5, 10, 20, 30, 40, 50, 60, 100 | casino, key, stealth (not being spotted by Dog), gas valves, dialogue or combat, collar timer | **PARTIAL** | Played: starting in the casino as the Lobby's knock-out script leaves the player (`characters/dead-money-casino.txt`: MQ02 done, MQ03a at stage 5), then the power switch with E (`--use`): MQ03a goes to stage 10, MQ03b and MQ03c start, the slot machines, hologram dealers and bartender come into view, Elijah's intercom speaks. Not played: reaching the switch on foot, the kitchen key, the gas valves, Dog's fight or talk, the collar timer. [G] The detection rules for 'not spotted' use nv-rs's own detection, unverified for this. Also played (current main): the kitchen's three gas valves (message buttons, Repair check, XP, objective), Dog's barks and collar sequence to his death; one run ended in "Quest Failed" for MQ03 that is not explained [C]. |
| Curtain Call at the Tampico (`NVDLC01MQ03b`) | 10, 15, 20, 30, 40, 50, 60, 100 | theatre interior, security holograms, holotape projector, key, combat or dialogue, collar timer | **NOT PLAYED** | The theater interior loads and its start scripts run (Dean a ghost); nothing else played. Blocked by holograms as enemies being untried (they fought ghosts in the Gala test). |
| Last Luxuries (`NVDLC01MQ03c`) | 10, 20, 30, 100 | executive suites, combat with a former companion, collar timer | **NOT PLAYED** | The suites interior loads and its start scripts run (holograms, Christine enabled); nothing else played. A quest script ends it if Christine dies by other means. |
| Heist of the Centuries (`NVDLC01MQ03`) | 10, 20, 30, 40, 50, 100, 255 | casino interior, receptionist terminal and music sequence puzzle, vault with locks and voice code, elevators, sneaking, a boss fight, collar explosion timer, music, fade to credits | **PARTIAL** | Played from `characters/dead-money-vault.txt` with helpers: stages 20 to 50 (SetStage), the vault elevator with Elijah dealt with (stage 100, XP 200, "Safety Deposit Box", the Villa's end marker), the launcher `FNV Vault Ending`. Not played: the receptionist terminal and music puzzle (Chazm), the vault's doors, the fight with Elijah, the 70 second vault collar. [C] The vault in the original. |
| Put the Beast Down (`NVDLC01MQ03a`) | 5, 10, 20, 30, 40, 50, 60, 100 | casino, key, stealth (not being spotted by Dog), gas valves, dialogue or combat, collar timer | **PARTIAL** | Played: starting in the casino as the Lobby's knock-out script leaves the player (`characters/dead-money-casino.txt`: MQ02 done, MQ03a at stage 5), then the power switch with E (`--use`): MQ03a goes to stage 10, MQ03b and MQ03c start, the slot machines, hologram dealers and bartender come into view, Elijah's intercom speaks. Not played: reaching the switch on foot, the kitchen key, the gas valves, Dog's fight or talk, the collar timer. [G] The detection rules for 'not spotted' use nv-rs's own detection, unverified for this. |
| Curtain Call at the Tampico (`NVDLC01MQ03b`) | 10, 15, 20, 30, 40, 50, 60, 100 | theatre interior, security holograms, holotape projector, key, combat or dialogue, collar timer | **NOT PLAYED** | Blocked by the casino and by holograms as enemies being untried. |
| Last Luxuries (`NVDLC01MQ03c`) | 10, 20, 30, 100 | executive suites, combat with a former companion, collar timer | **PARTIAL** | Played from `characters/dead-money-suites.txt`: the greeting and the replies up to the fight, the collar rules (proximity, then scanning), Christine's death (XP, objective, 30 second timer), the elevator (stage 100) and leaving to survive, or staying to be killed by the timer. Not played: winning the fight by shooting, the friendly Christine, the talk-down reply, the terminal that turns the holograms on her. [C] How the original balances the fight. |
| Curtain Call at the Tampico (`NVDLC01MQ03b`) | 10, 15, 20, 30, 40, 50, 60, 100 | theatre interior, security holograms, holotape projector, key, combat or dialogue, collar timer | **PARTIAL** | Played from `characters/dead-money-theater.txt` with console-line helpers: the key trigger (stage 15), Vera's Partitures and the key, the backstage door waking the holograms, Dean's holotape (stage 30), the projector and the show (stage 40), Dean's death (stage 60) and the escape trigger (stage 100). Helpers: the collar switch off, moves into triggers, `Activate Player 1`, `AddItem`, `Kill`. Not played: by hand, the speakers, the holograms' threat, the fight, Dean's talk, the Vera-tape way out. [C] How long the player has to leave after Dean dies. |
| Last Luxuries (`NVDLC01MQ03c`) | 10, 20, 30, 100 | executive suites, combat with a former companion, collar timer | **NOT PLAYED** | Blocked by reaching the suites. A quest script ends it if Christine dies by other means. |
| Wake Up the Sierra Madre (`NVDLC01MQ03d`) | 10, 20, 100 | casino electrical closet, power switch, security holograms | **NEVER STARTED** | Nothing in `DeadMoney.esm` starts it or sets one of its stages: no script, dialogue result, terminal or other record names the quest's form (all of the plugin was searched, 2026-10-10, answered from the data), so it cannot be played and the original never plays it. Its two objectives are the ones MQ03a shows as its stage 10. |

### The opening and the ending

Scripted quests that carry the player between the bunker, the Villa and the credits.

| Quest (editor ID) | Stages | Needs | Status | Notes |
| --- | --- | --- | --- | --- |
| (no name) (`NVDLC01Intro`) | none | fade, slideshow textures, narrator voice, music, Pip-Boy radio off | **PLAYED** | Played in the bunker launch; confirmed by the maintainer (narrator voice, slide changes). |
| (no name) (`NVDLC01VillaTransitionTimer`) | none | fade to black, move to the Villa, music, sound | **PLAYED** | Played in the bunker launch: it moves the player to the Villa. |
| (no name) (`NVDLC01FountainStartSequence`) | 10, 20 | script packages on the hologram, fade in, controls back on, hologram dialogue | **PLAYED** | Played: the wake-up and Elijah's hologram starting to talk (the maintainer watched the replies appear). |
| (no name) (`NVDLC01BunkerTransitionTimer`) | none | fade, move to the bunker, music, radio station for Elijah | **PARTIAL** | Played at the end of the ending: the stinger, the fade, the move to the bunker, controls back, Elijah's radio. Not played: the fade's look. |
| (no name) (`NVDLC01FadeToCreditsTimer`) | none | fade, kill actor, autosave, music | **NOT PLAYED** | Belongs to the ending, never reached. |
| (no name) (`NVDLC01Ending`) | none | slideshow of endings [G] | **PARTIAL** | Played from the same route: the Mojave gate's box, the end slides' narrator lines for all companions and the closing speech, in the slideshow cell. Not played: the slide pictures (not drawn), the other variants (Elijah's team, the player trapped). |
| (no name) (`NVDLC01BunkerTransitionTimer`) | none | fade, move to the bunker, music, radio station for Elijah | **NOT PLAYED** | Belongs to the ending, never reached. |
| (no name) (`NVDLC01FadeToCreditsTimer`) | none | fade, kill actor, autosave, music | **PARTIAL** | Started with `StartQuest` in the vault character: the stinger, the permanent fade to black and the player's death (the reload of the last autosave is not built). Not played: reached from the ending's "failure" slide. |
| (no name) (`NVDLC01Ending`) | none | slideshow of endings [G] | **NOT PLAYED** | Never reached. [G] What it plays isn't known beyond its name. |

### Elijah's voice

Quests that hold Elijah's radio lines; their 'objectives' are mostly spoken lines, not journal objectives.

| Quest (editor ID) | Stages | Needs | Status | Notes |
| --- | --- | --- | --- | --- |
| Elijah Signal Established (`NVDLC01ElijahBarkTown`) | 27 stages (1 to 255) | radio on the collar, trigger volumes in the Villa, the Cloud, speakers, holograms, vending machines, the Gala route | **PARTIAL** | Played: his first lines at the fountain after the wake-up. Not heard: the place-triggered lines further on (the Cloud, the speakers, the clinic, the gala route, the casino gate) and the closing line when the player disobeys. |
| Elijah Signal Re-Established (`NVDLC01ElijahBarkCasino`) | none (about 60 numbered lines) | radio in the casino, locks, holograms, vault, kitchen gas, collar timers | **PARTIAL** | Started in the Lobby route; the tape objectives (ids 1E to DC) are not played. Its casino-side lines follow the power switch (see the casino route). [C] How the tape hunt goes in the original. |
| Lobby Casino Support Quest (`NVDLC01ElijahBarkLobby`) | none | casino lobby trigger | **PARTIAL** | Played from `dead-money-casino.txt` in the Lobby (`FNV Lobby Intercom`): Elijah's full lobby introduction through the intercom, line after line, to `bElijahLobbyIntroDone`. Not played: the casino-side repeats. |
| (no name) (`NVDLC01ElijahCollarBark`) | none | collar radio conversation (StartRadioConversation), timer | **PARTIAL** | The collar's radio interference messages worked in the Villa; the radio conversation playback itself isn't built (see the Radio section of DEAD_MONEY.md). |

### Companions and dialogue

Dialogue holders and timers for the companions.

| Quest (editor ID) | Stages | Needs | Status | Notes |
| --- | --- | --- | --- | --- |
| DLC01 Followers (`NVDLC01Followers`) | none | companions, follower faction | **PARTIAL** | Christine followed the player after recruit and companions came into view at the fountain; whether Dog and Dean follow as the original does hasn't been watched. [G] The follow distance (200 units) is a guess. |
| Dog/God Dialogue (`NVDLC01DogDialogue`) | none | dialogue, perks that switch Dog and God, sneaking, cage key knowledge | **PARTIAL** | Greeting and tape talk worked; the Dog/God perk switch and his later lines are untried. |
| Dean Dialogue (`NVDLC01DeanDialogue`) | none | dialogue, perk, holograms | **PARTIAL** | The first greeting played for real once the player could sit; later lines untried. |
| Christine Dialogue (`NVDLC01ChristineDialogue`) | none | dialogue, elevator, vault, collar disarm | **PARTIAL** | The recruit conversation played; the vault and collar-disarm lines belong to Act 3. |
| Father Elijah Dialogue (`NVDLC01ElijahDialogue`) | none | dialogue, casino barks | **PARTIAL** | The Act 1 and Act 2 greetings and replies played; his casino lines haven't. |
| (no name) (`NVDLC01DogBarkTimer`) | none | 30 second timer, companion idle lines | **PARTIAL** | Played with `dead-money-act2.txt` and the launcher `FNV Dog Barks`: the timer ticks every 30 seconds and Dog says his generic idle lines with his voice. Not played: the other states (not recruited, God in control, at the Gala), not in combat. |
| (no name) (`NVDLC01DeanBarkTimer`) | none | 30 second timer, companion idle lines | **PARTIAL** | Played with `dead-money-act2-dean.txt` and `FNV Dean Barks`: at the rooftop Dean says "Do not leave me here." with his voice. Not played: his other states. |
| (no name) (`NVDLC01ChristineBarkTimer`) | none | 30 second timer | **PARTIAL** | Started with the Christine escort character: the timer ticks and asks for her Act 3 idle topic, which has no line in Act 2 ("nothing to say"). Not played: Act 3 (suites). [C] Her idle barks in the original. |
| (no name) (`NVDLC01ChristineTerminalFX`) | none | vault password terminal, elevator unlock, conversation | **NOT PLAYED** | Act 3; never reached. |
| (no name) (`NVDLC01DogBarkTimer`) | none | 30 second timer, companion idle lines | **NOT PLAYED** | Never watched. |
| (no name) (`NVDLC01DeanBarkTimer`) | none | 30 second timer, companion idle lines | **NOT PLAYED** | Never watched. |
| (no name) (`NVDLC01ChristineBarkTimer`) | none | 30 second timer | **NOT PLAYED** | Never watched. |
| (no name) (`NVDLC01ChristineTerminalFX`) | none | vault password terminal, elevator unlock, conversation | **PARTIAL** | Played from `dead-money-suites.txt` (`FNV Suites Terminal FX`): the quest started, the elevator unlocks and after 5 seconds "The Vault Elevator unlocks its seals". Not played: reaching it from the terminal (Chazm's), her talk after it. |
| (no name) (`NVDLC01NewVegasFollowerFireQuest`) | none | dismissing the player's Mojave companions on arrival | **NOT PLAYED** | The test character has no companions, so it never had anything to do. |
| (no name) (`NVDLC01StarletDialogue`) | none | hologram dialogue, bark counter | **PARTIAL** | Played in the suites route: the Starlet hologram's barks count up `iHoloBark`; the quest holds only that counter. Her talk with the player is not played. |
| (no name) (`NVDLC01ChristineTerminalFX`) | none | vault password terminal, elevator unlock, conversation | **NOT PLAYED** | Act 3; never reached. |
| (no name) (`NVDLC01NewVegasFollowerFireQuest`) | none | dismissing the player's Mojave companions on arrival | **PARTIAL** | Played from `dead-money-casino.txt` (`FNV Follower Fire`) with Boone and with Arcade hired by console line: moved home, perk lost, both boxes shown. Not played: the other companions. |
| (no name) (`NVDLC01StarletDialogue`) | none | hologram dialogue, bark counter | **NOT PLAYED** | Starlet's hologram dialogue; not tried. |

### Collars and radio

The bomb collar and the stations the collars and speakers play.

| Quest (editor ID) | Stages | Needs | Status | Notes |
| --- | --- | --- | --- | --- |
| Bomb Collar Quest (`NVDLC01BombCollarQuest`) | none | collar timers, speakers, beeps, rumble, explosion, kill, companions | **PARTIAL** | The beeping and the radio interference messages worked in the Villa. Not played: the countdown to the explosion and a death by it, the Christine collar message, the casino collar rules. |
| Bomb Collar Audio (`NVDLC01RadioSpeakers`) | none | radio station | **PARTIAL** | Started: runs; only variables. Its radio lines need the radio engine. |
| Dog's Collar Radio (`NVDLC01RadioDog`) | none | radio station | **PARTIAL** | Started in the casino character: the broadcast flag rules ran (with Dog beside the player, `RadioDog` switched *Dean's* station off and left Dog's on, as the script reads). The lines are radio conversations: no radio playback (`StartRadioConversation` is a no-op). |
| Dean's Collar Radio (`NVDLC01RadioDeanQuest`) | none | radio station | **PARTIAL** | Started with Dean beside the player: Dean's station switched off, by the script. Radio conversations are not played (no radio engine). |
| (no name) (`NVDLC01RadioDeanMusic`) | none | radio station music | **PARTIAL** | Started: runs; it has no script (its lines are dialogue only). Needs the radio engine to be heard. |
| Christine's Collar Radio (`NVDLC01RadioChristine`) | none | radio station | **PARTIAL** | Started with Christine beside the player: her station switched off, by the script. Radio conversations are not played (no radio engine). |
| Elijah's Collar Radio (`NVDLC01RadioElijah`) | none | radio station, elevator, vault | **PARTIAL** | Started: the quest runs (its script only holds variables); Elijah's radio lines need the radio engine, which nv-rs lacks. |
| Vera Radio Dialogue (`NVDLC01RadioCasino`) | none | radio station, music sequence | **PARTIAL** | Started: runs; only a line counter. Vera's radio lines need the radio engine. |
| DLC01 Ambient Music (`NVDLC01RadioAMBMusic`) | none | radio station, music | **PARTIAL** | Started: runs; only two flags. The ambient music changes it makes are `StartRadioConversation`, which does nothing here. |

### World systems and support

Quests that hold a system's rules.

| Quest (editor ID) | Stages | Needs | Status | Notes |
| --- | --- | --- | --- | --- |
| Script Functionality for Toxic Clouds (`NVDLC01ToxicCloud`) | none | the Cloud, damage over time, vision effect, companion warnings | **PARTIAL** | Played from `dead-money-act2-christine.txt` (`FNV Toxic Cloud`): the player in the cloud trigger takes the script's damage until death, with the warning message and the image effect. Not played: leaving the cloud, Dean's reduction, the exterior weather. |
| Script Functionality for Global Toxic Damage (`NVDLC01GlobalToxicDamage`) | none | the Cloud, timer | **PARTIAL** | Started from `dead-money-act2.txt` (`FNV Global Toxic`): the script runs and exits at once because Hardcore is off; nv-rs cannot turn Hardcore on. Blocked by the missing Hardcore setting. [C] The original's damage rhythm. |
| Scripting Support for Hologram Features (`NVDLC01HoloSupport`) | none | security holograms, enabling and disabling them | **PARTIAL** | Started by the fountain sequence and ran in the Villa without errors; the holograms as armed enemies were not played. |
| Hologram Vendor Quest (`NVDLC01HoloVendor`) | none | hologram vending machines, recipe menu, first-use message | **PARTIAL** | Played from `dead-money-theater.txt`: the Tampico's hologram vending machine shows its first-use box and sets the flag. Not played: the recipes. |
| NVDLC01WTSupport (`NVDLC01WTSupport`) | none | terminal use flags, hologram flags, the Gala lockdown | **NOT PLAYED** | Never reached. |
| (no name) (`NVDLC01Challenges`) | none | challenges (achievement-like counters), alert state | **NOT PLAYED** | Counters exist in nv-rs; none was played through. |
| NVDLC01CasinoComps (`NVDLC01CasinoComps`) | none | casino winnings level, chips, casino games | **NOT PLAYED** | The casino games (slots, blackjack, roulette) aren't built. |
| Chip Reward Quest (`NVDLC01ChipReward`) | none | chips, casino comps, timers | **NOT PLAYED** | Needs the casino comps. |
| (no name) (`NVDLC01ClinicAutoDocQuest`) | none | a 4 second fade, a sound, controls back on (ordinary Auto-Docs) | **PARTIAL** | Started with `StartQuest` in the clinic: the fade to black with its sound, then the controls back on. The game starts it when an ordinary Auto-Doc is used; the clinic's own Auto-Doc scene (MQ01c) is in the Christine route. |
| (no name) (`NVDLC01Challenges`) | none | challenges (achievement-like counters), alert state | **PARTIAL** | The challenge counters its DLC scripts call were seen in the routes (Loose Ends, Assemble Your Crew, Having a Ball, Safety Deposit Box). Not played: the quest's own Authority Issues and History's Sake counters. |
| NVDLC01CasinoComps (`NVDLC01CasinoComps`) | none | casino winnings level, chips, casino games | **PARTIAL** | Started in the vault character: it runs; the winnings level stays 0 because the casino games (Chazm's) record none, so no comp is given. Blocked by the games. |
| Chip Reward Quest (`NVDLC01ChipReward`) | none | chips, casino comps, timers | **PARTIAL** | Played from `dead-money-vault.txt` (`FNV Casino Support`): started as MQ03's end does it, 100 chips go to Elijah's dropbox. Not played: the repeat after 3 game days, the comp voucher. |
| (no name) (`NVDLC01ClinicAutoDocQuest`) | none | a 4 second fade, a sound, controls back on (ordinary Auto-Docs) | **NOT PLAYED** | The short fade and sound after a cure at an ordinary Auto-Doc in the clinic; not played. Christine's pod has its own script, which was played (see MQ01c). |
| (no name) (`NVDLC01GalaEventQuest`) | none | timers, enabling things, Dean's topic | **NOT PLAYED** | Needs MQ02. |
| DLC01 Enemy Test (`NVDLC01EnemyTest`) | 10, 11, 20, 21, 22, 30, 31, 32, 50, 51 | teleports, keys, followers, enabling ambush groups | **PARTIAL** | Played with `dead-money-casino.txt` and `FNV Enemy Test`: stages 10, 11, 20, 21, 22, 30, 31, 32 by `SetStage`; the result scripts run (ghosts enabled and disabled, the three recruitments' results, teleports to the escorts' places). A developer's quest; stages 50 and 51 not looked at. |
| (no name) (`NVDLC01ArcadeGoodbye`) | none | dialogue only [G] | **NOT PLAYED** | No script. [G] Presumably Arcade's farewell line when dismissed on arrival. |
| DLC01 Enemy Test (`NVDLC01EnemyTest`) | 10, 11, 20, 21, 22, 30, 31, 32, 50, 51 | teleports, keys, followers, enabling ambush groups | **NOT PLAYED** | [G] Looks like a developer test quest (it moves the player between test spots and enables trappers); not part of the story. |
| (no name) (`NVDLC01ArcadeGoodbye`) | none | dialogue only [G] | **PARTIAL** | A start-game quest with no stages or script; the dismissal that would trigger his goodbye was played (messages, move home) and no spoken line was seen. [C] The goodbye in the original. |

## Next quests to play

Of the quests not finished, the three that look best to take next (a judgement [G], by size and by
what they unblock, not a measured effort). The casino has been shown to load and play, so the MQ03
quests are open to a test character that starts there:

1. **`NVDLC01MQ02b` Strike Up the Band (Dean).** The next companion escort after Dog's: the rooftop
   and its two security holograms and switches. It tests holograms as enemies and a second wait/follow
   run, which the first escort has now cleared.
2. **`NVDLC01MQ02c` Mixed Signals (Christine).** The switching station: the electrical box, the elevator
   and the remote terminal. Together with MQ02b it unblocks the Gala itself (`NVDLC01MQ02`).
3. **`NVDLC01MQ03c` Last Luxuries.** Four stages, one fight against Christine in the executive suites
   with her collar's timer. Open now from the casino character; a good test of combat with a former
   companion and the collar explosion.

Not on the list on purpose: MQ02 (needs MQ02a to c first), MQ03 itself and MQ03a/b (large), and the radio quests (the station playback is a feature to build, not a quest to
play).

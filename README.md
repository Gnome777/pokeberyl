# Pokémon Beryl

This is a ROMhack of Pokémon Emerald. This is not a difficulty hack or a catch-em-all, although it modifies the difficulty of boss trainers and allows for a wider selection of Pokémon. This is a reimagining of the third generation by me, essentially answering the question "What if I was in charge of making the Pokémon games?"

It includes the following features:

- Savefile backwards compatibility
- Connectivity with Vanilla and other hacks (that support it)
- A variety of bugfixes
- Shops provide items on a badge-count basis instead of being limited by location
- Repels now prompt you for reuse
- Pokéballs can be used outside of battle on Pokémon to switch the ball they're held in]
- Keep party menu open during field medicine use
- Move items between Pokémon without needing to open the bag
- Bag sorting by name, quantity, or type
- Use any field move if Pokémon in your party can learn it
- Chain fishing (fishing up the same species increases the chance of it being shiny next time)
- PC in the pokeNAV
- Hidden Power type in summary screen
- Color stats by nature in summary screen
- Physical/Special Split
- Learn moves upon evolution
- IVs/EVs in summary screen
- Press B in wild battles to Run
- Move Relearner in the party screen
- Move Relearner can teach moves to pre-evolutions
- Option to show type effectiveness in battle
- Pressing start on a move in battle shows stats and description
- Swap Pokémon using Select in the party menu
- Return/Frustration power in summary screen
- Item Descriptions on first obtain
- Infinite TM Usage and unsellable
- Money limit increased to 99,999,999
- Multiple Premier Balls for each set of 10 Pokéballs
- IV Checker NPC added
- Pokémon can forget HM moves
- Field Moves no longer require badges
- Match Calls only appear if Caller wants rematch
- Jump over Ledges with Acro Bike
- Trees are permanently cut, once cut for the first time
- Removal of badge boosts and disobedience
- Generation 6 EXP Share
- Gain XP by catching a Pokémon
- Roamers no longer flee
- Improved Trainer AI and Partner Battle Code
- Daycare Pokémon have perfect IVs
- Shifting to a Pokémon already in battle exits the shift menu
- Defeating a wild Pokémon with an item puts the item in your bag
- Trade with FRLG without beating the game

It builds the following ROM:

* [**pokeemerald.gba**](https://datomatic.no-intro.org/index.php?page=show_record&s=23&n=1961) `sha1: f3ae088181bf583e55daf962a92bb46f4f1d07b7`

To set up the repository, see [INSTALL.md](INSTALL.md).

## FAQ
### `(followers*)` Q: Where are the config settings?
A: Configuration for the follower system is mostly in [event_objects.h](include/constants/event_objects.h):
```c
// If true, follower pokemon will bob up and down
// during their idle & walking animations
#define OW_MON_BOBBING  TRUE

// If true, adds a small amount of overhead
// to OW code so that large (48x48, 64x64) OWs
// will display correctly under bridges, etc.
#define LARGE_OW_SUPPORT TRUE

// Followers will emerge from the pokeball they are stored in,
// instead of a normal pokeball
#define OW_MON_POKEBALLS TRUE

// New/old handling for followers during scripts;
// TRUE: Script collisions hide follower, FLAG_SAFE_FOLLOWER_MOVEMENT on by default
// (scripted player movement moves follower too!)
// FALSE: Script collisions unhandled, FLAG_SAFE_FOLLOWER_MOVEMENT off by default
#define OW_MON_SCRIPT_MOVEMENT TRUE

// If set, the only pokemon allowed to follow you
// will be those matching species, met location,
// and/or met level;
// These accept vars, too: VAR_TEMP_1, etc
#define OW_MON_ALLOWED_SPECIES (0)
#define OW_MON_ALLOWED_MET_LVL (0)
#define OW_MON_ALLOWED_MET_LOC (0)
```

### `(lighting)` Q: How do I mark certain colors in a palette as light-blended?
A: Create a `.pla` file in the same folder as the `.pal` with the same name.

In this file you can enter color indices [0,15]
on separate lines to mark those colors as being light-blended, i.e:

`06.pla:`
```
# A comment
0 # if color 0 is listed, uses it to blend with instead of the default!
1
9
10
```

You might have to `make mostlyclean` or change the `.pal` file to pick up the changes.

### `(guillotine)` Q: How can I keep my string(s) from being decapped?
A: There are a number of ways to make a string "fixed case" so that it will not be decapitalized when displayed:

C strings: Replace the `_` with `_C`:
```c
// _C = fixed (C)ase string!
const u8 gText_IDNumber[] = _C("IDNo.");
```
ASM strings: Replace `.string` with `.fixstr`:
```arm
gText_SavingDontTurnOff::
    @ Lasts until the string terminator '$'
	.fixstr "SAVING…\n"
	.string "DON'T TURN OFF THE POWER.$"
```
You can fix-case/unfix parts of a string like so:
```arm
	.string "{FIXED_CASE}WARNING!{UNFIX_CASE}\p"
```
For a placeholder (only the placeholder will be fixed-case):
```arm
	.string "{STR_VAR_2_FIXED} was transferred to\n"
	.string "BOX “{STR_VAR_1}.”$"
```
See also the configuration in [text.h](gflib/text.h).

There's also special handling for "separated bigrams"; basically, two letter words.
This includes: `"TM01", "PC", "EV"`, any two uppercase characters surrounded by digits, whitespace, or the start/end of a string. These will not be decapped.

## See also

For contacts and other pret projects, see [pret.github.io](https://pret.github.io/).

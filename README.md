<p align="center">
  <img src="https://github.com/user-attachments/assets/f627c110-ed9a-45ff-9631-f3ef5d426959" alt="TurboLoot Banner" width="100%">
</p>

# Quickstart Guide

> **TurboLoot** - Automated INI based corpse looting, selling, banking, tributing, destroying, collecting, distributing for EverQuest EMUs (E3Next / MacroQuest)

**On this page:** [Download](#-download) · [Install](#1-installation-2-minutes) · [How loot chooses](#2-how-turboloot-decides-what-to-loot) · [INI](#3-the-ini-at-a-glance) · [Commands](#4-commands) · [TurboKey / TurboGive](#5-turbokey--turbogive-quick-tag--handouts) · [Workflow](#6-typical-workflow) · [Auto-loot](#7-auto-looting-setup-e3next) · [Multi-looter](#8-multiple-looters) · [Wildcards](#9-wildcards) · [Tips](#10-tips)

## 📥 Download

For most users, the easiest way to get started is to download the latest release:

👉 https://github.com/drel-git/TurboLoot/releases/latest

This includes the latest macros and INI files:

- **`TurboLoot.ini`**- minimal **template** (safe starting point).
- **`Example TurboLoot.ini`**- **heavy sample** with lots of items; use for ideas or diff against your own INI.

Rename or copy the template to **`turboloot.ini`** where TurboLoot expects it (`Config` or `Macros`- see [Install](#1-installation-2-minutes)).

---

## What's In the Box

| File | What It Does |
|---|---|
| `TurboLoot.mac` | The main macro - runs all looting and inventory management |
| `TurboGive.mac` | Companion macro - distributes items between characters using `[GiveList]` in the same INI |
| `TurboKey.mac` | Quick-tag helper - pick up an item, run one command, and it's categorized in your INI |
| `TurboLoot.ini` | **Starter template**- copy to Config/Macros as `turboloot.ini` and customize |
| `Example TurboLoot.ini` | **Large sample**- same layout with many `[ItemLimits]` / `[GiveList]` examples |
| [`TurboGive Getting Started.md`](https://github.com/drel-git/TurboLoot/blob/main/TurboGive%20Getting%20Started.md) | TurboGive quick start + full command tables |
| `PATCH_NOTES.md` | Release highlights, plain-English `FastLootMode` explanation, and TurboGive overview |

---

## 1. Installation (2 Minutes)

1. Drop **`TurboLoot.mac`**, **`TurboKey.mac`**, and (if you use handouts) **`TurboGive.mac`** into your MQ **`Macros`** folder (or **`Config`** - TurboLoot checks both for the INI path; the `.mac` files usually live in **`Macros`** per your MQ setup).
2. Copy **`TurboLoot.ini`** (template) to **`turboloot.ini`** in **`Config`** (preferred) or **`Macros`**. TurboLoot and TurboKey load **`../Config/turboloot.ini`** first, then **`../Macros/turboloot.ini`**. (Optional: browse **`Example TurboLoot.ini`** for a filled-out reference.)
3. In-game, type: `/mac TurboLoot` - if it runs without errors, you're set. Use `/mac TurboGive help` to verify TurboGive if you installed it.


<-------------------------------------> **I recommend using a good text editor like [Notepad++](https://notepad-plus-plus.org/downloads/) or [VSCodium](https://vscodium.com/)** <------------------------------------->

---

## 2. How TurboLoot Decides What to Loot

TurboLoot checks items in this order:

1. **Exact item name in `[ItemLimits]`** - if it finds a rule for the item, it follows that rule.
2. **Wildcard matches** - spell scrolls (`SPELL:`), skill-ups (`SKILL:`), tomes, songs are handled by wildcard settings.
3. **Value-based rules** - if the item isn't listed, it checks against your minimum platinum thresholds:
   - `lootHighValueMinPP` - loot non-stackable items worth this many pp or more (default: 50)
   - `lootStackableMinPP` - loot stackable items worth this many pp or more (default: 50)
4. **Everything else** - skipped and announced (or silently ignored, depending on your settings).

---

## 3. The INI at a Glance

One file, several sections:

| Section | Used by | Purpose |
|---------|---------|---------|
| `[Settings]` | TurboLoot | Radius, announcements, `FastLootMode`, sell/bank toggles, etc. |
| `[Wildcards]` | TurboLoot | Prefix rules for spells, skills, tomes, customs - [§9 Wildcards](#9-wildcards) |
| `[ItemLimits]` | TurboLoot, TurboKey | Per-item loot rules (`SELL`, `BANK`, …) |
| `[GiveList]` | TurboGive | Who receives which items / patterns - see [§5](#5-turbokey--turbogive-quick-tag--handouts) and **[`TurboGive Getting Started.md`](TurboGive%20Getting%20Started.md)** |

### [Settings] - Recommended Defaults

```ini
[Settings]
; --Core--
lootRadiusFeet=100
inventoryWarnSlots=5
debug=OFF
logToFile=OFF
; logLevel: DEBUG, INFO, WARN, or ERROR. Controls minimum level written to file when logToFile=ON.
; DEBUG=everything, INFO=operations (default), WARN=warnings only, ERROR=errors only.
; logLevel=INFO

; --Looting & Selling Rules--
lootHighValueMinPP=50
lootStackableMinPP=50
sellUnlistedStackable=OFF
sellUnlistedItems=OFF
sellWildcards=OFF
bankWildcards=ON
StopLootWhenAttacked=OFF
returnToLeader=ON
dropLevBeforeNav=OFF
; OFF = default (fast and reliable). Set ON if you have a setup that can handle it (fastest, but can skip loot if you have a bad connection)
FastLootMode=OFF

; --Announcements--
announceDefaultTo=e3bc
announceDoneLootingTo=e3bc
announceSkipTo=gsay
announceBankSellPerItem=ON
announceLoot=ON
announceDestroy=ON
announceRunSummary=ON
autoRsayInRaid=OFF

; --Advanced--
corpseHideMode=LOOTED
lootNoDropPrompt=never
lootNoDropPromptReset=always
```

### What Each Setting Does

| Setting | Default | What It Does |
|---|---|---|
| `lootRadiusFeet` | 100 | How far (in feet) TurboLoot will search for corpses |
| `inventoryWarnSlots` | 5 | Warns you when you have this many free inventory slots left |
| `debug` | OFF | Enable debug output (echo) |
| `logToFile` | OFF | Append to `Logs/TurboLoot.mac.log` when ON (see also `logLevel` in sample INI) |
| `lootHighValueMinPP` | 50 | Loot non-stackables worth ≥ this many pp. `0` = disabled |
| `lootStackableMinPP` | 50 | Same but for stackable items |
| `sellUnlistedStackable` | OFF | When selling, also sell unlisted stackable items |
| `sellUnlistedItems` | OFF | When selling, also sell items not in your INI (be careful!) |
| `sellWildcards` | OFF | Auto-sell wildcard items unless marked otherwise |
| `bankWildcards` | ON | Auto-bank wildcard items unless marked otherwise |
| `StopLootWhenAttacked` | OFF | Stop looting if hostile mobs are detected nearby |
| `returnToLeader` | ON | Return to group leader after looting |
| `corpseHideMode` | LOOTED | After loot: `LOOTED`, `ALL`, `GROUP`, `SELF`, or `OFF` - see sample INI comments |
| `announceDefaultTo` | e3bc | Where messages go: `echo`, `e3bc`, `say`, `gsay`, `rsay`, `t CharName` |
| `announceSkipTo` | gsay | Where skip messages go - `gsay` lets your group see what was left behind - use gsay if you use the LazBiS lua |
| `announceBankSellPerItem` | ON | Announce each item individually during bank/sell/tribute operations |
| `announceLoot` | ON | Announce when items are looted |
| `announceDestroy` | ON | Announce when items are destroyed |
| `announceRunSummary` | ON | Show a summary at the end of each loot run |
| `autoRsayInRaid` | OFF | Auto-switch announcements to `/rsay` when in a raid |
| `announceDoneLootingTo` | e3bc | Where DONE LOOTING messages go: `echo`, `e3bc`, `say`, `gsay`, `rsay`, `t CharName`. **Solo:** **`echo`**, `t`/`tell`, **`say`**, **`rsay`**; **`gsay`** / **`e3bc`** need a group. Use a **space** after `t` / `tell`. |
| `lootNoDropPrompt` | never | No-drop behavior: `prompt` (ask), `always` (grab it), `never` (leave it) |
| `lootNoDropPromptReset` | always | Whether to reset no-drop prompt setting after each run |
| `FastLootMode` | OFF | Default OFF for reliability. Set ON for faster, more aggressive loot timing |

### [ItemLimits] - Your Loot Rules

This is where you tell TurboLoot exactly what to do with specific items. Add one line per item:

```ini
[ItemLimits]
Item Name=RULE
```

| Rule | What Happens |
|---|---|
| `KEEP` | Always loot, never sell/bank/destroy |
| `5` (any number) | Loot up to that many, then stop - alerts you when the limit is reached |
| `SELL` | Loot it, then sell it when you run the sell command |
| `BANK` | Loot it, then bank it when you run the bank command |
| `TRIBUTE` | Loot it, then tribute it when you run the tribute command |
| `DESTROY` | Loot it and destroy it immediately |
| `IGNORE` | Skipped on corpses (not looted). Skip lines may still go to your **announce skip** channel if configured. |

> **Note:** `ALL` still works as a legacy alias for `KEEP`, but `KEEP` is the preferred keyword going forward.

**Examples:**
```ini
[ItemLimits]
Kunark Green Pinot Gris=5
Emerald=KEEP
Junk Item=SELL
Mystery Orb=BANK
Some Tribute Item=TRIBUTE
Useless Trash=DESTROY
Rusty Dagger=IGNORE
```

---

## 4. Commands

Use `/e3bct LOOTERCHARACTERNAME` before any command below to have a bot run it instead of you.

| Command | What It Does |
|---|---|
| `/mac turboloot` | **Loot mode** - runs to nearby corpses and loots based on your rules |
| `/mac turboloot sell` | Sells all items marked `SELL` (must be at a vendor) |
| `/mac turboloot bank` | Banks all items marked `BANK` (must be at a banker) |
| `/mac turboloot tribute` | Tributes all items marked `TRIBUTE` (must be at a tribute master) |
| `/mac turboloot destroy` | Destroys items marked `DESTROY` (bags; main-inv slots may need moving - macro prints notices) |
| `/mac turboloot unload` | **Full dump:** bank → tribute → sell → destroy (all in one when you're in town) |
| `/mac turboloot sell dryrun` | Preview what *would* be sold (detailed; same family as `report`) |
| `/mac turboloot report` | Preview what *would* be sold - no items are touched |
| `/mac turboloot help` | Show the full command list in-game |

### 4.1 Set Up Aliases (Recommended)

Instead of typing the full commands every time, set up short aliases. Open your **`MacroQuest.ini`** in your E3Next Config folder and add these under `[Aliases]`:

```ini
[Aliases]
/turboloot=/squelch /mac turboloot
/turbosell=/squelch /mac turboloot sell
/turbobank=/squelch /mac turboloot bank
/turbotribute=/squelch /mac turboloot tribute
/turbounload=/squelch /mac turboloot unload
/turboreport=/squelch /mac turboloot report
/turbodestroy=/squelch /mac turboloot destroy
/turbohelp=/squelch /mac turboloot help
/tg=/squelch /mac turbogive
```

Now you can just type `/turboloot`, `/turbosell`, `/turboreport`, etc.

---

## 5. TurboKey & TurboGive (quick tag & handouts)

### TurboKey - tag items on the fly

Instead of manually editing your INI, pick up an item and run:

```
/mac TurboKey RULE
```

| Rule | Written to `[ItemLimits]` | What happens to the cursor item |
|------|---------------------------|----------------------------------|
| `KEEP` | Yes | `/autoinv` |
| `SELL` | Yes | `/autoinv` |
| `BANK` | Yes | `/autoinv` |
| `TRIBUTE` | Yes | `/autoinv` |
| `IGNORE` | Yes | `/destroy` (confirmation if the game asks) |
| `SKIP` | Stored as skip-like rule; TurboLoot treats like **IGNORE** for loot | `/destroy` (same as ignore for the item on cursor) |
| `DESTROY` | Yes | `/destroy` (confirmation if the game asks) |

Example: `/mac TurboKey SELL` - adds the cursor item as **`SELL`** and puts it in your bags.

**Overwrite:** If that item name already had a rule, it’s replaced; chat shows **old → new**.

---

### TurboGive - handouts (optional)

**TurboGive** uses the **same** `turboloot.ini` and writes **`[GiveList]`** (who gets what). It does **not** replace TurboLoot on corpses - it’s for moving loot to the right characters after the fact.

| Do this | Command |
|---------|---------|
| Full in-repo guide | **[`TurboGive Getting Started.md`](TurboGive%20Getting%20Started.md)** |
| Every command | `/mac TurboGive help` |
| Add cursor item for **target** character | `/mac TurboGive add` |
| Distribute to group (coordinated) | `/mac TurboGive` |
| Collect from group | `/mac TurboGive collect` |

**Patterns:** Only in **`[GiveList]`** lines like `_prefix1=Char:Spell:*` - **`Text*`**, **`*Text`**, **`*Text*`** - *not* in **`[Wildcards]`** (loot prefix-only). Details in **[`TurboGive Getting Started.md`](TurboGive%20Getting%20Started.md)**

**Example:** You loot a "Cracked Staff" and want to sell them from now on:
1. Pick it up (it's on your cursor)
2. Type `/mac TurboKey SELL`
3. Done - next time TurboLoot sees a Cracked Staff, it marks it for selling and will auto sell when you /mac turboloot sell or /mac turboloot unload.

---

## 6. Typical Workflow

1. **Kill mobs** → Run `/turboloot` (or set up auto-looting below)
2. **While grinding**, use `/mac TurboKey RULE` to categorize new items as you see them
3. **Before selling for the first time**, run `/turboreport` to preview what would be sold
4. **When bags are full**, head to town:
   - Run `/turbounload` near a banker, tribute master, and vendor to handle everything at once
   - Or run the individual commands (`/turbobank`, `/turbotribute`, `/turbosell`) one at a time
  (note: remember to set up Aliases in your MacroQuest.ini and hotkeys for ease of use)

---

## 7. Auto-Looting Setup (E3Next)

Set up TurboLoot to run automatically after kills

### Prerequisites
- `TurboLoot.mac` and `turboloot.ini` are installed (you can verify with `/mac turboLoot`)
- TurboLoot loads **`../Config/turboloot.ini` first**, otherwise **`../Macros/turboloot.ini`**. Debug/log lines show which file was used- edit **that** copy when changing settings (including `announceDoneLootingTo`).

### Step 1: Add the Event Trigger to Your Tank's E3Next INI

Paste these into your **tank's** character INI:

**Under `[EventRegMatches]`:**
```ini
Tloot=slain
```

**Under `[Events]`:**
```ini
Tloot=/timed 10 /if (!${Spawn[npc radius 50].Aggressive} && ${Turbo} && ${SpawnCount[npccorpse radius 50]}) /e3bct YOUR_LOOTER_NAME /mac turboLoot
```
> Replace `YOUR_LOOTER_NAME` with the character name of whoever should do the looting. Change the number after radius to whatever works for you.

**How it works:** When the game says something was "slain," the tank waits 1 second, checks that no aggressive mobs are within 50 units, checks that the `Turbo` toggle is on, checks that there's a corpse nearby, and then tells the looter character to run TurboLoot.

### Step 2: Optional - Auto-Enable on Startup

If you want auto-loot **on by default** when you load E3Next, add this to your tank's INI:

**Under `[Startup Commands]`:**
```ini
Command=/e3varset Turbo True
```

### Step 3: Toggle Auto-Loot On/Off

You need a way to flip auto-looting on and off. Choose **one** of these methods:

---

#### Option A: Button Master (Fancy Toggle Button)

If you use Button Master, create a button with:

**Button Label** (enable "Evaluate Label"):
```lua
return 'Auto: ' .. (tostring(mq.TLO.MQ2Mono.Query('e3, Turbo')()) == 'true' and "Loot ON" or "Loot OFF")
```

**Button Body:**
```
/docommand ${If[${Bool[${MQ2Mono.Query[e3,Turbo]}]},/e3bcaa /e3varset Turbo false,/e3bcaa /e3varset Turbo true]}
```

This gives you a single button that shows "Auto: Loot ON" or "Auto: Loot OFF" and toggles the state across all characters when clicked.

---

#### Option B: Two Hotkeys (No Button Master Needed)

Just make two in-game hotkeys:

**Hotkey 1 - Enable Auto-Loot:**
```
/e3varset Turbo true
```

**Hotkey 2 - Disable Auto-Loot:**
```
/e3varset Turbo false
```

Press one to turn it on, the other to turn it off. Simple.

---

### Step 4: Test It

1. **Check the toggle status:**
   ```
   /echo ${Bool[${MQ2Mono.Query[e3,Turbo]}]}
   ```
   Should return `TRUE` or `FALSE`.

2. **Toggle to Loot ON** (via button or hotkey).

3. **Kill a mob** - when you see the "slain" message, the looter should automatically run TurboLoot and start grabbing items.

4. **Toggle to Loot OFF** - kill another mob and confirm looting does *not* trigger.

---

## 8. Multiple Looters

By default, every character shares the same `turboloot.ini`. That works fine if you only have one looter or if all your looters follow the same rules. But if you want **multiple characters looting with different rules** (e.g., your bard loots gems to sell, your mage banks tradeskill mats), you can run separate copies of the macro with their own INI files.

### Setup: Duplicate Files Per Looter

1. **Copy the three files** and rename them with a suffix:
   - `TurboLoot.mac` -> `TurboLoot2.mac`
   - `TurboKey.mac` -> `TurboKey2.mac`
   - `turboloot.ini` -> `turboloot2.ini`

2. **Edit `TurboLoot2.mac`** - find the lines that reference `turboloot.ini` and change them to `turboloot2.ini`:
   ```
   ../Config/turboloot.ini  ->  ../Config/turboloot2.ini
   ../Macros/turboloot.ini  ->  ../Macros/turboloot2.ini
   ```

3. **Edit `TurboKey2.mac`** - same thing, update the INI path to `turboloot2.ini`.

4. **Edit `turboloot2.ini`** with your second looter's specific `[Settings]` and `[ItemLimits]`.

5. **Run it:** Your second looter uses `/mac turboloot2` instead of `/mac turboloot`, and `/mac TurboKey2 RULE` to tag items into their own INI.

All files live in the same `Macros` (or `Config`) folder - no need for separate MQ installations.

### Auto-Loot with Multiple Looters

If you want the tank to trigger multiple looters, add a separate event for each. In your tank's E3Next INI:

**Under `[EventRegMatches]`:**
```ini
Tloot=slain
Tloot2=slain
```

**Under `[Events]`:**
```ini
Tloot=/timed 10 /if (!${Spawn[npc radius 50].Aggressive} && ${Turbo} && ${SpawnCount[npccorpse radius 50]}) /e3bct LOOTER_ONE /mac turboLoot
Tloot2=/timed 20 /if (!${Spawn[npc radius 50].Aggressive} && ${Turbo} && ${SpawnCount[npccorpse radius 50]}) /e3bct LOOTER_TWO /mac turboLoot2
```

> The second looter uses `/timed 20` (2 seconds) so they don't both try to loot the same corpse at the exact same time. Note the second event runs `/mac turboLoot2` so it uses the second looter's INI and rules.

### If You Only Need One INI

If all your looters should follow the same rules, you don't need any of this. Just use one set of files and point the auto-loot event at whichever character you want doing the looting.

---

## 9. Wildcards

**TurboLoot `[Wildcards]`** only does **prefix matching** (name **starts with** your text). The **`Text*` / `*Text` / `*Text*`** pattern style exists **only** in **`[GiveList]` `_prefixN=Char:pattern`** for **TurboGive** (handouts), not for corpse looting-`*` in a `Wildcard1=` value is a **literal** character, not “ends with” or “contains.”

The five defaults work exactly like before. To disable one, just change it to OFF:

```ini
[Wildcards]
Spell:=ON
Skill:=ON
Song:=OFF
Tome =ON
Tome of =ON
```

### Add Custom Wildcards

Use `Wildcard1=` through `Wildcard20=`. The value is the **prefix** to match (the beginning of the item name):

```ini
[Wildcards]
Spell:=ON
Skill:=ON
Song:=ON
Tome =ON
Tome of =ON
Wildcard1=Rune of 
Wildcard2=Distillate of 
Wildcard3=Draught of 
```

This would auto-loot any item whose name **starts with** "Rune of", "Distillate of", or "Draught of".

> **Tip:** Include a trailing space after your prefix to avoid false matches. `Rune of ` (with space) matches "Rune of Fire" but won't match "Runestone". Without the space, `Rune of` would still work but could theoretically match "Rune offering" if such an item existed.

### Disable All Wildcards

Comment out or remove every line in `[Wildcards]`. TurboLoot will only loot items in your `[ItemLimits]` or caught by value thresholds.

---

## Quick Reference

| What you want | What to do |
|---|---|
| Disable a built-in wildcard | `Song:=OFF` |
| Add a custom wildcard | `Wildcard1=Rune of ` |
| Remove a custom wildcard | Delete the line or comment it out with `;` |
| Disable ALL wildcards | Comment out everything in `[Wildcards]` |
| Check what's loaded | Set `debug=ON` and run the macro |
| Multiple looters | Each INI (`turboloot.ini`, `turboloot2.ini`) has its own `[Wildcards]` section |

---

## Notes

- Wildcards for looting match the **beginning** of item names only (prefix matching).
- **Performance:** After `[Wildcards]` is loaded, TurboLoot builds a tiny first-letter index so items whose names **cannot** match any configured prefix skip the full wildcard walk
- Matching is **case-insensitive** - `Spell:` matches "SPELL: Gate" and "Spell: Gate".
- Items explicitly listed in `[ItemLimits]` always take priority over wildcard matching. If you set `Spell: Gate=IGNORE` in ItemLimits, it will be ignored even though it matches the `Spell:` wildcard.
- The `bankWildcards` and `sellWildcards` settings in `[Settings]` still control what happens to wildcard-matched items during bank/sell operations.

---

## 10. Tips

- **Start simple.** Don't fill out the whole INI upfront if you're overwhelmed. Just set your `lootHighValueMinPP` threshold and use TurboKey to categorize items as you encounter them.
- **Use `/mac turboloot report` first.** Before your first `/mac turboloot sell` or `/mac turboloot unload`, run the report to preview what would be sold. Better safe than sorry.
- **`announceDefaultTo=e3bc`** is great for multiboxing - all your characters see loot announcements in the MQ window.
- **`announceSkipTo=gsay`** lets your group see what's being left on corpses, handy for anyone who wants to grab something. Also good to trigger lazbis.
- **`IGNORE` is your friend** for spammy items you never want to see again (Bone Chips in your 50s, etc.).
- **`/mac turboloot unload` is the town command.** Park near a banker, tribute master, and vendor, then run it to handle bank -> tribute -> sell -> destroy all at once.
- **`bankWildcards=ON`** and **`sellWildcards=ON`** means spell scrolls, tomes, and skill-ups get auto banked or sold unless you've given them a different rule - great for alts.
- **`logToFile=ON`** appends forever until you clear/rename the log (`/mqlog clear` in macro context, or delete `Logs/TurboLoot.mac.log`). Use only when encountering issues and send me the log for help.

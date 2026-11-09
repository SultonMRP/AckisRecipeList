# Ackis Recipe List — agent notes

Folder: `D:\Warcrafts\SingleWoW\Interface\AddOns\AckisRecipeList`

Client: 3.3.5a (`## Interface: 30300`). Cross-ref: `D:\Warcrafts\SingleWoW\Interface\AddOns\agent-global.md`

---

## What it is

Profession recipe tracker (ARL). Main window lists recipes; hover shows two tooltips:

1. **Acquire tooltip** (`LibQTip`, `acquire_tip`) — skill level, BOP/BOE, trainers, vendors, coords, Alt/Ctrl/Shift hints.
2. **Spell tooltip** (`AckisRecipeList_SpellTooltip`) — reagents, requires, item use text.

Code: `Interface\List.lua`. Defaults in `core.lua`: both `spelltooltiplocation` and `acquiretooltiplocation` are `"Right"`.

---

## Session changes (2026-11-09)

### Tooltips no longer overlap

TipTac was calling `AddModifiedTip(spell_tip)` **without** the restyle-only flag, so it moved the spell tip on top of the acquire tip.

- `AddModifiedTip(spell_tip, true)` — restyle only, do not let TipTac move it.
- `PlaceSpellTipNextToAcquire` — prefer **right of** the acquire tip; if that would leave the screen, **below**; if no room below, **above**.

### Tooltip trainer list is player-faction only

`GetTipFactionInfo` used to honor `filters.general.faction` (include Alliance **and** Horde). That made Catseye Elixir-style lists huge.

Now:

- Show **player faction** + **Neutral**
- Never list the opposite faction on the hover tooltip

Scan/list filters are unchanged; this is tooltip display only.

---

## Do not

- Pass `true` as the second arg to TipTac on the acquire tip already (OnHide hook used to C-stack overflow). Keep that.
- Re-enable opposite-faction trainers on the tooltip unless the user asks.
- Use `SetClampedToScreen(true)` on the spell tip if it fights the manual beside/below placement.

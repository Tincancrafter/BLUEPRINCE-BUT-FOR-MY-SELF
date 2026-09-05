# Red Prince 1.0.13

## September 5 hotfix

- Fixed the game plugin reporting version 1.0.12 after installing 1.0.13.
- Fixed received-item replay stopping on a missing AP item effect, most visibly
  when receiving the Battery Pack after resetting or reloading a save.
- A newly hosted Archipelago room that reuses the same generated seed no longer
  receives cached checks from the previous room. Normal reconnect recovery is
  unchanged when the server still contains checked locations.
- Fixed generation failing when Archipelago's early-item pass had already filled
  one of the two Bunk Room entrance checks. The second check now receives a
  matching item as intended.

## Gameplay and logic

- Receiving the Gemstone Caverns or Blackbridge progression unlock now opens
  the corresponding area without also requiring its vanilla puzzle.
- Starting-room selections now always include at least one path room.
- Added explicit early-drafting access logic for the Garage, Tunnel,
  Foundation, and Secret Passage.
- Paper Crown logic now accounts for its five-coin cost through the renewable
  Coin Purse route and sufficient reachable rooms.
- Fixed exhausted unique rooms remaining in draft picker arrays and appearing
  again after their allowed copies had already been drafted.

## Commissary upgrade disk

- The Commissary upgrade disk is an Archipelago location and item only when
  Special Shop Sanity is enabled.
- Its logic requires the Coin Purse as a renewable source for the purchase.
- When Special Shop Sanity is disabled, the disk retains its vanilla behavior.

## Performance

- Removed the experimental unchecked-room draft border/highlight and its
  recurring scene-wide TextMeshPro scan. The release contains no border or
  highlight feature code.

## Compatibility

- Archipelago 0.6.7 or newer.
- The 1.0.13 game plugin and 1.0.13 APWorld should be distributed together.

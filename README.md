# Astral Drift - Android app

A working single-player slice of the idle MMORPG. The real engine runs on-device,
ticking once a second so skills level, loot drops, and Idle Essence accrue live -
and now it **saves**: close the app and reopen it, and your progress (plus
everything you earned while away) is restored.

## Project layout
```
AstralDrift/
- settings.gradle.kts        modules: :app and :engine
- build.gradle.kts           plugin versions
- engine/                    pure-Kotlin engine + backend (reusable, no Android deps)
- app/                       Android app (Compose + Room)
  - data/AppDatabase.kt      Room: one-row JSON save
  - game/SaveState.kt        serializable save payload
  - game/GameRepository.kt   load / offline catch-up / save
  - ui/GameViewModel.kt      idle loop + autosave + UI state
  - MainActivity.kt          the screen
```

## How to run
1. Install the latest **Android Studio** (bundles the right JDK + SDK tools).
2. **File -> Open** this `AstralDrift` folder; let Gradle sync.
3. If Studio offers to update AGP / Kotlin / Gradle, **accept all together** (they must stay compatible).
4. Accept any SDK install prompt (compileSdk 34).
5. Create an emulator or attach a phone, then press **Run**.

You should see skills leveling and inventory filling within seconds. Background the
app and reopen it - it reloads your save and shows a "Welcome back" note crediting
the offline time. "Skip 1 minute" fast-forwards so you can see it without waiting.

## Persistence behaviour
- **Autosave** every 15 seconds, on action change, and when the app is backgrounded.
- **Offline catch-up** on launch: Idle Essence is banked from the saved timestamp to
  now (real-time, uncapped), and offline action progress is applied (capped at 12h by
  the engine). The save is one JSON blob (`SaveState`) stored in Room.

## Version note (important, and normal)
Known-compatible set as written:
`AGP 8.5.2 - Kotlin 1.9.24 - KSP 1.9.24-1.0.20 - Compose Compiler 1.5.14 -
Compose BOM 2024.06.00 - Room 2.6.1 - Gradle 8.7`.
Two hard rules if you update anything: the **Kotlin version must match both the
Compose Compiler version and the KSP version** (KSP is `<kotlin>-<ksp>`, e.g.
`1.9.24-1.0.20`). If Studio pushes updates, let it bump them as a set.

This project wasn't compiled in the environment it was written in, so the first
Gradle sync is the real test. If it errors, it's almost always a one-line version
or import fix - send the exact message.

## Still to come
- **Full UI** beyond this one screen (skills list, equipment, essence spending).
- **Multiplayer** (trade/market) - needs the backend in `engine/.../server/` deployed to a server.

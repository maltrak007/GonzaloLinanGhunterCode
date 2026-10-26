# Ghunter — Enemy AI & Combat Code

> **Best University Game** at BIG Conference Bilbao · **Second Place** at Guerrilla Games Festival

**Ghunter** is an action-adventure *cooking* game: uncover the mysteries of Garcosa, hunt your prey and save the King with your dishes. This repository contains the **gameplay code I wrote as AI Programmer & Combat Designer**: the enemy roster, the Behavior Tree building blocks, group coordination and the environmental hazards that shape every fight.

[▶ Official Trailer](https://youtu.be/yjipXpGEbDo) · [Full team project](https://github.com/IsFriskis/ghuntercode)

| | |
|---|---|
| **Engine** | Unreal Engine 5 |
| **Language** | C++ (Blueprints and Behavior Tree graphs not included here) |
| **Systems** | Gameplay Ability System, Behavior Trees, AI Perception, EQS, Motion Warping |
| **Role** | AI Programmer & Combat Designer |

> This is a **code-only extract** (`.h` / `.cpp`). Content assets, Blueprints and Behavior Tree graphs live in the full team repository, so this repo is meant to be read, not built standalone. Class names use the project's internal module name `TINYTAVERNGAME`.

## What's inside

```
Character/            Base character, combat interface, anim notifies
  Enemy/
    EnemyBaseClass/   EnemyBase, AI controller, Group, Burrow, Corpse, IA_EnemyManager
    Basilisco/        Ranged enemy: poison-spit projectile + dedicated AI controller
    Cucafera/         Group-based enemy (CucaferaGroup)
    Gamusino/         Burrowing / evasive enemy with its own group + controller
Hazards/              GuardianWall, HideGrass, TallGrass, PuffBall
IA/
  Tasks/              Behavior Tree tasks (BTT_*)
  Services/           Behavior Tree services
  Decorators/         Tag-based decorators
  MIscelaneous/       Waypoints, enemy reset triggers
```

## Highlights

### Modular enemy architecture
Every enemy derives from `AEnemyBase`, so shared behaviour (stats, tags, death and corpse handling, burrowing) is written once and each variant only adds what makes it unique. Variants are cheap to create, which allowed fast difficulty iteration during playtests.

### Reusable Behavior Tree library
Behavior is built from small, composable nodes instead of per-enemy monoliths:

- **Tasks:** `BTT_Patrolling`, `BTT_IdleWander`, `BTT_Chase_Basilisco`, `BTT_Chase_Gamusino`, `BTT_Charge`, `BTT_WaveAttack`, `BTT_PoisonSpit`, `BTT_Peck`, `BTT_Flee`, `BTT_FalseFlee`, `BTT_DesesperatelyFlee`, `BTT_ErraticDodge`, `BTT_Laugh`, `BTT_RollDice`, `BTT_ModifyAttribute`, `BTT_PlayAnimation`
- **Services:** `GetPlayer`, `CheckAttributes`, `CheckAttributeAndFinishNode`, `DetectPlayerFromWall`, `RestoreWalls`, `AddTagToEndNode`, `RemoveTagToEndNode`
- **Decorators:** `CheckTag`, `CheckTagDecorator`, `OnEnd_AddTag`, `OnEnd_RemoveTag`

Gameplay tags are the shared language between abilities, decorators and tasks, which keeps trees readable and puts designers in control.

### Group coordination
`UIA_EnemyManager` is a `UGameInstanceSubsystem` that tracks every `AEnemyGroup` in the world, while `CucaferaGroup` and `GamusinoGroup` specialise group behaviour per species.

### Environment-aware combat
Fights are shaped by the level, not just by stats: `TallGrass` / `HideGrass` conceal enemies, `GuardianWall` is detected and restored through dedicated services, and `PuffBall` is an interactive hazard. Custom nodes read these hazards and enemy attributes to decide what to do next.

### Enemy variants that test the player
- **Basilisco** — keeps its distance and spits poison (`PoisonSpitProjectile`).
- **Cucafera** — fights as a coordinated group.
- **Gamusino** — burrows (`GamusinoBurrow`), flees, fakes fleeing and dodges erratically.

## My contributions

- Architected modular AI on top of GAS for scalable enemy abilities and behaviours.
- Implemented Behavior Trees and AI Perception for reactive engagement.
- Built custom behaviour nodes influenced by environmental hazards and enemy stats.
- Created a task library that allows rapid iteration on enemy variants.
- Tuned combat difficulty from structured playtesting (behaviours, timing windows, stat scaling).

## Author

**Gonzalo Liñán Aldana** — Gameplay Programmer & Technical Designer · [GitHub](https://github.com/maltrak007) · [LinkedIn](https://www.linkedin.com/in/gonzalo-liñan-aldana-934283221/)

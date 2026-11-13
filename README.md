# SplatLab: Fold & Swat

A reflex game for iPhone, built for the foldable iPhone Duo. Unfold the phone: one half shows a scene with flies buzzing around it, the other half shows your weapon. When a fly lands, fold the phone shut to slam the weapon down on it. On any other iPhone, swipe the weapon across the scene instead.

- **8 scenes**: Night Meal, Breakfast, Picnic, Desk, Backyard BBQ, Birthday Party, Fruit Market, Movie Night
- **7 weapons**, each playing differently: Hardcover, Electric Swatter, Morning Paper, Cast-Iron Pan, Flip-Flop, Classic Swatter, Glue Trap
- **3 lives** per run, with flies getting faster after every kill
- **Pairs**: squash two flies in one slam to win back a life
- **Combos** of up to 5× for consecutive hits
- Procedural stereo audio and haptics, with no ads, accounts, tracking or network access

## Requirements

| | |
|---|---|
| Xcode | 27.2 or later |
| Deployment target | iOS 18.0 |
| Fold detection via the hinge | iOS 27.1 or later on iPhone Duo (`iPhone19,4`) |
| Devices | iPhone only |
| Bundle ID | `io.axon86.splatlab` |

There are no third-party dependencies.

## Getting started

1. Open `splatlab.xcodeproj` in Xcode.
2. Select the `splatlab` scheme and an iPhone simulator or device.
3. Press Run.

To try the fold gameplay, use the **iPhone Duo** simulator and unfold it in Device Hub. On any other simulator the game uses swipe-to-slam.

Command-line build:

```sh
xcodebuild -project splatlab.xcodeproj -scheme splatlab \
  -destination "platform=iOS Simulator,name=iPhone Duo" build
```

## Running tests

```sh
xcodebuild test -project splatlab.xcodeproj -scheme splatlab \
  -destination "platform=iOS Simulator,name=iPhone 18 Pro Max" \
  -only-testing:splatlabTests
```

The unit tests cover scoring, the difficulty curve, pair chance, weapon trade-offs, scene landing spots and the panel layout.

## How fold detection works

- **Posture** comes from the window size. A window whose short side is at least 560 points can only be an unfolded foldable (`FoldSupport`).
- **The fold itself** is detected through the hinge-angle API on iOS 27.1 and later (`HingeObserver`). Older systems fall back to CoreMotion, which watches for one half of the phone swinging around the hinge (`MotionFoldDetector`).
- **Layout**: when unfolded, the weapon sits on the left and the scene on the right by default. Players can switch sides in Settings or from the pause menu.
- **Swipe-to-slam** works on every iPhone and can be turned off on foldables.

## Project structure

```
splatlab/
├── App/        App entry, AppModel (routing, posture), RootView, debug launch options
├── Core/       Fold and hinge detection, sound engine, haptics, saved profile
├── Game/       GameController (rules, lives, scoring), FlyField and FlyNode (SpriteKit fly AI)
├── Model/      Arena (scenes), Weapon, FlySpecies, ScoreRules (all tuning numbers)
├── Views/      SwiftUI screens: Home, Game, Folded, Game Over, How to Play, Settings, Theme
└── PrivacyInfo.xcprivacy
splatlabTests/  Unit tests
AppStore/       App Store listing assets (not included in the app)
```

## Tuning difficulty

Every gameplay number lives in `splatlab/Model/ScoreRules.swift`:

| Setting | What it controls |
|---|---|
| `startingLives` | Lives per run (3) |
| `warmUpKills`, `postWarmUpSitFactor` | Easy first few kills, then 15% shorter sit times |
| `sitTimeFactor(afterKills:)`, `minSitTime` | How fast landed flies get twitchier, and the floor |
| `alertnessFactor(afterKills:)` | How quickly flies react to a coming strike |
| `pairChance(afterKills:)`, `pairBonus` | How often pairs appear, and their reward |
| `comboMultiplier`, `maxComboMultiplier` | Combo scoring |

Each weapon's trade-offs (dodge factor, points multiplier, mid-air reach, combo step) are in `Weapon.swift`.

## Debug launch options

These options are only compiled into Debug builds and are never shipped. Pass them as launch arguments in the scheme, or with `xcrun simctl launch <device> io.axon86.splatlab <args>`.

| Argument | Effect |
|---|---|
| `-SLSkipTutorial` | Skip the first-run tutorial |
| `-SLAutoPlay <scene> <weapon>` | Start a game straight away, e.g. `-SLAutoPlay barbecue pan` |
| `-SLSingleScreen` | Play on the current screen (swipe mode) |
| `-SLSideBySide` | Force the unfolded side-by-side layout |
| `-SLUnfolded` | Treat the phone as unfolded |
| `-SLLandscape` | Rotate the app to landscape |
| `-SLAutoSlam` | A bot slams automatically when a fly lands |
| `-SLLives <n>` | Start with `n` lives |
| `-SLFlap <0…1>` | Freeze the weapon panel partway through its swing |

Scene names: `nightMeal`, `breakfast`, `picnic`, `desk`, `barbecue`, `party`, `market`, `movieNight`.
Weapon names: `book`, `zapper`, `newspaper`, `pan`, `flipFlop`, `swatter`, `glueTrap`.

## Privacy

SplatLab collects no data. Progress and settings are stored on the device with `UserDefaults`, declared in `PrivacyInfo.xcprivacy` with reason `CA92.1`. The app makes no network requests and contains no third-party SDKs. See `AppStore/privacy-policy.html`.

## App Store

`AppStore/` holds everything for the App Store listing:

- `metadata.md`: name, subtitle, promotional text, description, keywords, age rating and review notes
- `Screenshots/`: iPhone Duo (2853×2007), 6.3-inch (1206×2622) and 6.9-inch (1320×2868) sets
- `Header/`: header artwork
- `support.html`, `privacy-policy.html`, `index.html`: pages to host for the Support and Privacy Policy URLs

## License

© 2026 Humza Sami Chughtai. All rights reserved.

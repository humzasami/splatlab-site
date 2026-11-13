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


## Privacy

SplatLab collects no data. Progress and settings are stored on the device with `UserDefaults`, declared in `PrivacyInfo.xcprivacy` with reason `CA92.1`. The app makes no network requests and contains no third-party SDKs. See `AppStore/privacy-policy.html`.

## License

© 2026 Humza Sami Chughtai. All rights reserved.

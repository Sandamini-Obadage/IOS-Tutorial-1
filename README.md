# Tap Frenzy

Tap Frenzy is a SwiftUI game for iPhone on iOS 17 or later. The first tap starts a 10-second round and scores one point. Tap as fast as you can, then compare your result with your best score. The app saves the best score and completed round count across launches.

## Architecture

```text
TapFrenzy/
├── App/
│   └── TapFrenzyApp.swift
└── Features/
    └── Game/
        ├── Model/
        │   └── GameRound.swift
        └── Views/
            ├── GameView.swift
            ├── GameTheme.swift
            ├── RoundTimerView.swift
            └── TapControl.swift
Tests/
└── GameRoundChecks.swift
```

The code is organized by feature. `GameRound` is a Swift value type with idle, playing, and game-over states. It takes timestamps as inputs, so the round checks can exercise the deadline without waiting 10 seconds. `GameView` owns the current round through `@State`, saves completed-round statistics through `@AppStorage`, and passes state and actions to its child views. The timer checks a monotonic deadline on every scoring tap, so a delayed screen refresh cannot award a late point.

## Visual theme

The game uses a dark arcade theme with a deep navy background, slate panels, and a mint-green action button. The timer moves from mint to amber to coral-red while the countdown stays visible in text. Rounded score numerals, SF Symbols, and a soft glow around the button carry the arcade style. The palette lives in `GameTheme.swift`.

Two stretch features are implemented from the tutorial brief. The score animates when it changes, and the timer changes from mint to amber to red. The course's separate section 05 challenge list still needs to be checked against these features.

## Build from Windows

1. Open the repo folder in VS Code and push it to a private GitHub repository.
2. In a personal Codemagic account, add the GitHub repository, then choose **Start new build**.
3. Select the branch you pushed and the **Tap Frenzy iOS Build** workflow. The workflow uses a Mac mini M2 and builds for the iOS Simulator with signing disabled.
4. Check for `Round checks passed` in the scoring and timing step and `** BUILD SUCCEEDED **` in the compile log. Match the commit shown by Codemagic to your submission commit.

Codemagic can regenerate `TapFrenzy.xcodeproj` from `project.yml` when needed. A successful build offers `TapFrenzy-Xcode.zip` in **Artifacts**, containing the project and its sources. The native project is committed in this repository, so it can be opened directly after cloning. If you later add or relocate source files, regenerate the project or update its file references in Xcode.

## Run in a Windows browser

Download `TapFrenzy-Simulator.zip` from the latest successful Codemagic build's **Artifacts**. It contains the compiled ARM64 iOS Simulator bundle at `TapFrenzy.app/`. Upload the ZIP directly to [Appetize](https://appetize.io/upload) and launch an iPhone running iOS 17 or later. Use the checklist below for the gameplay checks available in your simulator session.

## Run and check in Xcode

On a Mac with Xcode 15 or newer and an iOS 17 or newer simulator installed:

```sh
git clone https://github.com/Sandamini-Obadage/IOS-Tutorial-1.git
cd IOS-Tutorial-1
open TapFrenzy.xcodeproj
```

Select the shared **TapFrenzy** scheme and an iPhone simulator, then press **⌘R**. The `.xcodeproj` is committed, so cloning is enough; XcodeGen and the Codemagic artifact are not needed for a local build. To run on a physical iPhone, select your development team in the TapFrenzy target's **Signing & Capabilities** tab and use a bundle identifier available to your team.

`project.yml` is the XcodeGen source for CI and uses the same Xcode 15 project format. After source or build-setting changes, XcodeGen 2.45 or newer can regenerate the project with `xcodegen generate`.

- Confirm the first tap starts the timer and scores one point. Tap rapidly and watch the score increase.
- Let the timer expire. Check the final score, best score, and completed round count. Tap **Try again** to start a fresh round.
- Beat your previous best, then complete a lower-scoring round. Force-quit and reopen the app. Check that the best score and round count persist.
- Rotate during a round. Check that the score, timer, and button remain usable.
- Enable Reduce Motion and check that the score and button can be used without their animations.

# PeaceSet

A wellness-focused iOS app that helps people reduce compulsive phone use by enforcing app and website access rules using Apple's Screen Time APIs and presenting insights that combine usage signals with HealthKit signals into a single wellness score.

## Features

- **Smart Restrictions**: Block distracting apps and websites during focus times
- **Wellness Score**: Daily score combining sleep, HRV, and focus metrics
- **Privacy-First**: All data stays on your device
- **Beautiful UI**: Clean, calming interface designed for wellness

## Screenshots

*Coming soon*

## Requirements

- iOS 16.0+
- Xcode 15.0+
- Node.js 18+
- Ruby 3.0+ (for CocoaPods)
- Physical iPhone (Screen Time APIs require a device)
- Apple Developer account with Family Controls entitlement

## Setup

### 1. Clone and Install Dependencies

```bash
cd app
npm install
cd ios
bundle install
bundle exec pod install
```

### 2. Configure Xcode

1. Open `ios/PeaceSetApp.xcworkspace` in Xcode
2. Select the main app target and configure signing
3. Add capabilities:
   - Family Controls
   - HealthKit
   - App Groups (set to `group.com.peaceset.shared`)
4. Configure signing for all extension targets
5. Link PeaceSetShared package to all targets

### 3. Request Family Controls Entitlement

Screen Time APIs require Apple's approval. Request the Family Controls entitlement at:
https://developer.apple.com/contact/request/family-controls-distribution

### 4. Run on Device

```bash
cd app
npx react-native run-ios --device
```

Or build and run from Xcode (Cmd+R).

## Project Structure

```
/PeaceSet
├── app/                      # React Native app
│   ├── src/
│   │   ├── core/            # Config, logging, constants
│   │   ├── domain/          # Types, scoring algorithm
│   │   ├── data/            # Repositories, persistence
│   │   ├── native/          # Native module wrappers
│   │   ├── features/        # Feature modules
│   │   ├── ui/              # Shared components
│   │   ├── navigation/      # React Navigation
│   │   └── store/           # Zustand stores
│   └── ios/                 # iOS project
├── ios/
│   ├── Packages/            # Swift Package (PeaceSetShared)
│   ├── PeaceSet/            # Main app
│   └── Extensions/          # iOS extensions
└── docs/                    # Documentation
```

## Architecture

See [docs/architecture.md](docs/architecture.md) for detailed architecture documentation.

## Privacy

See [docs/privacy.md](docs/privacy.md) for our privacy policy.

## Wellness Disclaimer

PeaceSet is a wellness tool designed to help you build healthier digital habits. It does not diagnose, treat, cure, or prevent any medical or mental health condition.

If you have concerns about your mental health, please consult a qualified healthcare professional.

## Tech Stack

- **React Native** (TypeScript) - Cross-platform UI
- **Swift 5.9+** - Native iOS modules and extensions
- **Zustand** - State management
- **SQLite** - Local database
- **React Navigation** - Navigation

## Running Tests

```bash
cd app
npm test
```

## License

*TBD*

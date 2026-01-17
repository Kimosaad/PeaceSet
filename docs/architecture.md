# Omit Architecture

## Overview

Omit is a wellness-focused iOS app that helps users reduce compulsive phone use through Screen Time enforcement and provides insights via a wellness score combining HealthKit and usage data.

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     React Native Layer                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │   UI/UX     │  │   Domain    │  │     Native Wrappers     │  │
│  │  (Screens)  │  │  (Scoring)  │  │  (ScreenTime, HealthKit)│  │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Swift Native Layer                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │ScreenTime  │  │ HealthKit   │  │   PeaceSetShared        │  │
│  │  Bridge    │  │   Bridge    │  │   (Swift Package)       │  │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                     iOS Extensions                              │
│  ┌───────────────┐  ┌─────────────┐  ┌───────────────────────┐  │
│  │DeviceActivity │  │   Shield    │  │   DeviceActivity      │  │
│  │   Monitor    │  │Configuration│  │      Report           │  │
│  └───────────────┘  └─────────────┘  └───────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

## Key Components

### React Native Layer

- **UI/UX**: Onboarding flow, Home, Restrictions, Analytics, Settings screens
- **Domain**: Wellness scoring algorithm, rule validation
- **Native Wrappers**: TypeScript interfaces for native modules

### Swift Native Layer

- **ScreenTimeBridge**: Manages FamilyControls authorization and DeviceActivity monitoring
- **HealthKitBridge**: Queries sleep and HRV data from HealthKit
- **PeaceSetShared**: Swift Package shared across main app and extensions

### iOS Extensions

- **DeviceActivityMonitorExtension**: Enforces shields on schedule start/end and threshold reached
- **ShieldConfigurationExtension**: Customizes shield UI appearance
- **ShieldActionExtension**: Handles shield button taps
- **DeviceActivityReportExtension**: Renders Screen Time reports

## Data Flow

### Enforcement Flow

1. User creates rule in RN app
2. ScreenTimeBridge saves rule to App Group
3. ScreenTimeBridge starts DeviceActivityCenter monitoring
4. DeviceActivityMonitor receives callbacks
5. ShieldStateEngine computes union of active rules
6. ManagedSettingsStore applies shields

### Score Computation Flow

1. App opens or health data arrives
2. HealthKitBridge queries sleep and HRV data
3. ScreenTimeBridge reads enforcement events
4. TypeScript scoring algorithm computes score
5. Score displayed in Analytics screen

## Storage Architecture

All data stays on device. Storage locations:

- **App Group**: Rules, selections, snoozes, events (JSON/JSONL)
- **SQLite**: Imported events, cached health data, computed scores
- **AsyncStorage**: UI preferences, onboarding state

## Security & Privacy

- All personal data stays on device
- No network calls for health or usage data
- App Group uses file coordination for concurrent access
- Token serialization uses Apple's Codable encoding

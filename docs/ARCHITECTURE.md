# Architecture Guidelines

This document outlines the architectural patterns and best practices to be followed in the Track My Finance app, inspired by the Now In Android app architecture.

## Overview

The app follows Clean Architecture principles with MVVM pattern, organized in the following layers:

```
app/
├── ui/          # Presentation layer (Compose UI, ViewModels)
├── domain/      # Business logic and interfaces
├── data/        # Data handling and implementations
└── di/          # Dependency injection modules
```

## Key Components

### UI Layer
- UI State holders (ViewModels)
- UI Elements (Composables)
- State management
- UI business logic

### Domain Layer
- Business logic
- Domain models
- Repository interfaces
- Use cases

### Data Layer
- Repository implementations
- Data sources
- API integration
- Local persistence
- Mappers

## Dependency Flow
```
UI Layer → Domain Layer ← Data Layer
```

## Component Dependencies
- UI Layer can only depend on Domain Layer
- Domain Layer has no dependencies on other layers
- Data Layer can only depend on Domain Layer

## Module Organization
Each feature module should follow this structure:
```
feature_name/
├── ui/
│   ├── screens/
│   │   └── FeatureScreen.kt          # Compose UI
│   ├── components/                    # Reusable UI components
│   ├── FeatureViewModel.kt
│   ├── FeatureState.kt
│   ├── FeatureEvent.kt
│   └── FeatureEffect.kt
├── domain/
│   ├── model/
│   │   └── FeatureEntity.kt
│   ├── repository/
│   │   └── FeatureRepository.kt
│   └── usecase/
│       └── FeatureUseCase.kt
└── data/
    ├── repository/
    │   └── FeatureRepositoryImpl.kt
    ├── remote/
    │   ├── api/
    │   │   └── FeatureApi.kt
    │   ├── model/
    │   │   └── FeatureApiModel.kt
    │   └── mapper/
    │       └── FeatureApiMapper.kt
    └── local/
        ├── model/
        │   └── FeatureEntity.kt
        └── mapper/
            └── FeatureEntityMapper.kt
```

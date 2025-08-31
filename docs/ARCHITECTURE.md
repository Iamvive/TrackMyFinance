# Project Architecture

This document outlines the architecture for Track My Finance.

## Overview

Track My Finance employs a Clean Architecture with the MVVM pattern, featuring clear separation of layers and modules.

## Diagram

![Architecture Diagram](architecture-diagram.png)

_A placeholder diagram is provided; update with an actual diagram as the project evolves._

## Layers

- **Presentation Layer:** UI, ViewModels
- **Domain Layer:** Use cases, business logic
- **Data Layer:** Repositories, data sources (API, DB)

## Module Organization

- `app/` - App entry, DI, navigation
- `domain/` - Business models, use cases
- `data/` - API, persistence, repositories

## Dependencies

- Presentation → Domain → Data
- No reverse dependencies

_See more in [UI Guidelines](UI_GUIDELINES.md) and [Data Guidelines](DATA_GUIDELINES.md)._
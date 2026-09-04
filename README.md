# 🗂️ Project Structure

> **Architecture:** Feature-Driven Clean Architecture  
> **State Management:** Riverpod (`flutter_riverpod`)  
> **Local Database:** Isar (NoSQL, code-generated)  
> **Backend:** Firebase (Auth · Firestore · Storage · App Check)

---

Each feature is fully self-contained and separated into three horizontal layers — **data**, **domain**, and **presentation** — following Clean Architecture principles. Cross-feature concerns live in the shared `core/` module.

---

```
daftar_altrzy/
├── pubspec.yaml                        # Dependencies & asset declarations
├── analysis_options.yaml               # Dart linter configuration
├── firestore.rules                     # Firestore security rules
├── storage.rules                       # Firebase Storage security rules
│
└── lib/
    ├── main.dart                       # App entry point; initialises Firebase & Isar
    ├── app.dart                        # Root widget; theme & routing setup
    │
    ├── core/                           # ── Shared cross-feature infrastructure ──
    │   ├── theme.dart
    │   ├── animations/
    │   │   └── app_animations.dart     # Reusable transition & animation helpers
    │   ├── constants/
    │   │   └── app_strings.dart        # Centralised localised string constants
    │   ├── error/                      # (reserved) Global error/failure types
    │   ├── firebase/
    │   │   └── firebase_options.dart   # Auto-generated Firebase platform config
    │   ├── providers/
    │   │   ├── firebase_providers.dart # Riverpod providers for Firebase services
    │   │   ├── notification_provider.dart
    │   │   └── pdf_provider.dart
    │   ├── services/
    │   │   └── notification_service.dart  # Local push-notification scheduling
    │   ├── theme/
    │   │   ├── app_colors_v2.dart      # Design-token colour palette
    │   │   ├── app_colors_compat.dart  # Legacy colour compatibility shim
    │   │   ├── app_theme.dart          # Material ThemeData (light/dark)
    │   │   ├── app_typography.dart     # Text-style scale & font configuration
    │   │   └── receipt_theme.dart      # Specialised receipt/print theme
    │   ├── utils/
    │   │   ├── date_formatter.dart     # Shared date formatting utilities
    │   │   ├── pdf_service.dart        # PDF generation & export helper
    │   │   └── result.dart             # Functional Result<T> type for error handling
    │   └── widgets/
    │       ├── app_button.dart         # Branded primary button component
    │       ├── app_loaders.dart        # Loading indicators & skeleton screens
    │       ├── app_snackbar.dart       # Unified snackbar / toast helper
    │       ├── measurement_field.dart  # Measurement input field widget
    │       ├── measurement_field_v2.dart
    │       ├── section_card.dart       # Reusable section card container
    │       └── section_card_v2.dart
    │
    └── features/                       # ── Feature modules (Clean Architecture) ──
        │
        ├── splash/                     # App launch & initialisation flow
        │   └── presentation/
        │       └── screens/
        │           └── splash_screen.dart
        │
        ├── clients/                    # Core client-management feature
        │   ├── data/                   # Data layer: models, sources & repository impl
        │   │   ├── data.dart
        │   │   ├── mappers/
        │   │   │   ├── client_mapper.dart   # Entity ↔ Isar model conversion
        │   │   │   └── mappers.dart
        │   │   ├── models/
        │   │   │   ├── client.dart          # Isar-annotated client model
        │   │   │   ├── client.g.dart        # Generated Isar adapter
        │   │   │   ├── measurement.dart     # Isar-annotated measurement model
        │   │   │   ├── measurement.g.dart
        │   │   │   ├── order.dart           # Isar-annotated order model
        │   │   │   ├── order.g.dart
        │   │   │   └── models.dart
        │   │   ├── repositories/
        │   │   │   ├── client_repository_impl.dart
        │   │   │   └── repositories.dart
        │   │   ├── services/
        │   │   │   ├── client_export_service.dart  # PDF/image export logic
        │   │   │   └── image_export_service.dart
        │   │   └── sources/
        │   │       ├── client_local_data_source.dart       # Abstract data source
        │   │       ├── client_local_data_source_impl.dart  # Isar implementation
        │   │       └── sources.dart
        │   ├── domain/                 # Domain layer: entities, contracts & use cases
        │   │   ├── entities/
        │   │   │   ├── client_entity.dart
        │   │   │   ├── measurement_entity.dart
        │   │   │   ├── order_entity.dart
        │   │   │   └── entities.dart
        │   │   ├── repositories/
        │   │   │   ├── client_repository.dart  # Abstract repository contract
        │   │   │   └── repositories.dart
        │   │   └── usecases/
        │   │       ├── add_client_usecase.dart
        │   │       ├── delete_client_usecase.dart
        │   │       ├── get_client_usecase.dart
        │   │       ├── get_clients_usecase.dart
        │   │       ├── search_clients_usecase.dart
        │   │       ├── update_client_usecase.dart
        │   │       └── usecases.dart
        │   └── presentation/           # Presentation layer: UI, state & widgets
        │       ├── providers/
        │       │   ├── client_list_provider.dart
        │       │   ├── client_details_provider.dart
        │       │   ├── client_form_provider.dart
        │       │   ├── client_export_provider.dart
        │       │   ├── client_repository_provider.dart
        │       │   ├── search_provider.dart
        │       │   └── providers.dart
        │       ├── screens/
        │       │   ├── home_screen.dart
        │       │   ├── client_details_screen.dart
        │       │   └── client_form_screen.dart
        │       └── widgets/
        │           └── receipt_card.dart
        │
        ├── debts/                      # Financial debt tracking feature
        │   ├── data/                   # Data layer: Isar models & repository impl
        │   │   ├── data.dart
        │   │   ├── mappers/
        │   │   │   └── debt_mapper.dart
        │   │   ├── models/
        │   │   │   ├── debt_entry.dart
        │   │   │   └── debt_entry.g.dart
        │   │   ├── repositories/
        │   │   │   └── debt_repository_impl.dart
        │   │   └── sources/
        │   │       ├── debt_local_data_source.dart
        │   │       └── debt_local_data_source_impl.dart
        │   ├── domain/                 # Domain layer: entity & repository contract
        │   │   ├── entities/
        │   │   │   └── debt_entity.dart
        │   │   └── repositories/
        │   │       └── debt_repository.dart
        │   └── presentation/           # Presentation layer: UI screens & Riverpod state
        │       ├── providers/
        │       │   └── debt_providers.dart
        │       ├── screens/
        │       │   ├── debt_screen.dart
        │       │   └── debt_archive_screen.dart
        │       └── widgets/            # (reserved for debt-specific widgets)
        │
        ├── calendar/                   # Appointment & schedule calendar feature
        │   └── presentation/           # View-only layer (data sourced from clients feature)
        │       ├── providers/
        │       │   └── calendar_provider.dart
        │       └── screens/
        │           └── calendar_screen.dart
        │
        └── settings/                   # App settings, backup & cloud sync feature
            ├── data/                   # Data layer: cloud sync & backup services
            │   ├── repositories/       # (reserved)
            │   └── sources/
            │       ├── backup_service.dart
            │       ├── cloud_sync_service.dart
            │       ├── firebase_cloud_storage_adapter.dart
            │       └── sync_engine.dart
            ├── domain/                 # Domain layer (reserved for contracts)
            └── presentation/           # Settings UI & sync state management
                ├── providers/
                │   ├── backup_provider.dart
                │   └── cloud_sync_provider.dart
                └── screens/
                    └── settings_screen.dart
```

---

## 🏛️ Architectural Highlights

| Concern | Approach |
|---|---|
| **Architecture** | Feature-Driven Clean Architecture (Data / Domain / Presentation) |
| **State Management** | [Riverpod](https://riverpod.dev/) — provider-per-use-case pattern |
| **Local Persistence** | [Isar](https://isar.dev/) — high-performance NoSQL with code generation |
| **Remote Backend** | Firebase (Firestore, Auth, Storage, App Check) |
| **Sync Strategy** | Custom `SyncEngine` with a `FirebaseCloudStorageAdapter` |
| **Export** | PDF generation (`pdf` + `printing`) and image capture (`screenshot`) |
| **Notifications** | `flutter_local_notifications` with timezone-aware scheduling |
| **Error Handling** | Functional `Result<T>` type isolating failures from presentation |
| **Mappers** | Dedicated mapper classes decouple domain entities from Isar models |

# Technical Specifications

## Technology Stack

### Frontend
- **Framework**: Flutter 3.32+
- **Language**: Dart 
- **State Management**: flutter_bloc (Cubit pattern)
- **Routing**: go_router
- **UI Components**: Material Design 3

### Backend & Services
- **Authentication**: Firebase Auth / Auth0
- **Database**: Firebase Firestore

### Dependencies
```yaml
# Core Dependencies
flutter_bloc: ^8.1.6
equatable: ^2.0.5
go_router: ^14.2.7

# UI & UX
flutter_svg: ^2.0.10+1
cached_network_image: ^3.4.1
lottie: ^3.1.2

# Development
flutter_lints: ^4.0.0
bloc_test: ^9.1.7
mocktail: ^1.0.4
```

## Architecture

### Design Pattern
- **Architecture**: Clean Architecture + BLoC Pattern
- **State Management**: Cubit-only approach (no Events)
- **Data Layer**: Repository pattern with data sources

### Folder Structure
```
lib/
├── core/
│   ├── constants/
│   ├── errors/
│   ├── network/
│   ├── theme/
│   └── utils/
├── features/
│   └── [feature_name]/
│       ├── data/
│       │   ├── datasources/
│       │   ├── models/
│       │   └── repositories/
│       ├── domain/
│       │   ├── entities/
│       │   ├── repositories/
│       │   └── usecases/
│       └── presentation/
│           ├── cubits/
│           ├── pages/
│           └── widgets/
└── injection_container.dart
```

## Linting Configuration

### analysis_options.yaml
```yaml
include: package:flutter_lints/flutter.yaml

analyzer:
  exclude:
    - "**/*.g.dart"
    - "**/*.freezed.dart"
  errors:
    invalid_annotation_target: ignore

linter:
  rules:
    # BLoC Specific Rules
    bloc:
      - prefer_const_constructors
      - avoid_bloc_in_widgets
      - avoid_cubit_in_widgets

    # Additional recommended rules
    - always_use_package_imports
    - avoid_print
    - prefer_single_quotes
    - require_trailing_commas
    - sort_pub_dependencies
    - unnecessary_await_in_return
    - use_build_context_synchronously
```

### BLoC Cubit Best Practices
1. **Cubit Structure**:
   ```dart
   class FeatureCubit extends Cubit<FeatureState> {
     FeatureCubit(this._repository) : super(FeatureInitial());
     
     final FeatureRepository _repository;
     
     Future<void> performAction() async {
       emit(FeatureLoading());
       try {
         final result = await _repository.getData();
         emit(FeatureSuccess(result));
       } catch (e) {
         emit(FeatureError(e.toString()));
       }
     }
   }
   ```

2. **State Classes**:
   ```dart
   abstract class FeatureState extends Equatable {
     const FeatureState();
     @override
     List<Object?> get props => [];
   }
   
   class FeatureInitial extends FeatureState {}
   class FeatureLoading extends FeatureState {}
   class FeatureSuccess extends FeatureState {
     const FeatureSuccess(this.data);
     final DataModel data;
     @override
     List<Object?> get props => [data];
   }
   class FeatureError extends FeatureState {
     const FeatureError(this.message);
     final String message;
     @override
     List<Object?> get props => [message];
   }
   ```

## Performance Guidelines
- **Bundle Size**: Target < 20MB
- **Startup Time**: < 3 seconds cold start
- **Memory Usage**: < 100MB average
- **Frame Rate**: Maintain 60 FPS

## Security Considerations
- API keys stored in environment variables
- Certificate pinning for network requests
- Biometric authentication support
- Data encryption for sensitive information

## Testing Strategy
- **Unit Tests**: Cubits, repositories, use cases (80%+ coverage)
- **Widget Tests**: UI components and pages
- **Integration Tests**: Complete user flows
- **Golden Tests**: UI consistency across devices

## CI/CD Pipeline
- **Code Quality**: Automated linting and formatting
- **Testing**: Automated test execution
- **Build**: Automated APK/IPA generation
- **Deployment**: Automated distribution to stores

## Development Environment
### Required Tools
- Flutter SDK 3.32+
- Android Studio / VS Code
- Xcode (for iOS development)
- Git

### Setup Commands
```bash
# Install dependencies
flutter pub get

# Generate code
dart run build_runner build

# Run linter
flutter analyze

# Run tests
flutter test

# Build APK
flutter build apk --release
```

---
**Last Updated**: [Date]  
**Flutter Version**: 3.24+  
**Dart Version**: 3.5+
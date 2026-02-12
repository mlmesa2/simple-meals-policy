# SimpleMeals: Technical Overview

## 1. Technology Stack

SimpleMeals is engineered using a modern, scalable Android technology stack, prioritizing performance, user experience, and ease of maintenance.

### Core Technologies
*   **Platform**: Native Android Development.
*   **Language**: Kotlin.
*   **UI Framework**: Jetpack Compose (Modern, declarative UI toolkit).
*   **Architecture Pattern**: MVVM (Model-View-ViewModel) + Clean Architecture.
*   **Dependency Injection**: Hilt (for modularity and testability).
*   **Concurrency**: Kotlin Coroutines & Flow (for responsive background operations).

### AI Integration
The core intelligence of the application is powered by **Google Gemini AI**.
*   **Cloud API**: Leverages the Google Generative AI SDK to process ingredient lists and generate structured recipes.
*   **Computer Vision**: Utilizes multimodal capabilities to analyze images from the device camera, identifying ingredients instantly.

### Monetization & Subscription
*   **RevenueCat**: Implements the subscription infrastructure.
    *   Handles billing logic, receipt validation, and subscription status tracking.
    *   Enables dynamic paywall configuration without app updates.

## 2. Architecture Design

The application follows strictly defined architectural layers to ensure a robust and testable codebase.

### Presentation Layer (UI)
*   **User Interface**: Built with declarative components that react to state changes rather than manual updates.
*   **State Management**: ViewModels act as the bridge between the UI and business logic, holding the screen's state (e.g., loading, error, success) and handling user events.
*   **Lifecycle Awareness**: Components automatically handle Android lifecycle events to prevent memory leaks and crashes.

### Domain Layer (Business Logic)
*   **Use Cases**: Encapsulate the core rules of the application (e.g., "Generate Recipe", "Validate Subscription"). This layer is purely Kotlin and independent of any Android framework or UI details.
*   **Interfaces**: Define contracts for data operations, allowing the business logic to remain agnostic of the actual data source details (e.g., whether data comes from an API or a database).

### Data Layer (Implementation)
*   **Repositories**: Implement the domain interfaces. Responsible for deciding where to fetch data (cloud vs. local storage) and how to cache it.
*   **Data Sources**:
    *   **AI Service**: Handles network requests to the generative AI models.
    *   **Preferences Storage**: Manages local user settings.
    *   **Subscription Service**: Interfaces with the billing SDK to fetch entitlements and process purchases.

## 3. RevenueCat Integration Strategy

The monetization strategy relies on a seamless integration of the RevenueCat platform to manage the "Freemium" model.

### Workflow
1.  **Status Check**: Upon app launch, the system queries the current entitlements to determine if the user is a "Pro" subscriber.
2.  **Feature Gating**:
    *   **Free Users**: Logic enforces a strict daily limit on recipe generations. Access to advanced features is restricted.
    *   **Pro Users**: The validation layer unlocks unlimited access and premium features locally.
3.  **Paywall Presentation**:
    *   When a free user hits a limit or attempts to access a premium feature, a native paywall is displayed.
    *   This UI is dynamically powered by the subscription backend, allowing for A/B testing of pricing and copy.
4.  **Transaction Handling**:
    *   Purchases are processed securely through the Google Play Store via the SDK.
    *   Upon successful transaction, the app immediately receives a status update, creating a "live" unlock experience for the user.

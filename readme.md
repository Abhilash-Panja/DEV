# Angular and TypeScript Concepts Used in the Project

## Angular Concepts

| Angular Concept | Why We Used It |
|---|---|
| Standalone Components | To create components without using traditional NgModules. |
| Component Decorator | To define templates, styles, selectors, and component imports. |
| `bootstrapApplication` | To start the Angular application using the root component. |
| `ApplicationConfig` | To configure application-level providers such as routing. |
| Angular Router | To navigate between login, dashboard, booking, review, and success screens. |
| Route Configuration | To map URLs to Angular components. |
| Lazy Loading with `loadComponent` | To load components only when their routes are visited and improve initial loading performance. |
| `router-outlet` | To display the component that matches the current URL. |
| `RouterLink` | To navigate between pages without refreshing the browser. |
| `RouterLinkActive` | To highlight the currently active navigation link. |
| `Router` Service | To perform navigation programmatically after login, logout, and booking confirmation. |
| Dependency Injection | To provide services such as authentication, appointments, forms, and routing to components. |
| `@Injectable` | To mark classes as Angular services that can be injected. |
| Angular Signals | To store reactive state such as the current user, selected tab, appointments, and messages. |
| Read-only Signals | To allow components to read shared state without directly modifying it. |
| Computed Signals | To derive values such as the next upcoming appointment and filtered booking lists. |
| Angular Template Interpolation | To display dynamic values such as user names, dates, times, and doctor names. |
| Property Binding | To bind values to properties such as classes, attributes, and form groups. |
| Attribute Binding | To dynamically set attributes such as `aria-selected`. |
| Event Binding | To respond to events such as button clicks and form submissions. |
| Angular Control Flow `@if` | To conditionally display content such as user information, appointments, and empty states. |
| Angular Control Flow `@for` | To display multiple upcoming or completed appointments. |
| Reactive Forms | To build structured and controllable login, signup, password-reset, and booking forms. |
| `FormBuilder` | To create form groups and form controls with less code. |
| `FormGroup` | To manage the complete state of each form. |
| Angular Validators | To validate required fields, email format, password length, and checkbox acceptance. |
| Custom Validator | To compare password and confirm-password fields. |
| `markAllAsTouched()` | To show validation messages after an invalid form submission. |
| Component-level Styles | To keep the styles of each page separate and maintainable. |
| Semantic HTML with Angular | To improve accessibility and structure using `main`, `nav`, `aside`, `header`, and `section`. |
| Accessibility Attributes | To support screen readers and keyboard users using ARIA attributes and focus styles. |

## TypeScript Concepts

| TypeScript Concept | Why We Used It |
|---|---|
| Classes | To define Angular components and services. |
| Interfaces | To define the structure of users, appointments, bookings, and role options. |
| Enums | To define fixed user roles such as `Patient` and `Doctor`. |
| Union Types | To restrict values to known options such as `login`, `signup`, `upcoming`, and `completed`. |
| Type Aliases | To create reusable custom types for authentication views and booking tabs. |
| Generic Types | To specify the type of values stored in signals, such as `Signal<Appointment[]>`. |
| Type Annotations | To make method parameters, properties, and return values predictable. |
| Nullable Types | To represent states where data may or may not exist, such as `UserProfile \| null`. |
| Access Modifiers | To control visibility using `private` and `protected`. |
| `readonly` | To prevent properties and model fields from being reassigned accidentally. |
| Constructor Injection | To receive Angular services inside components. |
| Arrow Functions | To write callbacks used by `computed`, `find`, `filter`, `map`, and `update`. |
| Conditional Operator | To create different patient or doctor objects based on the selected role. |
| Object Spread Operator | To copy and update objects without directly mutating the original object. |
| Array Spread Operator | To add appointments while creating a new array. |
| Array Methods | To search, filter, and transform appointment data using `find`, `filter`, and `map`. |
| Optional Null Handling | To safely handle missing users, drafts, and appointments. |
| Modules and Imports | To separate files and reuse components, services, models, and Angular APIs. |
| Type-only Imports/Exports | To use TypeScript types without treating them as runtime JavaScript values. |
| Method Return Types | To clearly define whether methods return a user, an appointment, or nothing. |
| Object Literals | To create user and appointment objects using the required interface structure. |

## Project Services

| Service | Purpose |
|---|---|
| `MockAuthService` | Simulates login, registration, logout, and current-user state. |
| `BookingDraftService` | Temporarily stores booking form data between routes. |
| `AppointmentService` | Stores, adds, filters, and updates confirmed appointments. |
| `Router` | Navigates between application screens. |
| `FormBuilder` | Creates and configures reactive forms. |

## Main Architecture

```text
Angular Components
        ↓
Angular Templates
        ↓
User Interaction and Routing
        ↓
Angular Services
        ↓
Signals and Shared State
        ↓
TypeScript Models and Interfaces
```

## Main Reason for Using These Concepts

The project uses these Angular and TypeScript concepts to achieve:

- Reusable components.
- Clean separation between UI and business logic.
- Type-safe application data.
- Reactive updates when state changes.
- Form validation.
- Client-side navigation.
- Lazy loading.
- Shared authentication and appointment state.
- Better maintainability.
- Better accessibility.
- Easier future connection to a backend API.

## Concepts Not Yet Implemented

The project currently does not yet use:

- Real backend API calls.
- `HttpClient`.
- Authentication guards.
- Route resolvers.
- HTTP interceptors.
- Real database storage.
- Persistent browser storage.
- RxJS Observables for application data.
- NgModules.
- Real password authentication.

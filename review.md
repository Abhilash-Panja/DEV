Your `MockAuthService` is a good example of an Angular service. It is responsible for shared authentication state and user-related operations, while components are responsible mainly for displaying the UI.

Save the following as:

```text
04-angular-services-and-state-management.md
```

```md
# 4. Angular Services and State Management

## What Is an Angular Service?

An Angular service is a reusable TypeScript class that contains logic or data that should be shared between multiple components.

A service is commonly used for:

- Authentication
- API communication
- Shared application state
- Business logic
- Form-related operations
- Notifications
- Logging
- Local storage
- Data transformation
- Appointment and booking management

A component should mainly manage the screen and user interaction.

A service should manage reusable application logic and shared data.

## Component Responsibility vs Service Responsibility

| Component | Service |
|---|---|
| Displays the UI | Stores and manages shared data |
| Handles button clicks | Contains reusable business logic |
| Reads form values | Communicates with APIs |
| Shows validation messages | Manages authentication state |
| Navigates between screens | Manages appointments and bookings |
| Controls page-specific state | Shares data across components |

For example, the login component should not contain all authentication logic directly.

Instead, the component collects the email and selected role, then calls:

```ts
this.mockAuthService.login(loginData.email, selectedRole);
```

The service creates and stores the user.

## Why Do We Need Services?

Without services, every component might contain duplicate logic.

For example, without an authentication service:

- The login page would create the user.
- The dashboard would separately try to find the logged-in user.
- The booking review page would separately try to identify the patient.
- The logout logic could be different in every component.

This would make the application difficult to maintain.

With a service:

```text
Login Component
        ↓
MockAuthService
        ↓
Shared current user
        ↓
Dashboard and Booking Review
```

All components use one central source of truth.

## Dependency Injection

Angular uses Dependency Injection to provide services to components.

The service is registered using:

```ts
@Injectable({
  providedIn: 'root',
})
```

This tells Angular that:

1. The class can be injected into other classes.
2. Angular should make it available throughout the application.
3. Angular should create the service when it is required.
4. The service normally has one shared instance for the application.

A component receives the service through its constructor:

```ts
constructor(
  private mockAuthService: MockAuthService,
) {}
```

The component does not manually create the service using:

```ts
new MockAuthService()
```

Angular creates and provides the service automatically.

## What Does `providedIn: 'root'` Mean?

```ts
@Injectable({
  providedIn: 'root',
})
```

means that the service is registered with the root application injector.

This is useful for services such as:

- Authentication
- User state
- Appointment state
- Shopping cart state
- Notification state
- Application configuration

Because the service is provided at the root level, different components can access the same service instance.

This is important for shared state.

## MockAuthService Overview

File:

```text
src/app/shared/services/mock-auth.ts
```

The service is named `MockAuthService` because it simulates authentication without connecting to a real backend.

It currently:

- Creates a fake patient or doctor.
- Stores the current user.
- Provides the current user to components.
- Logs the user out.
- Creates a fake patient account.

It does not currently:

- Validate a password.
- Call a backend API.
- Check whether an account exists.
- Store a password.
- Use a real authentication token.
- Persist the user after page refresh.
- Protect routes with an authentication guard.

## Import Statements

```ts
import { Injectable, signal } from '@angular/core';
import { UserRole } from '../models/user-role.model';
import { UserProfile } from '../models/user.model';
```

### `Injectable`

`Injectable` allows Angular to use the class as a dependency-injectable service.

### `signal`

`signal` creates reactive state in Angular.

A signal stores a value and notifies Angular when that value changes.

### `UserRole`

`UserRole` defines the possible roles:

```ts
export enum UserRole {
  Patient = 'patient',
  Doctor = 'doctor',
}
```

### `UserProfile`

`UserProfile` defines the structure of a logged-in user:

```ts
export interface UserProfile {
  readonly id: string;
  readonly fullName: string;
  readonly email: string;
  readonly role: UserRole;
}
```

Using a model ensures that every user object has a predictable structure.

## Service Declaration

```ts
@Injectable({
  providedIn: 'root',
})
export class MockAuthService {
```

This declares a root-level Angular service named `MockAuthService`.

It can be injected into:

- Login
- Patient Dashboard
- Booking Review
- Future route guards
- Future navigation components

## Current User Signal

```ts
private readonly currentUserSignal = signal<UserProfile | null>(null);
```

This is the internal source of truth for the currently logged-in user.

### `UserProfile | null`

The value can be either:

```ts
UserProfile
```

or:

```ts
null
```

`null` means that no user is currently logged in.

At application startup, the value is:

```ts
null
```

After a successful login, it becomes a user object.

### `private`

The signal is private so that components cannot directly modify it.

This prevents external code from doing this:

```ts
mockAuthService.currentUserSignal.set(...)
```

The service controls when the user is created, changed, or removed.

### `readonly`

The `readonly` keyword means that the property reference cannot be replaced.

The signal's stored value can still be changed using:

```ts
.set(...)
```

or:

```ts
.update(...)
```

## Public Read-Only User Signal

```ts
readonly currentUser = this.currentUserSignal.asReadonly();
```

This exposes the current user to other components without allowing them to modify it.

Components can read the value:

```ts
const user = this.mockAuthService.currentUser();
```

Templates can also read it:

```html
@if (currentUser(); as user) {
  <h1>Welcome back, {{ user.fullName }}</h1>
}
```

Components cannot call:

```ts
this.mockAuthService.currentUser.set(...)
```

because `currentUser` is read-only.

This follows an important state-management principle:

```text
Service owns the state.
Components read the state.
Only service methods update the state.
```

## What Is a Signal?

A signal is a reactive value.

Example:

```ts
const name = signal('Asha');
```

To read the value:

```ts
name();
```

To replace the value:

```ts
name.set('Priya');
```

To update based on the current value:

```ts
name.update((currentName) => currentName.toUpperCase());
```

In this project:

```ts
currentUserSignal()
```

reads the current user.

```ts
currentUserSignal.set(user)
```

stores a new user.

When a component template reads a signal, Angular tracks that dependency and updates the template when the signal changes.

## Login Method

```ts
login(email: string, role: UserRole): UserProfile {
```

This method accepts:

- The user's email.
- The selected role.

Notice that the password is not passed to this mock service.

That is because this is only a frontend simulation.

A real authentication service would usually send credentials to a backend API.

## Creating the Fake User

```ts
const user: UserProfile =
  role === UserRole.Patient
    ? {
        id: 'patient-asha',
        fullName: 'Asha Menon',
        email,
        role,
      }
    : {
        id: 'doctor-patel',
        fullName: 'Dr. Patel',
        email,
        role,
      };
```

This uses a conditional expression.

If the role is `Patient`, the service creates:

```ts
{
  id: 'patient-asha',
  fullName: 'Asha Menon',
  email,
  role
}
```

Otherwise, it creates:

```ts
{
  id: 'doctor-patel',
  fullName: 'Dr. Patel',
  email,
  role
}
```

The email comes from the form input.

The name and ID are currently hard-coded because this is mock data.

## Updating the Current User

```ts
this.currentUserSignal.set(user);
```

This stores the newly created user in the signal.

Every component that reads `currentUser` can now access the updated user.

For example:

```text
Login
  ↓
MockAuthService.login()
  ↓
currentUserSignal is updated
  ↓
Dashboard reads currentUser()
  ↓
Dashboard displays the user's name
```

## Returning the User

```ts
return user;
```

The method returns the created user to the caller.

The login component uses the returned value:

```ts
const user = this.mockAuthService.login(loginData.email, selectedRole);

console.log('Mock login successful:', user);
```

Returning the user is useful when the component needs to:

- Log the result.
- Navigate based on the user's role.
- Display a message.
- Store additional data.
- Track analytics.

## Logout Method

```ts
logout(): void {
  this.currentUserSignal.set(null);
}
```

Logout removes the current user by setting the signal back to `null`.

The dashboard calls it:

```ts
protected onLogout(): void {
  this.mockAuthService.logout();
  this.router.navigateByUrl('/login');
}
```

The logout process is:

```text
User clicks Logout
        ↓
MockAuthService.logout()
        ↓
currentUser becomes null
        ↓
Application navigates to /login
```

The service clears the user state, while the component handles navigation.

## Register Patient Method

```ts
registerPatient(fullName: string, email: string): UserProfile {
```

This method simulates creating a new patient account.

It accepts:

- Patient name
- Patient email

It creates a user:

```ts
const user: UserProfile = {
  id: 'patient-new',
  fullName,
  email,
  role: UserRole.Patient,
};
```

The role is always `Patient` because the signup screen currently creates patient accounts only.

The new user becomes the current user:

```ts
this.currentUserSignal.set(user);
```

The login component then navigates to the dashboard:

```ts
this.router.navigateByUrl('/patient/dashboard');
```

## Mock Authentication Flow

```text
User enters email and password
        ↓
Login form is validated
        ↓
Selected role is read
        ↓
MockAuthService.login() is called
        ↓
Fake user is created
        ↓
Current user signal is updated
        ↓
User is navigated to the dashboard
```

## Services Used in This Project

The project currently contains three custom application services.

### 1. MockAuthService

File:

```text
src/app/shared/services/mock-auth.ts
```

Purpose:

- Manage the current user.
- Simulate login.
- Simulate registration.
- Simulate logout.
- Share authentication state.

Used by:

- `Login`
- `PatientDashboard`
- `BookingReview`

### 2. BookingDraftService

File:

```text
src/app/shared/services/booking-draft.ts
```

Purpose:

- Temporarily store the booking form data.
- Move booking data from the form page to the review page.
- Allow the success page to display the booking information.

Important methods:

```ts
save(draft: BookingDraft): void
```

Stores the booking form data.

```ts
get(): BookingDraft | null
```

Reads the stored booking draft.

```ts
clear(): void
```

Removes the stored draft.

The flow is:

```text
New Booking Form
        ↓
BookingDraftService.save()
        ↓
Review Page
        ↓
BookingDraftService.get()
```

This service is needed because the form page and review page are separate Angular components and separate routes.

### 3. AppointmentService

File:

```text
src/app/shared/services/appointment.ts
```

Purpose:

- Store confirmed appointments.
- Add new appointments.
- Mark appointments as completed.
- Share appointment data with multiple pages.

The service stores appointments in a signal:

```ts
private readonly appointmentsSignal = signal<Appointment[]>([]);
```

The public read-only list is exposed as:

```ts
readonly appointments = this.appointmentsSignal.asReadonly();
```

#### Adding an Appointment

```ts
add(appointment: Appointment): void {
  this.appointmentsSignal.update((appointments) => [
    ...appointments,
    appointment,
  ]);
}
```

The existing array is copied and the new appointment is appended.

The spread operator prevents direct mutation of the existing array.

#### Marking an Appointment Completed

```ts
markCompleted(appointmentId: string): void {
  this.appointmentsSignal.update((appointments) =>
    appointments.map((appointment) =>
      appointment.id === appointmentId
        ? {
            ...appointment,
            status: 'completed',
          }
        : appointment,
    ),
  );
}
```

This method:

1. Loops through all appointments.
2. Finds the appointment with the matching ID.
3. Creates a new appointment object with status `completed`.
4. Keeps all other appointments unchanged.

#### Clearing Appointments

```ts
clear(): void {
  this.appointmentsSignal.set([]);
}
```

This removes all appointments from memory.

## Appointment Service Flow

```text
User fills booking form
        ↓
BookingDraftService stores temporary data
        ↓
User reviews the booking
        ↓
BookingReview creates an Appointment object
        ↓
AppointmentService.add()
        ↓
Dashboard reads next appointment
        ↓
Bookings page reads upcoming and completed appointments
```

## Complete Service Data Flow

```text
Login Component
    ↓
MockAuthService
    ↓
Current User Signal
    ↓
Dashboard and Booking Review

New Booking Component
    ↓
BookingDraftService
    ↓
Booking Review

Booking Review
    ↓
AppointmentService
    ↓
Dashboard and Patient Bookings
```

## How Components Use the Services

### Login Component

Uses:

```ts
MockAuthService
```

Responsibilities:

- Validate login form.
- Call login service method.
- Navigate to the correct dashboard.
- Register a new patient.

### Patient Dashboard

Uses:

```ts
MockAuthService
AppointmentService
Router
```

Responsibilities:

- Read the current user.
- Read appointment data.
- Calculate the next upcoming appointment.
- Log the user out.
- Navigate between screens.

### Patient Bookings

Uses:

```ts
AppointmentService
```

Responsibilities:

- Read all appointments.
- Filter upcoming appointments.
- Filter completed appointments.
- Switch between booking tabs.

### New Booking

Uses:

```ts
BookingDraftService
Router
FormBuilder
```

Responsibilities:

- Collect booking information.
- Validate the form.
- Save the draft.
- Navigate to the review page.

### Booking Review

Uses:

```ts
MockAuthService
BookingDraftService
AppointmentService
Router
```

Responsibilities:

- Read the logged-in user.
- Read the booking draft.
- Create a confirmed appointment.
- Add the appointment to shared state.
- Navigate to the success page.

### Booking Success

Uses:

```ts
BookingDraftService
RouterLink
```

Responsibilities:

- Display the stored booking details.
- Provide navigation back to bookings.

## Angular Services Also Used

The project also uses Angular-provided services.

### Router

Used for programmatic navigation:

```ts
this.router.navigateByUrl('/patient/dashboard');
```

The Router changes the current URL and displays the matching component.

### FormBuilder

Used to create Reactive Forms:

```ts
this.fb.group({
  email: ['', Validators.required],
});
```

FormBuilder reduces the amount of code required to create form controls and validators.

### TestBed

Used only in unit tests.

It creates Angular testing environments and injects services during testing.

## Models Are Different from Services

Models describe the shape of data.

Examples:

```text
UserProfile
UserRole
BookingDraft
Appointment
```

Services manage behavior and data.

Example:

```text
MockAuthService manages UserProfile data.
AppointmentService manages Appointment data.
BookingDraftService manages BookingDraft data.
```

A model answers:

```text
What does the data look like?
```

A service answers:

```text
What can we do with the data?
```

## Why Signals Are Used in Services

Signals are useful when shared state changes during the application lifecycle.

Examples:

- Logged-in user changes after login.
- Logged-in user becomes null after logout.
- A new appointment is added.
- An appointment changes from upcoming to completed.

When a component reads a signal, Angular can update the UI automatically when the signal changes.

Example:

```ts
this.appointments = this.appointmentService.appointments;
```

The dashboard and bookings page both read from the same appointment signal.

When an appointment is added, both pages can receive the updated state.

## Computed Signals

The dashboard creates a computed signal:

```ts
this.nextAppointment = computed(
  () =>
    this.appointments().find(
      (appointment) => appointment.status === 'upcoming',
    ) ?? null,
);
```

This creates derived state.

The dashboard does not need to manually search for the next appointment every time.

Instead:

```text
All appointments
        ↓
Filter upcoming appointment
        ↓
nextAppointment
```

The bookings page also uses computed signals:

```ts
this.upcomingAppointments = computed(() =>
  this.appointments().filter(
    (appointment) => appointment.status === 'upcoming',
  ),
);
```

```ts
this.completedAppointments = computed(() =>
  this.appointments().filter(
    (appointment) => appointment.status === 'completed',
  ),
);
```

Computed signals are useful when a value can be calculated from another signal.

## When Should We Use a Service?

A service is appropriate when:

- Multiple components need the same data.
- A piece of logic should be reused.
- Data must survive navigation between components.
- The logic does not belong directly in the UI.
- The application needs a central source of truth.
- The application communicates with a backend.
- The application manages authentication or permissions.

Examples:

```text
AuthService
AppointmentService
BookingService
UserService
NotificationService
PaymentService
StorageService
```

## When Should We Not Use a Service?

A service may not be necessary for simple local UI state.

Examples of state that can stay inside a component:

- Whether a dropdown is open.
- Which tab is selected on one page.
- A temporary animation state.
- A local loading indicator.
- A form field that no other component needs.

For example:

```ts
selectedTab = signal<'upcoming' | 'completed'>('upcoming');
```

This can remain inside `PatientBookings` because the selected tab belongs only to that page.

## Mock Service vs Real Service

The current service is a mock service.

It simulates a backend using local memory.

```text
Current implementation:
Component → MockAuthService → In-memory signal
```

A real implementation would use an HTTP API:

```text
Component → AuthService → HttpClient → Backend API → Database
```

A real login process would usually:

1. Send credentials to the backend.
2. Validate the email and password.
3. Receive a user profile and authentication token.
4. Store the session securely.
5. Return success or an error.
6. Protect private routes.

## Current Limitations of MockAuthService

The current service does not provide real security.

### Password Is Ignored

The method accepts only:

```ts
login(email: string, role: UserRole)
```

The password is not passed to the service.

Therefore, any valid form password can result in a successful mock login.

### Data Is Hard-Coded

The service uses fixed values:

```ts
id: 'patient-asha'
fullName: 'Asha Menon'
id: 'doctor-patel'
fullName: 'Dr. Patel'
```

A real application would receive these values from the backend.

### State Is Lost on Refresh

The current user exists only in memory.

Refreshing the browser resets:

```ts
currentUserSignal
```

back to:

```ts
null
```

### No Route Guards

Users can currently type a private route directly into the browser.

For example:

```text
/patient/dashboard
```

A future `AuthGuard` should check whether a user is logged in before allowing access.

### No Role Protection

The current application does not yet prevent doctors from opening patient routes or patients from opening doctor routes.

A future role guard could check:

```ts
currentUser.role
```

before allowing navigation.

## Future Real Authentication Design

A future real authentication system could use:

```text
AuthService
    ├── login()
    ├── register()
    ├── logout()
    ├── currentUser()
    ├── isAuthenticated()
    └── hasRole()
```

The backend would handle:

- Password verification
- Account creation
- Authentication tokens
- Session expiration
- Role validation
- Error responses

The frontend service would handle:

- Calling the API
- Exposing the current user
- Storing session state
- Logging out
- Returning errors to components

## Important Service Design Principles

### Single Responsibility

Each service should focus on one main concern.

```text
MockAuthService → User authentication
BookingDraftService → Temporary booking draft
AppointmentService → Confirmed appointments
```

### Encapsulation

Internal state should be private.

Public methods should control how the state changes.

### Reusability

A service can be used by several components without duplicating logic.

### Testability

Services can be tested independently from the UI.

### Single Source of Truth

Shared data should have one central owner.

For example:

```text
AppointmentService owns appointment data.
```

Components should not maintain separate copies of the appointment list.

## Key Understanding

An Angular service is a reusable application class that separates shared logic and state from the UI.

In this project:

- `MockAuthService` manages the current user.
- `BookingDraftService` transfers temporary booking information between pages.
- `AppointmentService` manages confirmed appointments.
- Signals make shared state reactive.
- `providedIn: 'root'` makes the services available across the application.
- Components call service methods and display the returned state.
- The current implementation is an in-memory mock and will later be replaced or connected to backend APIs.

The main architecture is:

```text
Components handle the screen.
Services handle shared logic and state.
Models define the data structure.
Router handles navigation.
```
```

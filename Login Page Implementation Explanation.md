# Login Page Implementation Explanation

## Purpose

This document reverse-engineers the current combined patient and doctor login implementation.

It explains:

- the files involved;
- the route and component flow;
- the role-selection behavior;
- the login, signup, and forgot-password states;
- the reactive form validation;
- the current frontend-only boundary;
- the naming conventions;
- the explanation-first process to use for future features.

This is an explanation document. No new Angular feature is implemented by this document.

## 1. High-level architecture

~~~mermaid
flowchart TD
    A[main.ts] --> B[AppComponent]
    B --> C[RouterOutlet]
    C --> D[app.routes.ts]
    D --> E[LoginComponent]
    E --> F[HTML template]
    E --> G[Reactive forms]
    E --> H[Signals for UI state]
    E --> I[CSS styles]
    E --> J[UserRole model]
~~~

The flow is:

1. main.ts starts Angular.
2. AppComponent is the root component.
3. RouterOutlet displays the active route.
4. app.routes.ts redirects the default URL to the login route.
5. LoginComponent renders the authentication screen.
6. Reactive forms validate user input.
7. Signals keep the UI state synchronized with the template.

## 2. File map

| File | Responsibility |
| --- | --- |
| src/main.ts | Starts the Angular application |
| src/main.server.ts | Starts the server-rendered application |
| src/app/app.ts | Root Angular component |
| src/app/app.html | Root template |
| src/app/app.routes.ts | URL and component mapping |
| src/app/features/auth/login/login.component.ts | Login behavior and form state |
| src/app/features/auth/login/login.component.html | Login, signup, and recovery markup |
| src/app/features/auth/login/login.component.css | Login layout and responsive design |
| src/app/features/auth/login/login.component.spec.ts | Login component tests |
| src/app/shared/models/user-role.model.ts | Patient and doctor role definitions |
| src/styles.css | Global browser reset and styles |

The source files under src are the files to review and modify. The files under dist are generated build output and should not be edited manually.

## 3. Application startup

The application starts from src/main.ts.

~~~ts
import { bootstrapApplication } from '@angular/platform-browser';
import { appConfig } from './app/app.config';
import { AppComponent } from './app/app';

bootstrapApplication(AppComponent, appConfig)
  .catch((err) => console.error(err));
~~~

This code tells Angular to use AppComponent as the root, loads application configuration, and starts the application. There is no login logic in main.ts.

## 4. Root application shell

The root component is small because routing owns the page content.

~~~ts
import { Component } from '@angular/core';
import { RouterOutlet } from '@angular/router';

@Component({
  imports: [RouterOutlet],
  selector: 'app-root',
  styleUrl: './app.css',
  templateUrl: './app.html',
})
export class AppComponent {}
~~~

The root template is:

~~~html
<router-outlet />
~~~

RouterOutlet is the placeholder where Angular renders the component selected by the current URL.

## 5. Route configuration

The login route is configured in src/app/app.routes.ts.

~~~ts
import { Routes } from '@angular/router';

export const routes: Routes = [
  {
    path: '',
    pathMatch: 'full',
    redirectTo: 'login',
  },
  {
    loadComponent: () =>
      import('./features/auth/login/login.component').then(
        ({ LoginComponent }) => LoginComponent,
      ),
    path: 'login',
  },
  {
    path: '**',
    redirectTo: 'login',
  },
];
~~~

| URL | Result |
| --- | --- |
| / | Redirects to /login |
| /login | Loads LoginComponent |
| Any unknown URL | Redirects to /login |

loadComponent lazy-loads the login component when the login route is requested.

## 6. User role model

The role values are defined once and reused across the application.

~~~ts
export enum UserRole {
  Patient = 'patient',
  Doctor = 'doctor',
}

export interface UserRoleOption {
  readonly role: UserRole;
  readonly label: string;
  readonly description: string;
}
~~~

The enum prevents repeated strings such as patient and doctor from being scattered throughout the code.

~~~ts
selectedUserRole.set(UserRole.Patient);
~~~

This is safer and easier to refactor than repeating the string value in multiple files.

## 7. Login component state

LoginComponent owns the behavior of the page.

~~~ts
type AuthenticationView = 'login' | 'signup' | 'forgot-password';
type FeedbackType = 'error' | 'info' | 'success';

export class LoginComponent {
  protected readonly authenticationView =
    signal<AuthenticationView>('login');

  protected readonly selectedUserRole =
    signal<UserRole>(UserRole.Patient);

  protected readonly isPasswordVisible = signal(false);
  protected readonly isSubmitting = signal(false);
  protected readonly feedbackMessage = signal('');
  protected readonly feedbackType = signal<FeedbackType>('info');
}
~~~

| State | Meaning |
| --- | --- |
| authenticationView | The visible authentication screen |
| selectedUserRole | Patient or doctor selection |
| isPasswordVisible | Whether the password input is visible |
| isSubmitting | Whether a form submit is in progress |
| feedbackMessage | Information or error text |
| feedbackType | Visual type of the feedback message |

Signals are used because these values change during user interaction and the template must react immediately.

## 8. Role selection

The role options are stored in one collection.

~~~ts
protected readonly userRoleOptions: readonly UserRoleOption[] = [
  {
    description: 'Book and manage your appointments',
    label: 'Patient',
    role: UserRole.Patient,
  },
  {
    description: 'Manage appointments and patient visits',
    label: 'Doctor',
    role: UserRole.Doctor,
  },
];
~~~

The template loops over that collection:

~~~html
@for (roleOption of userRoleOptions; track roleOption.role) {
  <button
    type="button"
    class="role-selector__option"
    [class.role-selector__option--selected]="
      selectedUserRole() === roleOption.role
    "
    (click)="selectUserRole(roleOption.role)"
  >
    <strong>{{ roleOption.label }}</strong>
    <small>{{ roleOption.description }}</small>
  </button>
}
~~~

The click handler is:

~~~ts
protected selectUserRole(userRole: UserRole): void {
  this.selectedUserRole.set(userRole);
  this.authenticationView.set('login');
  this.clearFeedbackMessage();
  this.isPasswordVisible.set(false);
}
~~~

Selecting a role updates the selected role, returns to login, clears old messages, and hides the password.

## 9. Reactive forms

The page uses Angular reactive forms so validation rules live in TypeScript.

### Login form

~~~ts
protected readonly loginForm = this.formBuilder.nonNullable.group({
  email: ['', [Validators.required, Validators.email]],
  password: ['', [Validators.required, Validators.minLength(8)]],
  rememberMe: [false],
});
~~~

The login form requires an email and a password with at least eight characters. rememberMe is currently a UI field only; it does not persist a session.

### Registration form

~~~ts
protected readonly registrationForm = this.formBuilder.nonNullable.group({
  confirmPassword: ['', [Validators.required]],
  email: ['', [Validators.required, Validators.email]],
  fullName: ['', [Validators.required, Validators.minLength(2)]],
  password: ['', [Validators.required, Validators.minLength(8)]],
  termsAccepted: [false, [Validators.requiredTrue]],
});
~~~

The registration form requires a name, valid email, password, matching confirmation password, and terms acceptance.

### Forgot-password form

~~~ts
protected readonly forgotPasswordForm = this.formBuilder.nonNullable.group({
  email: ['', [Validators.required, Validators.email]],
});
~~~

The recovery form only asks for an email. Sending a real reset email will be added later.

## 10. Password confirmation

Password matching is checked by a component getter.

~~~ts
protected get isRegistrationPasswordMismatch(): boolean {
  const { confirmPassword, password } = this.registrationForm.controls;

  return (
    confirmPassword.touched &&
    confirmPassword.value.length > 0 &&
    confirmPassword.value !== password.value
  );
}
~~~

The mismatch message appears after the confirmation field is touched and contains a different value.

## 11. Switching authentication views

The page uses one component with three internal views.

~~~ts
protected openAuthenticationView(view: AuthenticationView): void {
  if (view === 'signup' && this.selectedUserRole() === UserRole.Doctor) {
    this.setFeedbackMessage(
      'Doctor accounts are created by the clinic. Switch to Patient to create a new account.',
      'info',
    );
    return;
  }

  this.authenticationView.set(view);
  this.clearFeedbackMessage();
  this.isPasswordVisible.set(false);
}
~~~

The template decides which form to show:

~~~html
@if (authenticationView() === 'login') {
  <!-- Login form -->
}

@if (authenticationView() === 'signup') {
  <!-- Patient registration form -->
}

@if (authenticationView() === 'forgot-password') {
  <!-- Password recovery form -->
}
~~~

The current MVP has one clinic-managed doctor account. Patients can register; doctors can sign in; doctors cannot self-register. The backend must also enforce this rule because a frontend message is not a security boundary.

## 12. Login submit behavior

The current login method is intentionally frontend-only.

~~~ts
protected handleLoginSubmit(): void {
  if (this.loginForm.invalid) {
    this.loginForm.markAllAsTouched();
    this.setFeedbackMessage(
      'Enter a valid email and password to continue.',
      'error',
    );
    return;
  }

  this.isSubmitting.set(true);
  this.setFeedbackMessage(
    this.selectedRoleLabel +
      ' login is ready to connect to the authentication service.',
    'success',
  );
  this.isSubmitting.set(false);
}
~~~

The method:

1. checks form validity;
2. marks fields as touched when invalid;
3. shows an error when needed;
4. shows a prototype success message when valid;
5. resets the submitting state.

It does not yet call an API, verify credentials, create a session, store a token, or redirect to a dashboard.

## 13. Registration and password recovery

Registration follows the same validation-first pattern.

~~~ts
protected handleRegistrationSubmit(): void {
  if (this.registrationForm.invalid || this.isRegistrationPasswordMismatch) {
    this.registrationForm.markAllAsTouched();
    this.setFeedbackMessage(
      'Complete the registration form with matching passwords.',
      'error',
    );
    return;
  }

  this.isSubmitting.set(true);
  this.setFeedbackMessage(
    'Patient registration is ready to connect to the account service.',
    'success',
  );
  this.isSubmitting.set(false);
}
~~~

Forgot-password validates its email and shows a prototype message. The recovery wording does not reveal whether an email is registered, which is useful for the real security flow.

## 14. Validation messages

Common validation messages are centralized in one helper.

~~~ts
protected getFieldErrorMessage(
  fieldName: 'email' | 'password' | 'fullName' | 'confirmPassword',
): string {
  const control = this.getActiveFormControl(fieldName);

  if (!control || !control.touched || !control.invalid) {
    return '';
  }

  if (control.hasError('required')) {
    return 'This field is required.';
  }

  if (control.hasError('email')) {
    return 'Enter a valid email address.';
  }

  if (control.hasError('minlength')) {
    return fieldName === 'password'
      ? 'Use at least 8 characters.'
      : 'Enter at least 2 characters.';
  }

  return 'Check this field and try again.';
}
~~~

The template displays an error only when the helper returns text.

~~~html
@if (getFieldErrorMessage('email')) {
  <span class="form-field__error">
    {{ getFieldErrorMessage('email') }}
  </span>
}
~~~

## 15. Password visibility

The password toggle changes the input type.

~~~ts
protected togglePasswordVisibility(): void {
  this.isPasswordVisible.update((isVisible) => !isVisible);
}
~~~

~~~html
<input
  [type]="isPasswordVisible() ? 'text' : 'password'"
  formControlName="password"
/>

<button type="button" (click)="togglePasswordVisibility()">
  {{ isPasswordVisible() ? 'Hide' : 'Show' }}
</button>
~~~

Only the display mode changes; the password remains in the reactive form.

## 16. CSS naming convention

The login page uses BEM-like names.

| Pattern | Example | Meaning |
| --- | --- | --- |
| Block | .login-page | Independent UI area |
| Element | .login-page__form-panel | Part of the block |
| Modifier | .role-selector__option--selected | Variation of an element |

Examples:

~~~css
.login-page {}
.login-page__shell {}
.login-page__form-panel {}
.role-selector {}
.role-selector__option {}
.role-selector__option--selected {}
.feedback-message--error {}
~~~

The CSS includes responsive behavior for desktop, tablet, and mobile layouts.

## 17. TypeScript naming conventions

### Files

- Components use names such as login.component.ts.
- Models use names such as user-role.model.ts.
- Tests use the component name with the .spec.ts suffix.

### Classes and types

Use PascalCase:

~~~ts
LoginComponent
AppComponent
UserRole
UserRoleOption
AuthenticationView
~~~

### Variables and methods

Use camelCase:

~~~ts
selectedUserRole
authenticationView
isPasswordVisible
handleLoginSubmit
openAuthenticationView
getFieldErrorMessage
~~~

### Boolean values

Use an is, has, or should prefix:

~~~ts
isSubmitting
isPasswordVisible
isRegistrationPasswordMismatch
~~~

### Form names

Use the Form suffix:

~~~ts
loginForm
registrationForm
forgotPasswordForm
~~~

## 18. Current prototype boundary

### Available now

- patient and doctor role selection;
- login form;
- patient registration form;
- forgot-password form;
- email and password validation;
- password confirmation validation;
- password visibility toggle;
- feedback messages;
- responsive design;
- /login route;
- component tests.

### Not connected yet

- backend authentication;
- database users;
- password hashing;
- session or token storage;
- protected routes;
- dashboard redirects;
- doctor account provisioning;
- email verification;
- real password reset email;
- server-side login errors.

The current success message means the form is valid and ready for integration. It does not mean the user is authenticated.

## 19. Recommended backend-ready structure

~~~text
src/app/
├── core/
│   └── auth/
│       ├── auth.service.ts
│       ├── auth.guard.ts
│       ├── auth.interceptor.ts
│       └── auth.models.ts
├── features/
│   └── auth/
│       └── login/
│           ├── login.component.ts
│           ├── login.component.html
│           ├── login.component.css
│           └── login.component.spec.ts
└── shared/
    └── models/
        └── user-role.model.ts
~~~

| Part | Responsibility |
| --- | --- |
| LoginComponent | Collect input and display UI state |
| AuthService | Call login, registration, and reset APIs |
| AuthGuard | Protect patient and doctor routes |
| AuthInterceptor | Attach authentication credentials to API requests |
| auth.models.ts | Define request, response, and session types |
| Backend | Verify credentials and enforce role permissions |

The component should not contain API URLs, token storage rules, or database permissions.

## 20. Future login flow

~~~mermaid
sequenceDiagram
    participant User
    participant LoginComponent
    participant AuthService
    participant API
    participant Router

    User->>LoginComponent: Submit email, password, and role
    LoginComponent->>LoginComponent: Validate form
    LoginComponent->>AuthService: login(credentials)
    AuthService->>API: POST /auth/login
    API-->>AuthService: User and session token
    AuthService-->>LoginComponent: Authenticated user
    LoginComponent->>Router: Navigate to role dashboard
~~~

For the MVP:

- patient login should navigate to the patient dashboard;
- the single doctor login should navigate to the doctor dashboard;
- invalid credentials should keep the user on the login screen;
- the server should be the final authority for role access.

## 21. Tests

The login component tests cover the main prototype behavior.

~~~ts
describe('LoginComponent', () => {
  it('renders patient and doctor role options', () => {
    // Verify both role choices appear.
  });

  it('shows validation feedback for an empty login form', () => {
    // Submit without values and verify the error message.
  });

  it('opens the patient signup view', () => {
    // Select signup and verify the registration view.
  });

  it('explains that doctor accounts are clinic-managed', () => {
    // Attempt doctor signup and verify the information message.
  });
});
~~~

These tests protect the initial UI behavior while backend authentication is added later.

## 22. Explanation-first workflow for future requests

For future features, use this order:

1. Restate the feature goal in product terms.
2. Identify the user roles involved.
3. Describe the screen and navigation flow.
4. Define the required data.
5. Define state transitions and error cases.
6. List files that would be created or changed.
7. Provide representative code blocks.
8. Explain each code block.
9. Wait for approval.
10. Implement only after approval.
11. Run tests and explain the result.

Before implementing patient booking, the explanation should cover:

- booking steps;
- network failure behavior;
- preserving entered data;
- appointment statuses;
- patient and doctor dashboard display;
- missed appointments;
- doctor unavailability;
- walk-in patient representation;
- required screens and backend models.

## 23. Prompt format for future generations

~~~text
Do not modify the code yet.

First explain:
1. The user flow.
2. The UI states.
3. The data model.
4. The files that would change.
5. Representative code blocks.
6. Error and recovery cases.
7. The test plan.

Provide the explanation in Markdown.
Wait for my approval before implementing anything.
~~~

## Summary

The current login page is a standalone Angular feature with:

- a root route that loads the login page;
- a shared patient and doctor role model;
- one component for login, signup, and password recovery;
- reactive forms for validation;
- signals for UI state;
- BEM-like CSS naming;
- frontend-only submit behavior;
- tests for the core interactions.

The next implementation should first be planned as an authentication service and dashboard navigation design. Code changes should happen only after that explanation is reviewed and approved.


---

# Pin-to-Pin Visual and Development Guide

## A. Important clarification about the background

The page does not use a background image.

There is no PNG, JPG, or SVG file behind the login card. The visual background is produced by CSS:

- a base page color;
- a radial gradient;
- a linear gradient;
- a CSS pseudo-element;
- borders;
- box shadows;
- two span elements for the small brand mark.

This means you can recreate the design without downloading an image.

## B. Page structure

The page has two major regions:

~~~html
<main class="login-page">
  <div class="login-page__shell">
    <section class="login-page__brand-panel">
      <!-- Brand content -->
    </section>

    <section class="login-page__form-panel">
      <div class="login-page__form-container">
        <!-- Login, signup, or forgot-password content -->
      </div>
    </section>
  </div>
</main>
~~~

The main element represents the page. The shell is the white card. The brand panel is the left area. The form panel is the right area.

## C. Outer page background

~~~css
.login-page {
  min-height: 100dvh;
  padding: 32px;
  background:
    radial-gradient(
      circle at 15% 10%,
      rgba(193, 227, 219, 0.58),
      transparent 28%
    ),
    #f2f7f6;
  color: #173b3a;
}
~~~

### What each line does

| CSS | Purpose |
| --- | --- |
| min-height: 100dvh | Makes the page fill the visible browser height |
| padding: 32px | Adds space around the card on desktop |
| radial-gradient | Creates the soft mint glow |
| #f2f7f6 | Provides the base background color |
| color: #173b3a | Sets the default dark teal text color |

The radial gradient is layered above the solid color. The gradient fades to transparent, so the base color remains visible.

Try changing the base color:

~~~css
background: #eef5f4;
~~~

Try changing the glow strength:

~~~css
rgba(193, 227, 219, 0.85)
~~~

The last number controls opacity.

## D. Centered white card

~~~css
.login-page__shell {
  display: grid;
  grid-template-columns: minmax(360px, 0.86fr) minmax(480px, 1.14fr);
  max-width: 1180px;
  min-height: 760px;
  margin: 0 auto;
  overflow: hidden;
  border: 1px solid rgba(49, 109, 103, 0.12);
  border-radius: 28px;
  background: #ffffff;
  box-shadow: 0 24px 80px rgba(29, 70, 67, 0.14);
}
~~~

### Layout concepts

- display: grid creates columns.
- grid-template-columns creates the brand and form widths.
- max-width stops the card from becoming too wide.
- margin: 0 auto centers the card.
- overflow: hidden clips decorations inside rounded corners.
- border-radius creates rounded corners.
- box-shadow gives the card elevation.

The shell is a layout container. It does not know anything about login validation.

## E. Brand panel color

~~~css
.login-page__brand-panel {
  position: relative;
  display: flex;
  flex-direction: column;
  padding: 64px 58px 52px;
  overflow: hidden;
  background:
    linear-gradient(
      152deg,
      rgba(22, 77, 73, 0.98),
      rgba(35, 113, 104, 0.96)
    ),
    #1d6d66;
  color: #f7fffd;
}
~~~

The linear gradient is diagonal because it uses 152 degrees. The two teal colors produce the brand-panel transition.

position: relative is important because the decorative circle uses absolute positioning relative to this panel.

## F. Decorative circle

~~~css
.login-page__brand-panel::after {
  position: absolute;
  right: -130px;
  bottom: -170px;
  width: 430px;
  height: 430px;
  border: 1px solid rgba(255, 255, 255, 0.14);
  border-radius: 50%;
  box-shadow:
    0 0 0 42px rgba(255, 255, 255, 0.03),
    0 0 0 84px rgba(255, 255, 255, 0.025),
    0 0 0 126px rgba(255, 255, 255, 0.018);
  content: '';
}
~~~

This is not an image. It is a generated CSS element.

- ::after creates an extra visual element.
- content makes the pseudo-element exist.
- equal width and height create a square.
- border-radius: 50% turns the square into a circle.
- negative right and bottom values place part of it outside the panel.
- overflow: hidden clips the outside part.
- unblurred box shadows create the additional rings.

To practice, create this smaller version first:

~~~css
.panel::after {
  content: '';
  position: absolute;
  width: 200px;
  height: 200px;
  right: -80px;
  bottom: -80px;
  border: 1px solid white;
  border-radius: 50%;
}
~~~

## G. Brand mark

The small mark is made from HTML spans:

~~~html
<div class="login-page__brand-mark" aria-hidden="true">
  <span class="login-page__brand-mark-line"></span>
  <span
    class="login-page__brand-mark-line
           login-page__brand-mark-line--short">
  </span>
</div>
~~~

~~~css
.login-page__brand-mark {
  display: inline-flex;
  flex-direction: column;
  gap: 4px;
  width: 40px;
  height: 40px;
  transform: rotate(-45deg);
}

.login-page__brand-mark-line {
  display: block;
  width: 36px;
  height: 9px;
  border-radius: 99px;
  background: #b5e3d8;
}

.login-page__brand-mark-line--short {
  width: 22px;
  margin-left: 14px;
  background: #f8dca0;
}
~~~

The modifier class changes only the second line. This is why the CSS is reusable instead of creating two completely different selectors.

## H. Brand content

~~~html
<p class="login-page__eyebrow">HARMONY CARE</p>

<h1>Care that stays connected.</h1>

<p class="login-page__brand-copy">
  Manage appointments, care journeys, and clinic visits from one calm,
  secure space.
</p>
~~~

The content is ordinary HTML. CSS controls its color, spacing, font, and size.

The trust list is also HTML:

~~~html
<div class="login-page__trust-item">
  <span class="login-page__trust-icon" aria-hidden="true">✓</span>
  <span>Simple appointment management</span>
</div>
~~~

The check icon is text, not an image or icon package.

## I. Login, signup, and recovery are view states

~~~ts
type AuthenticationView = 'login' | 'signup' | 'forgot-password';

protected readonly authenticationView =
  signal<AuthenticationView>('login');
~~~

The value is read in the template:

~~~html
@if (authenticationView() === 'login') {
  <!-- Login form -->
}

@if (authenticationView() === 'signup') {
  <!-- Patient signup form -->
}

@if (authenticationView() === 'forgot-password') {
  <!-- Recovery form -->
}
~~~

This is the most important architectural idea. The component changes its visible state instead of duplicating the entire page three times.

## J. Changing views

~~~ts
protected openAuthenticationView(view: AuthenticationView): void {
  if (view === 'signup' && this.selectedUserRole() === UserRole.Doctor) {
    this.setFeedbackMessage(
      'Doctor accounts are created by the clinic. Switch to Patient to create a new account.',
      'info',
    );
    return;
  }

  this.authenticationView.set(view);
  this.clearFeedbackMessage();
  this.isPasswordVisible.set(false);
}
~~~

The method:

1. receives the requested view;
2. blocks doctor self-registration for the current MVP;
3. updates authenticationView;
4. clears an old message;
5. resets password visibility.

## K. Login form topics

~~~ts
protected readonly loginForm = this.formBuilder.nonNullable.group({
  email: ['', [Validators.required, Validators.email]],
  password: ['', [Validators.required, Validators.minLength(8)]],
  rememberMe: [false],
});
~~~

The matching HTML is:

~~~html
<form
  [formGroup]="loginForm"
  (ngSubmit)="handleLoginSubmit()"
  novalidate
>
  <input
    type="email"
    formControlName="email"
    autocomplete="email"
  />

  <input
    type="password"
    formControlName="password"
    autocomplete="current-password"
  />

  <button type="submit">Sign in</button>
</form>
~~~

The important relationship is:

- loginForm in TypeScript connects to formGroup;
- email connects to the email control;
- password connects to the password control;
- ngSubmit calls the submit method.

## L. Patient signup topics

~~~ts
protected readonly registrationForm = this.formBuilder.nonNullable.group({
  confirmPassword: ['', [Validators.required]],
  email: ['', [Validators.required, Validators.email]],
  fullName: ['', [Validators.required, Validators.minLength(2)]],
  password: ['', [Validators.required, Validators.minLength(8)]],
  termsAccepted: [false, [Validators.requiredTrue]],
});
~~~

The fields are:

| Field | Reason |
| --- | --- |
| fullName | Identifies the patient |
| email | Login and recovery identifier |
| password | Account credential |
| confirmPassword | Prevents typing mistakes |
| termsAccepted | Requires agreement before account creation |

Password matching is a cross-field rule:

~~~ts
protected get isRegistrationPasswordMismatch(): boolean {
  const { confirmPassword, password } = this.registrationForm.controls;

  return (
    confirmPassword.touched &&
    confirmPassword.value.length > 0 &&
    confirmPassword.value !== password.value
  );
}
~~~

The normal validators check individual fields. The getter compares two fields.

## M. Forgot-password topics

~~~ts
protected readonly forgotPasswordForm = this.formBuilder.nonNullable.group({
  email: ['', [Validators.required, Validators.email]],
});
~~~

~~~html
<form
  [formGroup]="forgotPasswordForm"
  (ngSubmit)="handleForgotPasswordSubmit()"
  novalidate
>
  <label for="forgot-password-email">Email address</label>

  <input
    id="forgot-password-email"
    type="email"
    formControlName="email"
    autocomplete="email"
  />

  <button type="submit">Send reset instructions</button>
</form>
~~~

Today this only validates and displays a prototype message. Later, it should call a backend endpoint that sends a time-limited password-reset link.

## N. Validation message process

The submit method checks the form:

~~~ts
protected handleLoginSubmit(): void {
  if (this.loginForm.invalid) {
    this.loginForm.markAllAsTouched();
    this.setFeedbackMessage(
      'Enter a valid email and password to continue.',
      'error',
    );
    return;
  }

  this.setFeedbackMessage(
    'Login is ready to connect to the authentication service.',
    'success',
  );
}
~~~

The order is important:

1. Check invalid state.
2. Mark controls touched.
3. Show errors.
4. Stop the method with return.
5. Continue only when valid.
6. Show the temporary success state.

## O. Naming conventions used

### TypeScript

- classes and types: PascalCase;
- variables and methods: camelCase;
- booleans: is, has, or should prefix;
- event handlers: handle prefix;
- forms: Form suffix.

Examples:

~~~ts
LoginComponent
AuthenticationView
selectedUserRole
isSubmitting
handleLoginSubmit
loginForm
~~~

### HTML

Use descriptive class names:

~~~html
login-page__form-panel
role-selector__option--selected
feedback-message--error
~~~

### CSS

The naming style is BEM-like:

- block: login-page;
- element: login-page__form-panel;
- modifier: feedback-message--error.

## P. Topics actually used in this screen

| Area | Topics actually used |
| --- | --- |
| Angular | Standalone component, routing, lazy loading, signals, reactive forms, control flow |
| TypeScript | Class, enum, interface, union type, getter, typed method, readonly property |
| HTML | Semantic sections, forms, labels, inputs, buttons, checkbox, interpolation, ARIA |
| CSS layout | Grid, flexbox, padding, margin, max-width, min-height |
| CSS visuals | Colors, gradients, border, radius, shadow, pseudo-element |
| CSS behavior | Hover, focus, disabled states, transitions |
| Responsive CSS | Media queries, clamp, dynamic viewport height |
| Testing | Component tests, build checks, browser DevTools testing |

## Q. Desktop testing

Use the browser at normal desktop size first. Then open DevTools responsive mode.

Test these widths:

- 1440px;
- 1280px;
- 1024px;
- 900px;
- 768px.

Check:

- the shell is centered;
- the two panels fit;
- the heading does not overlap;
- the form is not too wide;
- no horizontal scrollbar appears;
- hover and focus states work;
- every input has a label;
- buttons can be reached with the keyboard.

Keyboard test:

1. Press Tab repeatedly.
2. Confirm the focus outline is visible.
3. Confirm the focus order is logical.
4. Press Enter on the submit button.
5. Confirm validation appears.

## R. Mobile testing

Use these device sizes:

- 375 x 667;
- 390 x 844;
- 412 x 915;
- 768 x 1024.

At 900px and below, the layout becomes one column. At 560px and below, the desktop brand panel is hidden and the form becomes full-screen.

Test:

- no horizontal scrolling;
- inputs fit within the screen;
- buttons are easy to tap;
- role cards remain readable;
- the mobile brand mark is visible;
- error text wraps correctly;
- signup can scroll;
- forgot-password can scroll;
- the keyboard does not hide the active field;
- landscape orientation does not break the layout.

## S. Manual functional testing

### Login

- Open /login.
- Confirm Patient is selected.
- Select Doctor.
- Confirm the selected style changes.
- Submit empty fields.
- Confirm validation messages.
- Use invalid email.
- Use a short password.
- Use valid values.
- Confirm the prototype success message.
- Toggle password visibility.

### Signup

- Select Patient.
- Click Create a patient account.
- Confirm the signup screen.
- Submit empty fields.
- Test short name.
- Test invalid email.
- Test short password.
- Test mismatched passwords.
- Leave terms unchecked.
- Use valid values.
- Confirm the prototype registration message.
- Go back to sign in.

### Doctor signup restriction

- Select Doctor.
- Click the signup action.
- Confirm the signup screen does not open.
- Confirm the clinic-managed account message.

### Forgot password

- Click Forgot password.
- Submit empty.
- Submit invalid email.
- Submit valid email.
- Confirm the feedback message.
- Return to sign in.

## T. Automated checks

Run these commands from the Angular project directory:

~~~text
npm.cmd run build
npm.cmd test -- --watch=false
~~~

Build checks:

- TypeScript;
- Angular templates;
- CSS budgets;
- production compilation.

Tests check behavior such as:

~~~ts
it('renders patient and doctor role options', () => {});
it('shows validation feedback for an empty login form', () => {});
it('opens the patient signup view', () => {});
it('explains that doctor accounts are clinic-managed', () => {});
~~~

## U. Recreate the page yourself

Follow this order:

1. Create the main, shell, brand panel, and form panel.
2. Add the page background color.
3. Add the radial glow.
4. Add the white shell and shadow.
5. Add the brand linear gradient.
6. Add the CSS decorative circle.
7. Add the brand mark with spans.
8. Add the brand text and trust list.
9. Add the login FormGroup.
10. Connect the form using formGroup and formControlName.
11. Add validation messages.
12. Add the signup FormGroup.
13. Add the forgot-password FormGroup.
14. Add authenticationView state.
15. Add @if blocks.
16. Add role selection.
17. Add responsive breakpoints.
18. Test desktop.
19. Test mobile.
20. Add component tests.

Do not start by connecting the backend. First make the UI state, validation, and responsive layout understandable.

## V. What is still a prototype

The current code does not yet:

- verify credentials;
- create users in a database;
- hash passwords;
- send reset emails;
- create a session;
- store an access token;
- protect dashboard routes;
- redirect after successful authentication.

Those responsibilities belong to an AuthService, backend API, session strategy, and route guards.

## Final learning target

After studying this guide, you should be able to explain:

- how the browser creates the background without an image;
- how the two-panel layout is built with CSS Grid;
- how the brand panel is styled with gradients;
- how the decorative circle is created with a pseudo-element;
- how the three forms are represented as component states;
- how Angular reactive forms connect TypeScript to HTML;
- how validation messages are displayed;
- how responsive CSS changes the layout;
- how to test the page manually and automatically.



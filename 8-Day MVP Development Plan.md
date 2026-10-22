# 8-Day MVP Development Plan

This plan is for a UI-first MVP. It does not include backend authentication, payments, family-member booking, or rebooking.

## Topics to know before starting

You should understand these Angular topics:

### Angular

- Standalone components
- Component files: TypeScript, HTML, CSS
- Angular routing
- Lazy-loaded routes
- `RouterOutlet`
- Signals
- Reactive forms
- `FormBuilder`
- Validators
- `@if` and `@for`
- Event binding: `(click)`, `(ngSubmit)`
- Property binding: `[disabled]`, `[formGroup]`
- Interpolation: `{{ value }}`
- Class binding
- Component testing with `TestBed`

### TypeScript

- Classes
- Interfaces
- Enums
- Union types
- Methods and getters
- Arrays of objects
- Type-safe form data
- `private`, `protected`, and `readonly`

### Web and CSS

- HTML forms and labels
- Accessibility attributes
- CSS Grid
- Flexbox
- Media queries
- Gradients
- Pseudo-elements
- Responsive design
- Browser local storage for temporary drafts

---

# Day 1 — Project structure and authentication foundation

### Tasks

- Review the Angular project structure.
- Understand `main.ts`, `app.ts`, `app.html`, and `app.routes.ts`.
- Confirm the `/login` route.
- Review the login component file structure.
- Understand how standalone components work.
- Confirm the MVP scope.

### Angular topics

- Standalone components
- Routing
- `RouterOutlet`
- Lazy loading
- Component metadata

### Expected result

You should be able to explain how the application reaches the login page.

```text
index.html
→ main.ts
→ AppComponent
→ RouterOutlet
→ LoginComponent
```

### Stand-up update

```text
Yesterday: I reviewed the project structure and application startup flow.
Today: I am confirming the authentication route and component responsibilities.
Blockers: None.
```

---

# Day 2 — Login, signup, and forgot-password forms

### Tasks

Complete the three authentication views:

- login;
- patient signup;
- forgot password.

Add these fields:

### Login

- email;
- password;
- remember me.

### Signup

- full name;
- email;
- password;
- confirm password;
- terms checkbox.

### Forgot password

- email.

### Angular topics

- Reactive forms
- `FormBuilder`
- `FormGroup`
- `formControlName`
- `Validators`
- `@if`
- Signals for changing views

### Expected result

The three screens should display correctly and switch between views.

### Stand-up update

```text
Yesterday: I reviewed the authentication structure and routing.
Today: I am completing the login, signup, and forgot-password forms.
Blockers: None.
```

---

# Day 3 — Validation and responsive authentication UI

### Tasks

Add validation for:

- required fields;
- valid email;
- minimum password length;
- matching passwords;
- terms acceptance.

Test the UI at:

```text
1440px desktop
1024px tablet
390px mobile
375px mobile
```

Check:

- no horizontal scrolling;
- readable error messages;
- visible keyboard focus;
- usable buttons;
- mobile form spacing.

### Angular topics

- Reactive form validation
- `markAllAsTouched()`
- Control status: `valid`, `invalid`, `touched`
- Conditional error messages
- Class binding

### CSS topics

- Media queries
- CSS Grid
- Flexbox
- Responsive spacing
- Focus states

### Expected result

Authentication screens should work on desktop and mobile.

### Stand-up update

```text
Yesterday: I completed the authentication forms.
Today: I am adding form validation and testing the screens responsively.
Blockers: I will record any mobile layout issues.
```

---

# Day 4 — Patient dashboard shell

### Tasks

Create the patient dashboard layout:

- sidebar;
- home menu item;
- bookings menu item;
- logout placeholder;
- dashboard header;
- responsive sidebar behavior.

Create routes:

```text
/patient/dashboard
/patient/bookings
/patient/bookings/new
/patient/bookings/success
```

### Angular topics

- Feature components
- Child routes
- Router navigation
- `routerLink`
- Active route styling
- Shared layout components

### Expected result

The patient can move between Home and Bookings sections.

### Stand-up update

```text
Yesterday: I completed validation and responsive authentication testing.
Today: I am creating the patient dashboard shell and sidebar navigation.
Blockers: None.
```

---

# Day 5 — Patient booking form

### Tasks

Create the self-booking form with:

- doctor information;
- appointment date;
- available time;
- reason for visit;
- notes;
- confirmation checkbox.

Since there is only one doctor, display the doctor as fixed information instead of creating a doctor-selection feature.

### Angular topics

- Reactive forms
- Date and time input handling
- Form validation
- Reusable form-field patterns
- Component state
- Submit event handling

### Error cases to design

- missing date;
- missing time;
- invalid date;
- unavailable time slot;
- incomplete form;
- user goes back before confirmation.

### Expected result

A patient can complete the booking form UI for himself or herself.

### Stand-up update

```text
Yesterday: I created the patient dashboard structure.
Today: I am designing the patient self-booking form and its validation states.
Blockers: Time-slot behavior needs confirmation.
```

---

# Day 6 — Booking recovery, success, and patient bookings

### Tasks

Add booking recovery behavior:

- preserve incomplete form data temporarily;
- show retry message if submission fails;
- allow the user to continue after returning;
- clear saved draft after successful booking.

Use browser storage only for temporary booking data.

Do not store:

- passwords;
- authentication tokens;
- sensitive medical information.

Create the success screen with:

- booking reference;
- doctor;
- date;
- time;
- reason;
- view bookings button;
- return home button.

Create patient booking sections:

```text
Upcoming bookings
Completed bookings
Missed bookings
Doctor unavailable
```

### Angular topics

- Services or helper functions
- Browser `localStorage`
- Navigation after submit
- Loading and error state
- Success state
- Empty state

### Expected result

The complete patient booking flow should be visible.

```text
Patient dashboard
→ New booking
→ Confirmation
→ Success page
→ Patient bookings
```

### Stand-up update

```text
Yesterday: I completed the patient self-booking form.
Today: I am adding draft recovery, booking success, and patient booking sections.
Blockers: None.
```

---

# Day 7 — Doctor dashboard and appointment statuses

### Tasks

Create the doctor dashboard with:

- today’s appointments;
- upcoming appointments;
- completed appointments;
- needs follow-up;
- walk-in patients.

Sort appointments according to timestamp.

Add actions for:

- mark as diagnosed;
- mark as missed;
- mark as doctor unavailable;
- add walk-in patient;
- complete walk-in consultation.

### Angular topics

- Interfaces for appointment data
- Enums for appointment status
- Lists with `@for`
- Conditional UI using `@if`
- Sorting arrays
- Updating item status
- Shared models

### Recommended statuses

```text
scheduled
checked-in
in-consultation
completed
missed
needs-follow-up
doctor-unavailable
walk-in
cancelled
```

### Status movement

```text
scheduled → completed
scheduled → missed
scheduled → doctor-unavailable
walk-in → completed
missed → needs-follow-up
```

### Expected result

The doctor can see appointments in time order and update their status.

### Stand-up update

```text
Yesterday: I completed the patient booking and success flow.
Today: I am creating the doctor dashboard and appointment status actions.
Blockers: None.
```

---

# Day 8 — Complete testing, cleanup, and mentor review

### Tasks

Test the complete flow:

```text
Login
→ Patient dashboard
→ Create booking
→ Booking success
→ Patient bookings
→ Doctor dashboard
→ Mark diagnosed
→ Completed booking
```

Test special cases:

- patient misses appointment;
- doctor is unavailable;
- walk-in patient;
- network failure;
- incomplete booking;
- empty booking lists;
- mobile screen behavior.

Run:

```powershell
npm.cmd run build
npm.cmd test -- --watch=false
```

Review:

- naming conventions;
- unused styles;
- duplicate code;
- accessibility;
- route behavior;
- form validation;
- loading and error states.

### Angular topics

- Component testing
- `TestBed`
- Route testing
- Form testing
- Build verification
- Debugging template errors

### Expected result

The UI MVP is ready for mentor review.

### Stand-up update

```text
Yesterday: I completed the doctor dashboard and appointment statuses.
Today: I am testing the complete MVP flow and preparing it for mentor review.
Blockers: I will document any test failures or mentor feedback.
```

---

## Scope to avoid during these 8 days

Do not add these yet:

- payment integration;
- family-member booking;
- rebooking;
- multiple doctors;
- real email verification;
- real password reset email;
- backend authentication;
- medical records;
- notifications;
- advanced analytics.

First complete and review the UI flow. After mentor approval, plan the backend separately.

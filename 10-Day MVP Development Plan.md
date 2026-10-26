# 10-Day MVP Development Plan

**Project:** Clinic Appointment Management System · **Frontend:** Angular · **Scope:** UI-first MVP

---

## Day 1 — Angular project setup and application structure

### Tasks

- Create a new Angular project from scratch.
- Understand the generated folder and file structure.
- Understand `main.ts`, `app.ts`, `app.html`, and `app.routes.ts`.
- Create the initial login component.
- Configure and verify the `/login` route.
- Confirm the MVP requirements and page structure.

### Angular topics

- Angular CLI
- Standalone components
- Component files: TypeScript, HTML, CSS
- Angular routing and `RouterOutlet`
- Lazy-loaded routes

### Expected result

A working Angular application with the login page accessible through `/login`.

### Stand-up update

```
Yesterday: Project planning and MVP requirements review.
Today: I am setting up the Angular project, understanding its structure, and configuring the login route.
Blockers: None.
```

---

## Day 2 — Authentication pages UI

### Tasks

- Build the login page UI.
- Build the patient signup page UI.
- Build the forgot-password page UI.
- Add navigation links between the three views.
- Create a consistent layout and styling for authentication pages.

### Pages handled

- `/login`
- Patient signup view
- Forgot-password view

### Angular topics

- Standalone components
- Component templates and styles
- Interpolation
- Event binding
- Angular routing and navigation

### Expected result

All three authentication screens are displayed and users can navigate between them. Form validation and detailed responsive behavior will be handled on Day 3.

### Stand-up update

```
Yesterday: I created the Angular project and configured the login route.
Today: I am developing the login, patient signup, and forgot-password page UIs.
Blockers: None.
```

---

## Day 3 — Authentication forms, validation and responsive design

### Tasks

- Implement reactive forms for login, signup, and forgot password.
- Add email and password validation.
- Add confirm-password matching validation.
- Add terms-and-conditions validation.
- Display appropriate validation error messages.
- Make the pages responsive for desktop, tablet, and mobile.
- Test keyboard focus and form usability.

### Angular topics

- `FormBuilder`, `FormGroup`, and `formControlName`
- `Validators`
- `markAllAsTouched()`
- Conditional rendering with `@if`
- Property and class binding
- Signals for UI state where needed

### CSS topics

- CSS Grid and Flexbox
- Media queries
- Responsive spacing
- Focus states

### Expected result

Responsive authentication pages with working client-side validation. No real authentication or password-reset emails are included in this MVP.

### Stand-up update

```
Yesterday: I developed the authentication page UIs.
Today: I am implementing form validation and responsive layouts for all authentication screens.
Blockers: None.
```

---

## Day 4 — Patient dashboard and navigation

### Tasks

- Create the patient dashboard layout.
- Build the sidebar and dashboard header.
- Add Home, Bookings, and Logout placeholder menu items.
- Configure the patient feature routes.
- Add active navigation styling.
- Make the sidebar usable on mobile devices.

### Pages and routes handled

```
/patient/dashboard
/patient/bookings
/patient/bookings/new
/patient/bookings/success
```

### Angular topics

- Feature components
- Child routes
- `routerLink`
- Active route styling
- Shared layout components
- `RouterOutlet`

### Expected result

The patient dashboard shell is ready, and users can navigate between the patient sections.

### Stand-up update

```
Yesterday: I completed authentication forms, validation, and responsive styling.
Today: I am creating the patient dashboard layout, sidebar, and feature navigation.
Blockers: None.
```

---

## Day 5 — Patient appointment booking page

### Tasks

- Build the new appointment booking page.
- Display the fixed doctor information.
- Add appointment date and time fields.
- Add reason-for-visit and notes fields.
- Add a confirmation checkbox.
- Implement form validation and submit-button behavior.
- Handle invalid dates and incomplete form submissions.

### Page handled

`/patient/bookings/new`

### Angular topics

- Reactive forms
- `FormBuilder` and `Validators`
- Date and time inputs
- Submit event binding with `(ngSubmit)`
- Component state
- Conditional rendering with `@if`

### Expected result

A patient can fill out the appointment booking form, see validation messages, and proceed when the required information is valid.

> **Scope note:** Since this MVP supports one doctor, doctor selection is not required. Actual appointment availability and booking persistence will be simulated on the frontend.

### Stand-up update

```
Yesterday: I completed the patient dashboard layout and navigation.
Today: I am developing the appointment booking page with form fields and validation.
Blockers: Time-slot selection behavior needs to be finalized.
```

---

## Day 6 — Booking confirmation, success and patient bookings

### Tasks

- Implement temporary draft recovery for incomplete bookings.
- Preserve non-sensitive booking form data using browser storage.
- Create the booking success page.
- Display the booking reference and appointment details.
- Add View Bookings and Return Home navigation.
- Build the patient bookings page.
- Add Upcoming, Completed, Missed, and Doctor Unavailable sections.
- Add empty states when no bookings exist.

### Pages handled

- `/patient/bookings/success`
- `/patient/bookings`

### Angular topics

- Services or helper functions
- Browser `localStorage`
- Navigation after submission
- Loading, success, and error states
- Arrays of objects and conditional rendering

### Expected result

The patient booking journey works across the booking form, confirmation screen, and bookings page using frontend demo data.

### Stand-up update

```
Yesterday: I completed the patient appointment booking form.
Today: I am implementing booking draft recovery, the success page, and patient booking sections.
Blockers: None.
```

---

## Day 7 — Doctor dashboard and appointment list

### Tasks

- Create the doctor dashboard layout.
- Display today's appointments.
- Add appointment details: patient name, time, reason for visit, and new/returning status.
- Add sections for upcoming appointments, completed appointments, and follow-ups.
- Display walk-in patients.
- Sort appointments by date and time.
- Add empty states for days without appointments.

### Page handled

- Doctor dashboard and appointment management views.

### Angular topics

- TypeScript interfaces
- Arrays of objects
- `@for` and `@if`
- Sorting arrays
- Component state
- Reusable UI components

### Expected result

The doctor can view the appointment list in chronological order and identify each patient's visit information.

### Stand-up update

```
Yesterday: I completed the patient booking confirmation and bookings pages.
Today: I am developing the doctor dashboard and displaying appointments with patient details and statuses.
Blockers: None.
```

---

## Day 8 — Appointment status management and walk-ins

### Tasks

- Add actions to mark appointments as completed or missed.
- Add the doctor-unavailable status.
- Implement the needs-follow-up state.
- Add a walk-in patient form or entry interface.
- Allow a walk-in consultation to be marked completed.
- Update the dashboard when an appointment's status changes.
- Verify that appointments appear in the correct sections.

### Angular topics

- TypeScript enums and union types
- Interfaces for appointment data
- Updating objects in arrays
- Event binding
- Conditional rendering
- State management and reusable methods

### Expected result

The doctor can update appointment statuses and manage walk-in patients through the frontend UI.

### Stand-up update

```
Yesterday: I completed the doctor dashboard and appointment list.
Today: I am implementing appointment status actions and walk-in patient management.
Blockers: None.
```

---

## Day 9 — End-to-end integration and functional testing

### Tasks

- Connect the patient booking pages and doctor dashboard using shared frontend demo data.
- Verify the booking-to-success-to-bookings flow.
- Check that doctor-side status changes are reflected in the relevant patient booking sections where applicable.
- Test form validation and navigation.
- Test special cases:
  - incomplete booking;
  - missed appointment;
  - doctor unavailable;
  - walk-in consultation;
  - empty booking lists;
  - draft recovery.
- Check responsive behavior on desktop and mobile.

### Angular topics

- Shared services
- Component communication
- Routing and navigation
- State synchronization
- Reactive form testing
- Debugging template errors

### Expected result

The patient and doctor UI flows work together with consistent frontend demo data, and the main user journeys have been tested.

### Stand-up update

```
Yesterday: I implemented appointment status management and walk-in patient handling.
Today: I am integrating the patient and doctor workflows and testing the complete MVP flow.
Blockers: I will document any integration or functional issues.
```

---

## Day 10 — Final testing, cleanup and mentor review

### Tasks

- Run the complete application from start to finish.
- Verify every route and navigation link.
- Test the authentication and booking forms.
- Check appointment status transitions.
- Verify responsive layouts and accessibility.
- Remove unused code and duplicate styles.
- Review naming conventions and component responsibilities.
- Run the production build and available component tests.
- Document known limitations and prepare the MVP for mentor review.

### Commands

```
npm.cmd run build
npm.cmd test -- --watch=false
```

### Angular topics

- Component testing with `TestBed`
- Form and route testing
- Debugging
- Build verification
- Code organization and cleanup

### Expected result

A tested, responsive, UI-first clinic appointment MVP ready for mentor review, with known limitations documented.

### Stand-up update

```
Yesterday: I integrated the patient and doctor workflows and tested the main journeys.
Today: I am completing final testing, fixing issues, cleaning up the code, and preparing the MVP for mentor review.
Blockers: I will report any remaining issues or test failures.
```

---

## Final scope for the 10 days

| Area           | Planned outcome                                                                |
| -------------- | ------------------------------------------------------------------------------ |
| Authentication | Login, signup, and forgot-password UI with client-side validation              |
| Patient        | Dashboard, appointment booking, success, and bookings pages                    |
| Doctor         | Dashboard, appointment list, status management, and walk-ins                   |
| UI behavior    | Navigation, form validation, responsive layouts, and temporary draft recovery  |
| Testing        | Main user flows, edge cases, build verification, and available component tests |

**Out of scope:** Backend authentication, database persistence, real email verification, real password-reset emails, payments, family-member booking, rebooking, multiple doctors, and real-time notifications.

> **Important:** This is a frontend MVP. Any booking references, appointment status changes, and shared data are demo behavior unless connected to a backend later.

The plan follows the original 8-day sequence while allocating two additional days for integration and final verification. It also keeps the work focused on building the application from scratch rather than adding new features.

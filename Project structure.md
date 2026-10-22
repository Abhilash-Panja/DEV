The attached images are screenshots of your Angular project structure. They contain no separate instructions.

## Project structure

```text
Hospital-Management-System/
├── .angular/
├── .vscode/
├── dist/
├── node_modules/
├── public/
├── src/
├── angular.json
├── package.json
├── package-lock.json
├── README.md
├── login-page-implementation-explanation.md
├── tsconfig.json
└── server.ts
```

## 1. Generated and dependency folders

| Folder | Importance |
|---|---|
| `.angular/` | Angular cache and temporary build data. Usually do not edit manually. |
| `.vscode/` | VS Code workspace settings, launch settings, and editor configuration. |
| `node_modules/` | Installed npm packages. Generated using `npm install`. Do not edit manually. |
| `dist/` | Production build output. Do not edit; it is recreated during build. |
| `public/` | Static assets such as images, icons, and fonts that can be served directly. |

Inside `dist`, you have:

```text
dist/Hospital-Management-System/
├── browser/
├── server/
├── 3rdpartylicenses.txt
└── prerendered-routes.json
```

- `browser/`: browser production files.
- `server/`: server-side rendering files.
- `3rdpartylicenses.txt`: licenses of installed third-party libraries.
- `prerendered-routes.json`: routes generated during prerendering.

## 2. The src folder

The `src` folder contains the actual application source code.

```text
src/
├── app/
├── index.html
├── main.ts
├── main.server.ts
├── server.ts
└── styles.css
```

## 3. The app folder

The `app` folder contains the main Angular application.

```text
src/app/
├── features/
├── shared/
├── app.config.server.ts
├── app.config.ts
├── app.css
├── app.html
├── app.routes.server.ts
├── app.routes.ts
├── app.spec.ts
└── app.ts
```

### app.ts

This is the root Angular component.

```ts
export class AppComponent {}
```

It connects the main application component to:

- `app.html`;
- `app.css`;
- Angular routing.

### app.html

This contains:

```html
<router-outlet />
```

`router-outlet` is the placeholder where Angular displays the current page.

For example:

```text
/login → LoginComponent appears inside router-outlet
```

### app.css

Contains styles that belong to the root application component.

It should contain only global root-level layout styles, not all login-page styles.

### app.routes.ts

Defines browser routes.

```ts
export const routes: Routes = [
  {
    path: '',
    redirectTo: 'login',
  },
  {
    path: 'login',
    loadComponent: () =>
      import('./features/auth/login/login.component')
        .then(({ LoginComponent }) => LoginComponent),
  },
];
```

It controls:

- which URL loads which component;
- default redirects;
- lazy loading;
- unknown route handling.

### app.config.ts

Contains browser application configuration, such as:

- router providers;
- animations;
- HTTP client;
- application-wide providers.

### app.routes.server.ts

Contains route configuration used during server-side rendering.

It helps Angular know how routes should be rendered on the server.

### app.config.server.ts

Contains server-specific Angular configuration.

It is used when Angular renders pages before sending HTML to the browser.

### app.spec.ts

Tests the root `AppComponent`.

It verifies that the root application component works correctly.

## 4. Features folder

The `features` folder organizes the application by business feature.

```text
src/app/features/
└── auth/
    └── login/
        ├── login.component.ts
        ├── login.component.html
        ├── login.component.css
        └── login.component.spec.ts
```

This is called feature-based organization.

## 5. Login feature files

### login.component.ts

This is the main logic file for login, signup, and forgot password.

It contains:

- `LoginComponent`;
- patient and doctor role selection;
- login form;
- registration form;
- forgot-password form;
- validation rules;
- password visibility;
- feedback messages;
- switching between login/signup/recovery views.

Example:

```ts
protected readonly authenticationView =
  signal<AuthenticationView>('login');
```

This controls which screen is visible.

### login.component.html

This is the UI template.

It contains:

- brand panel;
- patient and doctor selector;
- login form;
- signup form;
- forgot-password form;
- buttons;
- labels;
- validation messages.

Angular connects this HTML to TypeScript using:

```html
[formGroup]="loginForm"
formControlName="email"
(click)="selectUserRole(...)"
@if (...)
@for (...)
```

### login.component.css

Contains the visual design for the login feature.

It controls:

- background colors;
- gradients;
- layout;
- spacing;
- buttons;
- inputs;
- responsive behavior;
- mobile layout;
- decorative circle;
- hover and focus states.

The background image is not stored in a separate file. It is created with CSS:

```css
background:
  radial-gradient(...),
  #f2f7f6;
```

### login.component.spec.ts

Contains automated tests for the login component.

It can test:

- patient and doctor role options;
- empty form validation;
- signup view;
- doctor signup restriction;
- forgot-password behavior.

## 6. Shared folder

```text
src/app/shared/
```

The `shared` folder contains reusable models, components, directives, and pipes.

Currently, it contains the role model:

```text
src/app/shared/models/user-role.model.ts
```

This file defines:

```ts
export enum UserRole {
  Patient = 'patient',
  Doctor = 'doctor',
}
```

It prevents repeating role strings throughout the project.

Later, `shared` may contain:

```text
shared/
├── components/
├── models/
├── pipes/
├── directives/
└── validators/
```

## 7. Application entry files

### index.html

This is the main HTML document loaded by the browser.

It contains the Angular root element:

```html
<app-root></app-root>
```

Angular loads the application inside this element.

### main.ts

This starts the browser Angular application.

```ts
bootstrapApplication(AppComponent, appConfig);
```

The flow is:

```text
index.html
    ↓
main.ts
    ↓
AppComponent
    ↓
RouterOutlet
    ↓
LoginComponent
```

### main.server.ts

This starts the Angular server-side rendering version.

It is used when Angular renders HTML on the server before sending it to the browser.

### server.ts

This is the Node.js server entry file for Angular SSR.

It can handle:

- server-side rendering;
- HTTP requests;
- static files;
- server startup;
- Angular HTML rendering.

## 8. Global styles

### styles.css

This contains styles applied globally across the application.

Use it for:

- browser reset;
- body styles;
- global font;
- reusable utility styles;
- global box-sizing rules.

Example:

```css
* {
  box-sizing: border-box;
}

body {
  margin: 0;
}
```

Feature-specific styles should remain in the related component CSS file.

## 9. Configuration files

### angular.json

Angular CLI configuration file.

It defines:

- project name;
- build settings;
- development server;
- test configuration;
- styles;
- assets;
- output directory;
- bundle budgets.

### package.json

Defines:

- project scripts;
- Angular dependencies;
- development dependencies;
- project metadata.

Typical scripts include:

```json
{
  "start": "...",
  "build": "...",
  "test": "..."
}
```

### package-lock.json

Locks the exact installed dependency versions.

This ensures that another developer installs compatible package versions.

Do not edit it manually. It is updated by npm.

### tsconfig.json

Base TypeScript configuration for the project.

It defines:

- TypeScript compiler settings;
- strictness;
- module behavior;
- project-wide options.

### tsconfig.app.json

TypeScript configuration specifically for application source files.

### tsconfig.spec.json

TypeScript configuration specifically for test files.

### .prettierrc

Defines code formatting rules, such as:

- indentation;
- quotes;
- line width;
- trailing commas.

### .editorconfig

Provides common editor settings for different editors and IDEs.

### .gitignore

Lists files Git should not track.

Typical ignored files include:

```text
node_modules/
dist/
.angular/
```

## 10. Documentation files

### README.md

The main project documentation.

It should explain:

- what the project does;
- how to install dependencies;
- how to run the project;
- how to build;
- how to test;
- important development notes.

### login-page-implementation-explanation.md

This is your design and development explanation document.

It is not used by Angular at runtime. It is for:

- understanding the login page;
- reviewing the UI design;
- explaining the code;
- recording decisions;
- helping development and mentor review.

## 11. Which files should you edit?

For the login feature, mainly edit:

```text
src/app/features/auth/login/login.component.ts
src/app/features/auth/login/login.component.html
src/app/features/auth/login/login.component.css
src/app/features/auth/login/login.component.spec.ts
```

Edit these only when necessary:

```text
src/app/app.routes.ts
src/app/shared/models/user-role.model.ts
src/styles.css
```

Do not manually edit:

```text
dist/
node_modules/
.angular/
```

## 12. Recommended development flow

```text
1. Design the screen
2. Create HTML structure
3. Add CSS layout
4. Add TypeScript state
5. Add reactive forms
6. Add validation
7. Add navigation
8. Add tests
9. Test desktop
10. Test mobile
11. Connect backend later
```

For your current project, the most important files to understand first are:

1. `login.component.html`
2. `login.component.css`
3. `login.component.ts`
4. `app.routes.ts`
5. `app.ts`
6. `main.ts`

Those files explain how the login page is displayed, styled, controlled, and loaded.


# HTML Fundamentals — Conceptual & Interview Revision Notes

> **Learning Goal:** Understand how HTML structures web pages, how elements and attributes work, how forms communicate with servers, and how browsers represent HTML using the DOM.

---

## 1. Introduction to HTML

### What is HTML?

**HTML (HyperText Markup Language)** is the standard markup language used to define the **structure and meaning of content on a web page**.

HTML tells the browser what content exists and how it is organized.

Examples include:

- Headings and paragraphs
- Images and hyperlinks
- Lists and tables
- Forms and input fields
- Navigation bars and footers

### Why is HTML a Markup Language?

HTML is a **markup language, not a programming language**.

A markup language describes the structure or meaning of content using tags, whereas a programming language provides computational capabilities such as conditional execution, iteration, and data processing.

| HTML | Programming Language (JavaScript) |
|---|---|
| Structures web content | Implements logic and behavior |
| Uses elements and attributes | Uses variables, functions, and control flow |
| Describes content semantics | Can perform calculations and decision-making |
| Does not directly execute algorithmic logic | Can implement algorithms |

**Example:**

```html
<h1>Welcome to My Website</h1>
<p>This is my first web page.</p>
```

HTML describes the content, but does not implement any decision-making logic.

> **Remember:** HTML provides structure, CSS controls presentation, and JavaScript adds behavior and interactivity.

---

## 2. Understanding HyperText

**HyperText** refers to text containing hyperlinks that connect users to other documents, pages, or resources.

Instead of reading documents strictly in sequence, users can navigate between connected resources.

### Example

```html
<a href="https://www.google.com">Visit Google</a>
```

Clicking the link navigates to the specified address.

### Breaking Down HTML

- **HyperText:** Content connected through hyperlinks.
- **Markup:** Tags used to describe content structure and meaning.
- **Language:** A standardized set of rules and syntax.

**Key takeaway:** HyperText makes documents interconnected, while markup organizes their contents.

---

## 3. Basic Structure of an HTML Document

Every typical HTML document follows a basic structure.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, initial-scale=1.0">
    <title>My First Website</title>
</head>
<body>
    <h1>Hello World!</h1>
    <p>Welcome to HTML.</p>
</body>
</html>
```

### Understanding the Structure

| Component | Purpose |
|---|---|
| `<!DOCTYPE html>` | Declares the document as HTML5 |
| `<html>` | Root element containing the HTML document |
| `<head>` | Contains document metadata and linked resources |
| `<meta charset="UTF-8">` | Specifies character encoding |
| `<meta name="viewport">` | Helps pages display correctly on mobile devices |
| `<title>` | Defines the browser tab's title |
| `<body>` | Contains the visible page content |

### Important Observation

Content inside `<head>` generally provides information about the document rather than visible page content.

Content inside `<body>` represents the main content rendered on the page.

---

## 4. HTML Tags and Elements

### What is an HTML Tag?

A **tag** is markup enclosed within angle brackets (`< >`) that helps define an HTML element.

Tags are the fundamental building blocks used to construct HTML documents.

### What is an HTML Element?

An **HTML element** generally consists of:

1. An opening tag.
2. Content.
3. A closing tag.

```html
<p>Hello World</p>
```

Breakdown:

- `<p>` → Opening tag
- `Hello World` → Content
- `</p>` → Closing tag
- `<p>Hello World</p>` → Complete element

**Important:** A tag is not the same as an element. A tag is part of the markup used to define an element.

### Types of HTML Elements

#### A. Elements with Opening and Closing Tags

These elements generally contain text or nested elements.

```html
<h1>Main Heading</h1>
<p>This is a paragraph.</p>
<div>Container</div>
<span>Inline Text</span>
```

#### B. Void Elements

Void elements cannot contain child content and do not have closing tags.

```html
<img src="photo.jpg" alt="Profile photo">
<input type="text">
<br>
<hr>
```

Common void elements:

| Element | Purpose |
|---|---|
| `<img>` | Displays an image |
| `<input>` | Creates an input control |
| `<br>` | Inserts a line break |
| `<hr>` | Represents a thematic break |
| `<meta>` | Defines metadata |
| `<link>` | Links external resources |

> **Interview Tip:** These are properly called **void elements**, not self-closing tags. In HTML, writing `<img />` is allowed, but the `/` is unnecessary and does not make it self-closing. XML and JSX have different syntax rules.

---

## 5. HTML Attributes

### What are Attributes?

**Attributes** provide additional information or configuration for HTML elements.

They are generally written inside the opening tag.

### Syntax

```html
<tagname attribute="value">Content</tagname>
```

### Example

```html
<a href="https://example.com" target="_blank">
    Visit Website
</a>
```

Here:

- `href` specifies the destination.
- `target="_blank"` opens the destination in a new browsing context, usually a new tab.

### Common HTML Attributes

| Attribute | Purpose |
|---|---|
| `id` | Identifies an element uniquely within a document |
| `class` | Groups elements for styling or selection |
| `href` | Specifies hyperlink destinations |
| `src` | Specifies a resource URL |
| `alt` | Provides alternative text for images |
| `type` | Specifies the type of an input or other element |
| `name` | Defines a form control's submission name |
| `value` | Specifies a value |
| `placeholder` | Provides a short input hint |
| `required` | Marks form input as required |
| `disabled` | Disables supported interactive controls |
| `autofocus` | Requests initial focus when the page loads |

**Key observation:** Attributes configure elements; they are not independent HTML elements.

---

## 6. HTML Headings and Paragraphs

### Headings

HTML provides six heading levels, from `<h1>` to `<h6>`.

```html
<h1>Main Heading</h1>
<h2>Section Heading</h2>
<h3>Subsection Heading</h3>
<h4>Fourth-Level Heading</h4>
<h5>Fifth-Level Heading</h5>
<h6>Sixth-Level Heading</h6>
```

`<h1>` represents the highest heading level and `<h6>` the lowest.

**Best practice:** Use heading levels to represent document hierarchy, not simply to adjust font size.

### Paragraphs

The `<p>` element represents a paragraph.

```html
<p>HTML is used to structure web pages.</p>
```

### Line Breaks

```html
<p>Hello<br>World</p>
```

The `<br>` element creates a line break within content.

> Use `<p>` for paragraphs and `<br>` only when a line break is semantically appropriate, such as in an address or poem.

---

## 7. Lists in HTML

HTML provides different list structures.

### A. Unordered List — `<ul>`

Used when the order of items is not important.

```html
<ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
</ul>
```

**Output:**

- HTML
- CSS
- JavaScript

### B. Ordered List — `<ol>`

Used when the sequence matters.

```html
<ol>
    <li>Learn HTML</li>
    <li>Learn CSS</li>
    <li>Learn JavaScript</li>
</ol>
```

**Output:**

1. Learn HTML
2. Learn CSS
3. Learn JavaScript

### C. Description List — `<dl>`

Used for terms and their descriptions.

```html
<dl>
    <dt>HTML</dt>
    <dd>Defines web page structure.</dd>

    <dt>CSS</dt>
    <dd>Controls presentation.</dd>
</dl>
```

### Comparison

| Element | Purpose |
|---|---|
| `<ul>` | Unordered list |
| `<ol>` | Ordered list |
| `<li>` | List item |
| `<dl>` | Description list |
| `<dt>` | Term or name |
| `<dd>` | Description or value |

---

## 8. Images in HTML

The `<img>` element embeds an image in a web page.

### Syntax

```html
<img src="profile.jpg" alt="Person smiling">
```

### Important Attributes

**`src` — Source**

Specifies the image resource URL.

```html
<img src="images/logo.png" alt="Company logo">
```

The image URL may be relative to the document or an absolute URL.

**`alt` — Alternative Text**

Provides a text alternative for an image.

Useful for:

- Screen readers and accessibility.
- Understanding an image when it fails to load.
- Communicating the image's purpose or meaning.

### Important Accessibility Rule

Alternative text should describe the image's meaningful information or purpose, not simply repeat its filename.

```html
<!-- Meaningful image -->
<img src="dog.jpg" alt="Brown dog playing in a park">

<!-- Purely decorative image -->
<img src="divider.png" alt="">
```

An empty `alt` is appropriate for many purely decorative images.

**Interview takeaway:** The `<img>` element is a void element. The `src` identifies the image, while `alt` supplies its text alternative.

---

## 9. Hyperlinks and Anchor Tags

The `<a>` element creates hyperlinks.

### Basic Example

```html
<a href="https://example.com">Visit Website</a>
```

### Types of Links

**External link**

```html
<a href="https://developer.mozilla.org">
    MDN Documentation
</a>
```

**Internal link**

```html
<a href="about.html">About Us</a>
```

**Link to a section**

```html
<a href="#contact">Go to Contact</a>

<section id="contact">
    <h2>Contact Us</h2>
</section>
```

**Open in new tab**

```html
<a href="https://example.com" target="_blank" rel="noopener">
    Open Website
</a>
```

Using `rel="noopener"` is a good defensive practice when opening an external page in a new tab.

### Key Difference: `<a>` vs `<button>`

| `<a>` | `<button>` |
|---|---|
| Primarily used for navigation | Primarily used to perform actions |
| Uses `href` for destinations | Can trigger actions or submit forms |
| Represents a hyperlink | Represents an interactive button |

> **Rule:** Use an anchor for navigation and a button for actions.

---

## 10. Block and Inline Elements

HTML elements have different default display behaviors.

### A. Block-Level Display

Elements such as `<div>` and `<p>` normally use block display.

By default, block-displayed elements:

- Start on a new line.
- Normally expand across the available width of their container.
- Participate in block layout.

### Example

```html
<div>First Container</div>
<div>Second Container</div>
```

The containers normally appear on separate lines.

### B. Inline Display

Elements such as `<span>` and `<a>` normally use inline display.

By default, inline elements:

- Flow within surrounding text.
- Do not automatically start a new line.
- Occupy space according to their content.

### Example

```html
<p>
    I am learning
    <span>HTML</span>
    today.
</p>
```

### Block vs Inline

| Property | Block Display | Inline Display |
|---|---|---|
| Starts a new line | Normally yes | Normally no |
| Width | Normally fills available width | Generally follows content |
| Common example | `<div>` | `<span>` |
| Typical usage | Layout grouping | Text-level grouping |

**Important:** Block and inline are primarily display/layout behaviors. CSS can change an element's display behavior.

---

## 11. `<div>` and `<span>` Elements

### What is `<div>`?

`<div>` is a generic container used to group elements.

By default, it uses block display.

```html
<div class="profile-card">
    <h2>Student Profile</h2>
    <p>Name: Rahul</p>
    <p>Course: Computer Science</p>
</div>
```

### What is `<span>`?

`<span>` is a generic inline container used to group portions of text or phrasing content.

```html
<p>
    My favorite language is
    <span class="highlight">Java</span>.
</p>
```

### Difference

| `<div>` | `<span>` |
|---|---|
| Generic flow-content container | Generic phrasing-content container |
| Default display is block | Default display is inline |
| Commonly groups page sections | Commonly groups text fragments |
| No semantic meaning by itself | No semantic meaning by itself |

> **Important:** Neither `<div>` nor `<span>` has a specific semantic meaning. Prefer meaningful elements such as `<nav>`, `<main>`, and `<footer>` when appropriate.

---

## 12. Semantic HTML Elements

Semantic elements describe the purpose of content rather than acting as generic containers.

### Common Semantic Elements

| Element | Purpose |
|---|---|
| `<header>` | Introductory content for a page or section |
| `<nav>` | Major navigation links |
| `<main>` | Main content of the document |
| `<section>` | Thematic section of content |
| `<article>` | Self-contained content |
| `<aside>` | Tangential or supplementary content |
| `<footer>` | Footer information for a page or section |

### Example: Basic Page Layout

```html
<header>
    <h1>My Website</h1>
</header>

<nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/about">About</a>
    <a href="/contact">Contact</a>
</nav>

<main>
    <section>
        <h2>Welcome</h2>
        <p>This is the main content.</p>
    </section>
</main>

<footer>
    <p>&copy; 2026 My Website</p>
</footer>
```

### Why Use Semantic HTML?

- Makes code easier to understand and maintain.
- Gives browsers and assistive technologies meaningful structure.
- Supports accessibility.
- Helps search engines understand page structure.

**Interview Tip:** A `<nav>` element represents a navigation section. A `<footer>` represents footer content. Both convey more meaning than generic `<div>` elements.

---

## 13. Input Elements in HTML

The `<input>` element collects different types of user input.

It is a **void element**.

### Common Input Types

| Type | Purpose |
|---|---|
| `text` | Single-line text |
| `password` | Password entry |
| `email` | Email address |
| `number` | Numeric input |
| `date` | Date selection |
| `checkbox` | Independent selection |
| `radio` | Selection from a group |
| `file` | File selection |
| `tel` | Telephone number |
| `submit` | Form submission |

### Example

```html
<input type="text" placeholder="Enter your name">

<input type="email" placeholder="Enter your email">

<input type="password" placeholder="Enter password">

<input type="number" min="1" max="100">

<input type="date">

<input type="checkbox">

<input type="radio" name="plan" value="basic">
```

### Important Observation

Radio buttons with the same `name` are grouped so that normally only one option in that group can be selected.

```html
<label>
    <input type="radio" name="gender" value="male">
    Male
</label>

<label>
    <input type="radio" name="gender" value="female">
    Female
</label>
```

**Accessibility:** Input controls should have meaningful labels. Placeholder text is not a replacement for a label.

---

## 14. HTML Forms

### What is a Form?

The `<form>` element groups controls used to collect and submit user data.

Typical use cases include:

- Login
- Registration
- Feedback
- Search
- Contact forms

### Example

```html
<form action="/register" method="post">
    <label for="username">Username:</label>
    <input
        type="text"
        id="username"
        name="username"
        required
    >

    <label for="email">Email:</label>
    <input
        type="email"
        id="email"
        name="email"
        required
    >

    <button type="submit">Register</button>
</form>
```

### Understanding the Form

**`<form>`**

Defines the form.

**`<label>`**

Provides an accessible label for a form control.

**`<input>`**

Accepts user input.

**`<button type="submit">`**

Submits the form.

**`name`**

Defines the key used when the form control's value is submitted.

### Why is `name` Important?

Consider:

```html
<input type="text" name="username" value="Rahul">
```

The submitted data includes:

```text
username=Rahul
```

A normal named text input can contribute its value to form submission.

**Important:** An `id` alone does not make an input's value part of the submitted form data.

---

## 15. Form Attributes — `action` and `method`

Two important form attributes control where and how data is submitted.

### A. `action`

Specifies the URL that receives the form submission.

```html
<form action="/login">
```

Here, `/login` is the submission destination.

### B. `method`

Specifies the HTTP method used for submitting the form.

The commonly used native HTML form methods are:

- `GET`
- `POST`

### GET Method

```html
<form action="/search" method="get">
    <input type="text" name="query">
    <button type="submit">Search</button>
</form>
```

If the user enters `java`, the browser may navigate to:

```text
/search?query=java
```

### POST Method

```html
<form action="/login" method="post">
    <input type="text" name="username">
    <input type="password" name="password">
    <button type="submit">Login</button>
</form>
```

The form data is normally sent in the HTTP request body.

### GET vs POST

| Feature | GET | POST |
|---|---|---|
| Main purpose | Retrieve resources | Submit data for processing |
| Form data location | URL query string | Request body |
| Bookmarkable results | Generally yes | Not in the same way |
| Typical example | Search/filter | Registration/login |
| Data visible in URL | Yes | Normally no |

### Important Interview Clarification

**POST is not automatically secure.**

Although form values are not normally placed in the URL query string, they still require **HTTPS** to protect data in transit.

Also, changing data on the server should generally not be performed through GET requests.

---

## 16. HTML Form Validation

HTML provides built-in browser-side validation features.

### Common Validation Attributes

| Attribute | Purpose |
|---|---|
| `required` | Prevents submission when a required value is missing |
| `minlength` | Minimum permitted text length |
| `maxlength` | Maximum permitted text length |
| `min` | Minimum permitted numeric/date value |
| `max` | Maximum permitted numeric/date value |
| `pattern` | Specifies a regular-expression constraint |
| `type="email"` | Checks whether entered text follows email syntax |

### Example

```html
<form action="/signup" method="post">
    <label for="name">Name:</label>
    <input
        type="text"
        id="name"
        name="name"
        minlength="3"
        required
    >

    <label for="email">Email:</label>
    <input
        type="email"
        id="email"
        name="email"
        required
    >

    <button type="submit">Sign Up</button>
</form>
```

### How Validation Works

1. The user enters data.
2. The user attempts to submit the form.
3. The browser checks applicable validation constraints.
4. If invalid, normal form submission is blocked.
5. If valid, submission proceeds.

### Important Security Rule

**Client-side validation is not sufficient for security.**

Users or automated clients can bypass browser-side checks.

Therefore:

- Client-side validation improves the user experience.
- Server-side validation protects data integrity and enforces application rules.

> **Remember:** Never trust user input simply because it passed HTML validation.

---

## 17. The `autofocus` Attribute

The `autofocus` attribute requests that a particular element receive focus automatically.

### Example

```html
<input
    type="text"
    name="search"
    placeholder="Search here"
    autofocus
>
```

When the page loads, the browser may automatically focus the input.

### Important Points

- `autofocus` is a Boolean attribute.
- It does not require `autofocus="true"`.
- It should be used carefully because unexpected focus changes can affect accessibility.

### Boolean Attribute Examples

```html
<input type="text" required>

<input type="text" disabled>

<input type="checkbox" checked>
```

The presence of these Boolean attributes enables their respective behaviors.

**Common mistake:**

```html
<input required="false">
```

For a Boolean HTML attribute, its presence still means it is enabled.

To remove the requirement, omit the `required` attribute.

---

## 18. Client–Server Architecture

### What is Client–Server Architecture?

Client–server architecture is a model where a **client requests services or resources from a server**, and the server processes those requests and returns responses.

### Understanding the Components

**Client**

Usually a web browser or application that users interact with.

Responsibilities may include:

- Displaying the user interface.
- Accepting user input.
- Sending HTTP requests.
- Rendering server responses.

**Server**

A system that receives requests and provides the requested service.

Responsibilities may include:

- Processing requests.
- Running business logic.
- Authenticating users.
- Communicating with databases.
- Returning responses.

### Basic Request–Response Flow

```text
       USER
         |
         v
  +--------------+
  |    CLIENT    |
  |  (Browser)   |
  +--------------+
         |
         |  HTTP Request
         |  (Internet)
         v
  +--------------+
  |    SERVER    |
  |  (Backend)   |
  +--------------+
         |
         |  HTTP Response
         v
  +--------------+
  |    CLIENT    |
  | Renders Data |
  +--------------+
         |
         v
       USER
```

### Example: Login Form

**Step 1:** The user opens a login page in the browser.

**Step 2:** The user enters a username and password.

**Step 3:** The user submits the form.

**Step 4:** The browser sends an HTTP request to the server.

**Step 5:** The server validates the credentials against its authentication system.

**Step 6:** The server returns a response indicating success or failure.

**Step 7:** The browser displays the result or navigates accordingly.

### Example Request

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=rahul&password=example123
```

### Example Response

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

The response would normally also include a body containing the returned content.

### Key Observation

The browser does not directly execute server-side business logic.

Instead, the browser sends requests, and the server handles backend operations.

> **Interview Tip:** HTTP generally follows a request–response model. The client initiates a request, and the server returns a corresponding response.

---

## 19. DOM — Document Object Model

### What is DOM?

**DOM (Document Object Model)** is a programming interface that represents an HTML document as a structured tree of objects called nodes.

The browser parses HTML and constructs a DOM tree representing the document.

JavaScript can interact with this tree to read and modify page content and structure.

### HTML Example

```html
<html>
<head>
    <title>My Page</title>
</head>
<body>
    <h1>Hello</h1>
    <p>Welcome</p>
</body>
</html>
```

### Simplified DOM Tree

```text
Document
   |
   +-- html
        |
        +-- head
        |    |
        |    +-- title
        |         |
        |         +-- "My Page"
        |
        +-- body
             |
             +-- h1
             |    |
             |    +-- "Hello"
             |
             +-- p
                  |
                  +-- "Welcome"
```

### How It Works

1. The browser receives an HTML document.
2. The HTML parser processes its markup.
3. The browser constructs DOM nodes.
4. The nodes form a hierarchical tree.
5. JavaScript can inspect or modify the DOM.
6. DOM changes may trigger updates to the rendered page.

### Important Clarification

The DOM contains different node types, including:

- Document nodes.
- Element nodes.
- Text nodes.
- Comment nodes.

**Not every DOM node is an HTML element.**

### JavaScript DOM Example

```html
<h1 id="heading">Hello</h1>

<button type="button" onclick="changeText()">
    Change Text
</button>

<script>
    function changeText() {
        document.getElementById("heading").textContent =
            "Welcome to DOM";
    }
</script>
```

When the button is clicked, JavaScript updates the heading's text content.

### HTML vs DOM

| HTML | DOM |
|---|---|
| Markup defining document structure | In-memory object representation of the document |
| Written as source code | Constructed and maintained by the browser |
| Provides the initial document structure | Can be modified at runtime |
| Uses tags and attributes | Exposes nodes, properties, and methods |

> **Remember:** HTML defines the initial structure. The DOM is the browser's live representation of the document.

---

## 20. `id` and `class` Attributes

The `id` and `class` attributes are commonly used to identify or group HTML elements.

These are **global attributes**, meaning they can be used on HTML elements broadly.

However, they are not mandatory for every element.

### A. `id` Attribute

An `id` identifies a specific element within a document.

```html
<h1 id="page-title">Welcome</h1>
```

### Access Using JavaScript

```javascript
document.getElementById("page-title");
```

### Important Rules

- An `id` value should be unique within the document.
- An element should not have multiple `id` attributes.
- An `id` can be used for CSS, JavaScript, labels, and fragment navigation.

### B. `class` Attribute

A `class` groups elements that share common styling or behavior.

```html
<p class="highlight">First Paragraph</p>
<p class="highlight">Second Paragraph</p>
```

### Access Using JavaScript

```javascript
document.querySelectorAll(".highlight");
```

This returns a static NodeList of matching elements.

### Multiple Classes

One element can belong to multiple classes.

```html
<div class="card active featured">
    Product Details
</div>
```

### Comparison

| Feature | `id` | `class` |
|---|---|---|
| Primary purpose | Identify an element | Group or categorize elements |
| Reused across elements | Should not be | Yes |
| Multiple values per attribute | Single ID value | Space-separated class names |
| CSS selector | `#header` | `.header` |
| JavaScript example | `getElementById()` | `querySelectorAll()` |

### Important Interview Clarification

A class is not restricted to multiple elements: a single element can have a class too.

Similarly, an `id` is not exclusively for JavaScript. It can also help associate labels with form controls and identify sections for navigation.

---

## 21. Putting Everything Together — Complete HTML Example

The following example combines the major concepts covered in these notes.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, initial-scale=1.0">
    <title>Student Registration</title>
</head>

<body>

    <header>
        <h1>Student Portal</h1>
    </header>

    <nav aria-label="Main navigation">
        <a href="#home">Home</a>
        <a href="#register">Register</a>
    </nav>

    <main>

        <section id="home">
            <h2>Welcome Students</h2>
            <p>
                Learn <span class="highlight">HTML</span>
                and web development.
            </p>
        </section>

        <section id="register">
            <h2>Student Registration</h2>

            <form action="/register" method="post">

                <div>
                    <label for="name">Name:</label>
                    <input
                        type="text"
                        id="name"
                        name="name"
                        required
                        autofocus
                    >
                </div>

                <div>
                    <label for="email">Email:</label>
                    <input
                        type="email"
                        id="email"
                        name="email"
                        required
                    >
                </div>

                <div>
                    <label for="course">Course:</label>

                    <select id="course" name="course">
                        <option value="html">HTML</option>
                        <option value="css">CSS</option>
                        <option value="javascript">
                            JavaScript
                        </option>
                    </select>
                </div>

                <button type="submit">
                    Register
                </button>

            </form>
        </section>

    </main>

    <footer>
        <p>&copy; 2026 Student Portal</p>
    </footer>

</body>
</html>
```

### Concepts Used

| Concept | Example |
|---|---|
| Document structure | `html`, `head`, `body` |
| Headings | `h1`, `h2` |
| Paragraphs | `p` |
| Links | `a` |
| Containers | `div`, `span` |
| Semantic elements | `header`, `nav`, `main`, `section`, `footer` |
| User input | `input`, `select` |
| Forms | `form` |
| Validation | `required`, `type="email"` |
| Auto focus | `autofocus` |
| Element identification | `id`, `class` |
| Data submission | `action`, `method`, `name` |

**Note:** `/register` represents an example backend endpoint. The form needs a corresponding server-side handler to process and store submitted data.

---

## 22. Common Mistakes and Misconceptions

| Common Mistake | Correct Understanding |
|---|---|
| HTML is a programming language | HTML is a markup language |
| Tags and elements are identical | Tags define elements |
| Every element requires a closing tag | Void elements do not |
| `alt` only appears when images fail | It provides a text alternative, especially for accessibility |
| `div` and `span` have semantic meaning | They are generic containers |
| All elements are either permanently block or inline | CSS controls layout behavior |
| Every HTML element must have `id` and `class` | Both are optional |
| A class must be shared by multiple elements | A class can also belong to one element |
| Input `id` determines submitted form data | The `name` attribute provides the submission key |
| POST automatically protects sensitive data | HTTPS is required for transport security |
| Client validation guarantees data safety | Server validation is also necessary |
| HTML and DOM are identical | DOM is the browser's object representation |
| Placeholder replaces a label | Labels are still important for accessibility |

---

## 23. Interview Perspective — Important Questions

### Q1. What is HTML, and why is it not a programming language?

HTML is a markup language that structures web content using elements and attributes. It does not itself provide general-purpose computational constructs such as loops, functions, or conditional execution.

### Q2. What is the difference between a tag and an element?

A tag is part of HTML markup, such as `<p>` or `</p>`. An element is the complete structural unit represented by those tags and its content.

### Q3. What are void elements?

Void elements cannot contain child content and do not have closing tags. Examples include `<img>`, `<input>`, and `<br>`.

### Q4. What is the difference between `<div>` and `<span>`?

`<div>` is a generic flow-content container with block display by default, while `<span>` is a generic phrasing-content container with inline display by default.

### Q5. What is the difference between `id` and `class`?

An `id` identifies a specific element and should be unique within a document. A `class` groups or categorizes elements and can be reused.

### Q6. What is the purpose of `alt`?

It provides a text alternative for images, especially for assistive technologies and situations where the image cannot be displayed.

### Q7. What are `action` and `method` in a form?

`action` specifies the form submission destination. `method` specifies the HTTP method, commonly GET or POST.

### Q8. What is the difference between GET and POST?

GET requests are intended for retrieving resources and normally place HTML form data in the URL query string. POST requests submit data in the request body, often for server-side processing.

### Q9. What is the DOM?

The DOM is a tree-based programming interface representing a document. JavaScript can use it to inspect and modify document content and structure.

### Q10. What happens when a user submits an HTML form?

The browser validates applicable constraints, constructs the form data, sends an HTTP request according to the form configuration, and handles the server response.

### Q11. What is semantic HTML?

Semantic HTML uses elements whose names express their meaning or role, such as `<nav>`, `<main>`, `<article>`, and `<footer>`.

### Q12. Why is server-side validation necessary when HTML validation exists?

Browser-side validation can be bypassed. The server must independently verify incoming data to enforce application rules and maintain security.

---

## 24. Quick Revision Cheat Sheet — 2 to 5 Minutes

### Core Fundamentals

| Concept | Remember |
|---|---|
| HTML | HyperText Markup Language |
| Purpose | Structure and semantics of web content |
| HyperText | Connected documents through hyperlinks |
| Markup Language | Describes content using markup |
| CSS | Presentation |
| JavaScript | Behavior and interactivity |
| Tag | Markup defining an element |
| Element | Structural unit represented in the document |
| Attribute | Additional configuration for an element |

### Important Elements

| Element | Purpose |
|---|---|
| `<h1>`–`<h6>` | Headings |
| `<p>` | Paragraph |
| `<a>` | Hyperlink |
| `<img>` | Image |
| `<ul>` | Unordered list |
| `<ol>` | Ordered list |
| `<li>` | List item |
| `<div>` | Generic block-displayed container |
| `<span>` | Generic inline-displayed container |
| `<form>` | Form |
| `<input>` | Input control |
| `<label>` | Accessible input label |
| `<nav>` | Navigation |
| `<footer>` | Footer content |

### Critical Attributes

```text
src         → Resource URL
alt         → Image text alternative
href        → Link destination
id          → Unique element identifier
class       → Element grouping/category
name        → Form submission key
value       → Value
type        → Input type
required    → Required input
autofocus   → Requests automatic focus
action      → Form submission URL
method      → Form submission HTTP method
```

### Important Comparisons

```text
HTML vs JavaScript
HTML → Structure
JS   → Behavior and logic

div vs span
div  → Generic block-displayed container
span → Generic inline-displayed container

id vs class
id    → Unique within a document
class → Reusable

GET vs POST
GET  → Data normally sent in URL query string
POST → Data normally sent in request body

HTML vs DOM
HTML → Document markup
DOM  → Browser-managed node tree
```

### Form Submission Flow

```text
User
  ↓
HTML Form
  ↓
Browser Validation
  ↓
HTTP Request
  ↓
Server Processing + Validation
  ↓
HTTP Response
  ↓
Browser Displays Result
```

### DOM Recall

```text
HTML Source
    ↓
Browser Parsing
    ↓
DOM Tree
    ↓
JavaScript Can Modify Nodes
    ↓
Displayed Page May Update
```

### Must-Remember Interview Traps

1. HTML is a markup language, not a general-purpose programming language.
2. Void elements do not require closing tags.
3. `id` and `class` are optional global attributes.
4. Form controls generally need `name` to contribute data.
5. An `alt` attribute is important for image accessibility.
6. POST is not automatically secure; use HTTPS.
7. HTML validation does not replace server-side validation.
8. DOM nodes and HTML elements are not exactly the same.
9. Use semantic elements when they convey the correct meaning.
10. Default block/inline behavior can be changed using CSS.

---

## 25. Active Recall — Test Your Understanding

Try answering before checking the explanations.

### Question 1

Why is HTML considered a markup language rather than a programming language?

**Answer:** HTML describes document structure and meaning using markup but cannot independently implement general-purpose computational logic.

### Question 2

What happens when an `<input>` has an `id` but no `name`?

**Answer:** JavaScript and labels can still reference it through the ID, but the input's value will normally not be included in native form submission.

### Question 3

Why should we use `<nav>` instead of `<div>` for major navigation links?

**Answer:** `<nav>` provides semantic information that identifies a navigation region, improving accessibility and document structure.

### Question 4

Can multiple HTML elements use the same class?

**Answer:** Yes. Classes are reusable, and each element can also have multiple class names.

### Question 5

Is client-side form validation enough to protect a backend?

**Answer:** No. Client-side checks can be bypassed, so the server must independently validate submitted data.

### Question 6

What is the relationship between HTML and the DOM?

**Answer:** The browser parses HTML to construct the initial DOM, which represents the document as nodes and can change dynamically.

### Question 7

Why is `alt` important for images?

**Answer:** It communicates meaningful image content or purpose to assistive technologies and serves as a text alternative when the image is unavailable.

---

## Final Mental Model

Think about a website in four connected layers:

```text
HTML
  |
  | Defines structure and meaning
  v
CSS
  |
  | Controls appearance and layout
  v
JavaScript + DOM
  |
  | Enables interaction and updates
  v
HTTP + Server
  |
  | Processes requests and returns responses
  v
Dynamic Web Application
```

**One-line takeaway:**

> HTML structures the content, CSS presents it, JavaScript makes it interactive, the DOM exposes the document to code, and HTTP enables communication between clients and servers.

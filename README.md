# Signup Form Website Using HTML, PHP and MySQL

## Project Overview

This project is a basic Signup Form website. It allows a user to enter personal details in an HTML form and submit them to a PHP script. The PHP script receives the submitted data and attempts to save it in a MySQL database.

**Project type:** Dynamic web application  
**Frontend:** HTML  
**Backend:** PHP  
**Database:** MySQL  
**Form method:** HTTP POST  
**Database name in the supplied code:** `FCT`  
**Table name expected by the supplied code:** `users`

> **Important:** This README describes the supplied `signup.html` and `submit.php` files. The code expects the `FCT` database and `users` table to exist. The actual table schema and successful database setup were not included in the supplied files.

---

## 1. Introduction

The purpose of this project is to demonstrate how a web form can collect information from a user, send that information to a backend PHP script, and store it in a MySQL database.

The form includes these fields:

- **Name** — user's name (required)
- **Email** — user's email address (required)
- **Website** — website URL (optional)
- **Comment** — additional text (optional)
- **Gender** — Female, Male, or Other (required)

When the user clicks **Submit**, the browser sends the form fields to `submit.php` using the POST method. PHP reads the submitted values, connects to MySQL, and attempts to insert them into the `users` table. The result is then shown in the browser.

### Project objectives

- Build a basic HTML signup form.
- Send form data to a PHP backend using POST.
- Connect PHP to a MySQL database using MySQLi.
- Attempt to insert user-submitted data into a database table.
- Display a success message and submitted details, or an error message.

---

## 2. Architectural Diagram

```text
+----------------------+
|         User         |
| Fills in signup form |
+----------+-----------+
           |
           v
+----------------------+
|     signup.html      |
|      HTML Form       |
+----------+-----------+
           |
           | HTTP POST
           v
+----------------------+
|      submit.php      |
| Reads form data      |
| Connects to MySQL    |
| Runs INSERT query    |
+----------+-----------+
           |
           | MySQL connection
           v
+----------------------+
|    MySQL Database    |
|         FCT          |
|      users table     |
+----------+-----------+
           |
           v
+----------------------+
| Browser displays     |
| success or error     |
| and submitted data   |
+----------------------+
```

### Architecture explanation

1. The user opens `signup.html` in a browser.
2. The user enters the requested information and submits the form.
3. The browser sends the form fields to `submit.php` through an HTTP POST request.
4. PHP reads the values from `$_POST`.
5. PHP connects to the MySQL server on `localhost`.
6. PHP attempts to insert the data into the `FCT.users` table.
7. The browser receives a page showing the result.

---

## 3. Input

The input is the information entered by the user in the signup form.

| Field | HTML input type | Required? | Description |
|---|---|---|---|
| Name | `text` | Yes | User's name |
| Email | `email` | Yes | User's email address |
| Website | `url` | No | User's website URL |
| Comment | `textarea` | No | Additional comment |
| Gender | `radio` | Yes | Female, Male, or Other |

### Form configuration

The supplied HTML uses:

```html
<form action="submit.php" method="post">
```

- `action="submit.php"` sends the form data to the PHP file.
- `method="post"` sends the form fields in the HTTP request body.
- `required` makes the browser require values for Name, Email, and Gender before submitting.
- The Website and Comment fields are optional in the supplied HTML.

---

## 4. Output

After the form is submitted, `submit.php` attempts to save the submitted values and displays a result.

### Successful insert

If the database insert succeeds, the page displays:

- `New record created successfully!`
- Submitted Name
- Submitted Email
- Submitted Website
- Submitted Comment
- Submitted Gender

### Error output

If the SQL insert fails, the script displays an error message. If the database connection fails, it displays a connection failure message.

**Note:** The success message appears only if the SQL query succeeds. The supplied code alone does not prove that the database or table has been created or that the project has been run successfully.

---

## 5. Background Process

The background process explains what happens after the user clicks Submit.

### Step-by-step workflow

1. **Open the form:** The browser loads `signup.html`.
2. **Enter details:** The user fills in Name, Email, Website, Comment, and Gender.
3. **Submit data:** The browser sends the fields to `submit.php` using POST.
4. **Read the values:** PHP reads the fields using `$_POST`.
5. **Connect to the database:** PHP uses MySQLi to connect to MySQL on `localhost`.
6. **Prepare the insert operation:** The script builds an SQL `INSERT` query for the `users` table.
7. **Execute the query:** PHP calls `mysqli_query()` to attempt to insert the submitted values.
8. **Show the result:** The page displays the success message and submitted details, or an error.
9. **Close the connection:** PHP calls `mysqli_close($conn)`.

### Files in the project

```text
signup-project/
├── signup.html
├── submit.php
└── README.md
```

- `signup.html` — displays the signup form.
- `submit.php` — receives the submitted form fields, connects to MySQL, attempts the insert, and displays the result.
- `README.md` — documents the project and how it works.

### Technologies used

| Technology | Role |
|---|---|
| HTML | Builds the signup form and input fields |
| PHP | Processes submitted form data on the server |
| MySQL | Stores the submitted information |
| MySQLi | Allows PHP to connect to and query MySQL |
| Web browser | Displays the form and result page |
| Local web server | Runs PHP and serves the project during local development |

### Database requirements

The supplied PHP code expects:

- MySQL server running on `localhost`
- Database named `FCT`
- Table named `users`
- Columns named `name`, `email`, `website`, `comment`, and `gender`
- A MySQL account with permission to connect and insert records

The supplied files do **not** include a `CREATE DATABASE` or `CREATE TABLE` statement, so the exact column types and table schema must be configured separately.

### How to run locally

1. Install a local PHP/MySQL web stack, such as XAMPP, and start Apache and MySQL.
2. Place `signup.html` and `submit.php` together inside the web server's document root (for example, XAMPP's `htdocs/signup-project/` folder).
3. Create the `FCT` database in MySQL.
4. Create a `users` table with columns matching the fields used by the PHP script: `name`, `email`, `website`, `comment`, and `gender`.
5. Configure the database username and password in `submit.php` to match your local MySQL setup.
6. Open the form through the local web server, for example: `http://localhost/signup-project/signup.html`.
7. Enter test information and click Submit.

Do not open the HTML file directly as a `file://` URL when testing PHP; it must be served through a PHP-capable web server.

### Important security and reliability notes

This is a learning example and should be improved before real-world use:

- The current SQL query inserts values directly into the SQL string. Use a **prepared statement** to help prevent SQL injection.
- Validate all submitted fields on the server; browser-side `required` checks are not enough.
- Keep database credentials private and use a database account with only the permissions needed by the application. Do not publish real credentials in a public repository.
- Avoid displaying detailed database errors to public users in production; log errors securely instead.
- Use HTTPS when deploying a real website.
- The current PHP code enables error display, which is useful for local learning but should normally be disabled in production.
- Escape displayed user input. The supplied code uses `htmlspecialchars()` for the displayed field values, which is a useful protection when rendering those values as HTML.

---

## 6. Conclusion

This project demonstrates the basic connection between a frontend HTML form, a backend PHP script, and a MySQL database. The user enters details into the form, PHP receives them through POST, and the application attempts to save them in the database and show the result.

The project provides introductory practice with HTML forms, PHP superglobals, MySQLi database connectivity, SQL INSERT operations, and displaying a server-generated response. Further improvements should include prepared SQL statements, server-side validation, secure database credentials, and production-safe error handling.

---

## Quick Summary

- **Project:** Signup Form Website
- **Frontend:** HTML
- **Backend:** PHP
- **Database:** MySQL
- **Database connection library:** MySQLi
- **Form submission:** POST
- **Database:** `FCT` (as specified in the supplied PHP code)
- **Table:** `users` (expected by the supplied PHP code)
- **Main files:** `signup.html`, `submit.php`, `README.md`

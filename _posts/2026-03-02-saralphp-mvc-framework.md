---
title: "SaralPHP - Building a Custom PHP MVC Framework From Scratch"
date: 2026-03-02
layout: single
classes: narrow
description: "A deep dive into how I built SaralPHP: a lightweight mini-framework designed for building basic CRUD websites and understanding framework internals.Developed by Saral Singh"
image: /assets/images/php-mvc/php-mvc.jpg
categories: [php, backend]
tags: [php, mvc, framework, backend]
---

## Building a Custom PHP MVC Framework From Scratch

<p align="center">
  <img src="/assets/images/php-mvc/php-mvc.jpg" width="700" alt="PHP MVC Framework 3D Image">
</p>

Hey guys! My name is Saral Singh, and today I want to take you under the hood of a project I recently finished building: **SaralPHP**.

As a backend developer, it is incredibly easy to rely on massive frameworks like Laravel or Symfony. You type `php artisan make:controller`, magic happens, and you move on. But I wanted to understand the *magic*. I wanted to know exactly how routing engines parse URLs, how middleware pipelines chain together, and how an ORM securely binds data to a database. 

So, I built my own.

In this post, I am going to break down the core components of my custom PHP mini-framework, how it handles basic security, and why building your own MVC is one of the best learning exercises you can do.

---

### Part 1: The Architecture & Autoloading

Before writing any logic, I needed a structure. I modeled SaralPHP after the industry standard: the **Model-View-Controller (MVC)** pattern.

- `app/Controllers/`: Handles the business logic and HTTP responses.
- `app/Models/`: Interacts with the database.
- `views/`: Contains the frontend HTML/PHP templates.

To glue everything together without writing a bunch of `require_once` statements, I utilized **Composer and PSR-4 Autoloading**. By mapping the `App\` namespace to my `app/` directory in `composer.json`, PHP automatically finds and loads my classes the exact moment I instantiate them.

---

### Part 2: The Router (Dynamic Parameters)

The heart of any framework is the Router. It dictates where a user's web request should go.

Initially, I built a router that used strict string matching (`if ($uri === '/login')`). But real-world applications need dynamic URLs, like `/user/5`.

To solve this, I upgraded the router to use **Regular Expressions (`preg_match`)**.

When you define a route in SaralPHP:
```php
Route::get('/user/{id}', 'UserController', 'show');
```

The router dynamically converts `{id}` into a Regex capture group `([a-zA-Z0-9_-]+)`. When a user visits `/user/25`, the router extracts the `25` and passes it directly into the Controller method as an argument!

---

### Part 3: The Middleware Resolver (Chaining)

Middleware acts as a bouncer for your routes. If someone tries to visit a protected page without being logged in, the middleware kicks them out.

Instead of hardcoding `if` statements into every controller, I built a `MiddlewareResolver`. 
The cool part here is the ability to **chain** them using the pipe `|` operator in the routes file:

```php
Route::get('/admin', 'AdminController', 'index', 'auth|admin');
```

Behind the scenes, the framework runs an `explode('|', $middleware)` and loops through the array. First, it runs the `AuthMiddleware` to ensure the session exists. If they pass, it moves to the `AdminMiddleware` to check their role. If any middleware fails, the request is stopped.

---

### Part 4: The Database Layer & Basic Security

My goal was to build a base ORM (`baseModel`) that allowed for standard C.R.U.D operations while maintaining good security practices.

#### SQL Injection Protection
You should never pass raw user strings into a database query. SaralPHP's ORM utilizes **PDO Prepared Statements**. On top of that, when creating or updating records, the framework runs a validation against the column arrays to ensure users cannot inject SQL payloads into the database structure itself.

#### Mass Assignment Protection
In frameworks like Laravel, if you try to save a whole `$_POST` array, a malicious user could inject an `admin=1` hidden field into the form and give themselves privileges. 

I implemented a similar defense: The `$fillable` array.
```php
class User extends baseModel {
    protected $fillable = ['name', 'email', 'password']; 
}
```
If a user tries to pass an `admin` flag during registration, the `baseModel` automatically filters it out and drops it before the database query is built.

#### Session Protections
In my `AuthMiddleware`, just checking if `$_SESSION['id']` exists isn't always enough because session cookies can be stolen. To add a layer of defense, upon login, SaralPHP binds the user's `IP Address` and `User-Agent` (Browser string) to the session. On every protected request, it validates those markers. If they suddenly change, the session is destroyed.

---

### Conclusion

Building **SaralPHP** from scratch was an incredible learning experience. It stripped away the magic of large frameworks and forced me to engineer solutions to real routing and security problems.

Today, I have a quite good mini MVC framework that I can use for basic CRUD websites instead of dealing with the massive overhead of larger frameworks. It includes `.env` environment loading and global exception handling, so it's quite handy for rapid development.

If you are a student or developer looking to understand framework internals, or just want to poke around the source code, you can check it out below!

**🔗 [View SaralPHP on GitHub](https://github.com/SaralSingh/SaralPHP)**  
**🌐 [Live Demo](https://minimvc.wuaze.com)**

*Keep building, keep learning!*

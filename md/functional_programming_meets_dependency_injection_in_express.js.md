---
date: 2025-03-26T12:45:14-04:00
draft: false
title: Functional Programming Meets Dependency Injection in Express.js
---

Recently, I’ve been exploring some core functional programming (FP) principles and experimenting with how to incorporate them into my daily development workflow. One area where FP really shines is in how it pairs with Express.js—especially when it comes to structuring dependency injection (DI).

DI is a familiar pattern used to keep application code modular, clean, and maintainable. In the Node.js + Express ecosystem, you’ll often come across tutorials where services, database clients, and configuration are wired directly into the app. That works, but there’s a simpler and more flexible alternative using FP principles like composition and higher-order functions.

## What is Dependency Injection (DI)?

In a typical Express.js app, services, database clients, and config values are commonly wired up like this:

```
const express = require('express');
const app = express();
const dbClient = require('./dbClient');
const userService = require('./userService')(dbClient);

app.get('/users', async (req, res) => {
    const users = await userService.getUsers();
    res.json(users);
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

While this approach is functional and easy to follow, it has a few downsides:

- `userService` is tightly coupled to dbClient, making testing and future changes harder.
- The app logic is directly tied to how the services are instantiated, which reduces flexibility.

## Bringing in Functional Programming

Functional programming encourages passing dependencies **explicitly** via **parameters**. It focuses on **pure functions**, **composition**, and **clarity**. Here’s how that can play out in your Express.js app.

## Step 1: Create Pure Business Logic Functions

Instead of defining `userService` as a class or an immediately invoked function, use a factory function:

```
const createUserService = (db) => ({
    getUsers: async () => {
        return db.query('SELECT * FROM users');
    }
});
```

This keeps your business logic clean and makes it easy to pass in any kind of `db` — a real client in production or a mock in tests.

## Step 2: Compose Routes Using Injected Dependencies

Route handlers can be built in a similarly modular way:

```
const createUserRoutes = (userService) => {
    const router = require('express').Router();

    router.get('/users', async (req, res) => {
        const users = await userService.getUsers();
        res.json(users);
    });

    return router;
};
```

Now your routing layer doesn’t know or care how the service is built — it just uses what it’s given.

## Step 3: Assemble the Application

Finally, you bring everything together in the main application file:

```
const express = require('express');
const dbClient = require('./dbClient');
const createUserService = require('./userService');
const createUserRoutes = require('./userRoutes');

const app = express();

const userService = createUserService(dbClient);
const userRoutes = createUserRoutes(userService);

app.use('/api', userRoutes);

app.listen(3000, () => console.log('Server up on 3000'));
```

This approach keeps your main file focused on configuration and composition.

## Why This Approach Works Well

Adopting functional programming for DI offers several benefits:

1. **Improved Testability** – Easily swap in mock implementations without rewriting core logic.
1. **Greater Modularity** – Each component is self-contained and can be developed in isolation.
1. **Enhanced Reusability** – Functions and services can be reused across routes or even projects.
1. **Simplified Maintenance** – With clear dependency boundaries, the code is easier to understand and refactor.
1. **Reduced Dependency Overhead** – There’s no need to rely on an external DI framework. Let’s face it: npm packages can be black holes—pulling in way more than expected.

You don’t need a complex setup or heavy abstraction to apply DI effectively in Express.js. With just a few functional programming patterns, you can keep your app lightweight, testable, and easy to evolve.

If you're looking to simplify your architecture while staying flexible, this approach is definitely worth trying out.
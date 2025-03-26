# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [✓] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [✓] Commit: `Create Subscriber model struct.`
    -   [✓] Commit: `Create Notification model struct.`
    -   [✓] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [✓] Commit: `Implement add function in Subscriber repository.`
    -   [✓] Commit: `Implement list_all function in Subscriber repository.`
    -   [✓] Commit: `Implement delete function in Subscriber repository.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [✓] Commit: `Create Notification service struct skeleton.`
    -   [✓] Commit: `Implement subscribe function in Notification service.`
    -   [✓] Commit: `Implement subscribe function in Notification controller.`
    -   [✓] Commit: `Implement unsubscribe function in Notification service.`
    -   [✓] Commit: `Implement unsubscribe function in Notification controller.`
    -   [✓] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Given the current simplicity of BambangShop, where the Subscriber class acts as the sole observer, a single model struct suffices for now. Implementing a trait would become essential if Subscriber needed to accommodate multiple variations or behaviors. This approach aligns with the Open/Closed Principle from the SOLID design principles.

2. `DashMap` provides several advantages over `Vec`, primarily due to its key-value system, which enables direct mapping of `id` and `url` to their respective `Program` and `Subscriber`. Additionally, `DashMap` handles large datasets and high request volumes efficiently.  

Conceptually, `Vec` is similar to Java's `ArrayList`, whereas `DashMap` is comparable to `ConcurrentHashMap`. When retrieving a value by `id` or `url`, `DashMap` offers an average and worst-case time complexity of O(1), while `Vec` may require O(n) in the worst case.

3. `DashMap` remains necessary in this case. While it enables multithreaded processing for the `Subscriber` map, its built-in thread safety ensures safe concurrent operations. A Singleton pattern alone would not suffice without additional mechanisms to handle thread safety. Therefore, `DashMap` is still the preferred choice.

#### Reflection Publisher-2
1. Separating `Service` and `Repository` from the `Model` ensures compliance with the **Single Responsibility Principle (SRP)** from the SOLID principles. The `Repository` focuses on data access and storage, while the `Service` handles business logic and data processing. This separation enhances testability, allowing each component to be tested in isolation. Additionally, modifications to the `Repository` or `Service` can be made independently without impacting other parts, as each module operates separately.

2. If we depend solely on the `Model` without separating `Service` and `Repository`, it would take on multiple responsibilities, from handling data storage to managing business logic. This tight coupling means that modifying one function could impact others, increasing the risk of bugs. Consequently, the codebase becomes harder to maintain, more complex, and less readable. Furthermore, as the application scales with multiple `Repository` and `Service` instances, the `Model` module could grow excessively large, making it even more difficult to manage.

3. Postman is a powerful tool for testing API endpoints in our applications. It helps analyze program behavior by allowing us to send various types of requests (GET, POST, PUT, DELETE, etc.) to the server. The responses are displayed in Postman, making it easier to validate whether the endpoints work correctly. Seems like posting is the way to go, not burping—oops! 

#### Reflection Publisher-3

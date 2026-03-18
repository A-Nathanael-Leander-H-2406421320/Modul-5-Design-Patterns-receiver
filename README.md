# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. In this tutorial, we used `RwLock<>` to synchronise the use of `Vec` of `Notification`s. Explain why it is necessary for this case, and explain why we do not use `Mutex<>` instead?

    **Answer**:

    We use `RwLock<>` because it allows multiple readers to access the `Vec` of `Notification`s simultaneously, while still ensuring that only one writer can modify it at a time. This is beneficial in scenarios where read operations are more frequent than write operations, as it can improve performance by allowing concurrent reads. On the other hand, `Mutex<>` would only allow one thread to access the `Vec` at a time, regardless of whether it's a read or write operation, which could lead to unnecessary blocking and reduced performance when there are many read operations.

2. In this tutorial, we used `lazy_static` external library to define `Vec` and `DashMap` as a "static" variable. Compared to Java where we can mutate the content of a static variable via a `static` function, why did not Rust allow us to do so?

    **Answer**:

    Rust does not allow direct mutation of static variables because it would introduce data races and make the program's behavior unpredictable (it requires an `unsafe` block). Instead, Rust provides mechanisms like `RwLock<>` and `Mutex<>` to safely manage concurrent access to shared data, ensuring memory safety without sacrificing performance.

#### Reflection Subscriber-2

1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

    **Answer**:

    Yes, I have explored `src/lib.rs` and I learned that it serves as the main entry point for the application. It sets up the Rocket web server and mounts the routes defined in the controllers. This file is crucial for initializing the application and ensuring that all components are properly connected and ready to handle incoming requests. In adddition of that, it also defines custom data types such as `Result` and `Error` that are used throughout the application for error handling and response management. This exploration helped me understand how the different modules interact with each other and how the overall architecture of the application is structured.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

    **Answer**:

    The Observer pattern allows for a decoupled design where the main app (the subject) can notify multiple subscribers (observers) without needing to know the details of each subscriber. This makes it easy to add more subscribers to the system, as they can simply subscribe to the notifications they are interested in without affecting the main app's functionality. 

    However, spawning more than one instance of the main app could introduce complexity, as each instance would need to manage its own set of subscribers and ensure that notifications are sent to the correct subscribers. It would require additional coordination between the instances of the main app to ensure that they are aware of each other's subscribers and can properly route notifications. While it is still possible to add more instances of the main app, it may require additional design considerations to maintain a clean and efficient architecture.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).


    **Answer**:

    I haven't tried to make my own Tests or enhance documentation on the Postman collection yet because the default tests are enough. However, I believe that creating tests and improving documentation would be very useful for my work. Tests would help ensure that the functionality of the notification system is working as expected and would allow me to catch any bugs or issues early on. Enhancing the documentation in the Postman collection would also make it easier for others to understand how to use the API and test it effectively, which could be beneficial for collaboration in group projects. Overall, I think these features would contribute to a more robust and user-friendly application.

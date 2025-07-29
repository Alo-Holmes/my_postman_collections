# API Testing Demo with Postman

This repository showcases my API testing capabilities through two comprehensive Postman collections. These collections demonstrate various aspects of API interaction, dynamic data handling, and robust test automation, designed for clarity and reusability.

## Table of Contents

* [Project Overview](#project-overview)
* [Collections](#collections)
    * [My Demo API Test Project](#my-demo-api-test-project)
    * [Swagger Petstore Simplified Tests](#swagger-petstore-simplified-tests)
* [Getting Started](#getting-started)
    * [Prerequisites](#prerequisites)
    * [Importing Collections into Postman](#importing-collections-into-postman)
    * [Configuring Collection Variables (Crucial for Petstore)](#configuring-collection-variables-crucial-for-petstore)
* [How to Run the Tests](#how-to-run-the-tests)
* [Important Considerations for Swagger Petstore](#important-considerations-for-swagger-petstore)
* [Contact](#contact)
* [License](#license)

## Project Overview

This project serves as a practical demonstration of my skills in API testing. It features well-structured Postman collections with detailed test scripts that cover:

* **Fundamental CRUD Operations:** Demonstrating the ability to Create, Read, Update, and Delete resources via API endpoints.
* **Diverse Assertion Types:** Utilizing Postman's built-in Chai.js assertion library to validate various aspects of API responses (status codes, data types, property presence, specific values).
* **Dynamic Data Generation:** Employing pre-request scripts to generate unique and relevant data for test scenarios, especially for `POST` operations.
* **Variable Management:** Effective use of collection variables for base URLs, API keys, and other configurable parameters.
* **Handling API Nuances:** Strategies for testing against mock APIs, including understanding and adapting to their non-persistent nature and specific response structures.

**Key Technologies Used:**

* **Postman:** The primary tool for constructing, executing, and automating API tests.
* **JavaScript (in Postman):** Used extensively within Postman's pre-request and test scripts for dynamic data manipulation, conditional logic, and advanced assertions.
* **Chai.js Assertion Library:** Integrated within Postman's test environment, enabling highly expressive and readable test assertions.

## Collections

### My Demo API Test Project

This collection is designed to demonstrate foundational API testing against a stable and publicly available mock API. It focuses on standard API interactions and common validation patterns.

* **API Under Test:** [JSONPlaceholder](https://jsonplaceholder.typicode.com/) – A free, fake REST API for testing and prototyping.
* **What's Tested & How:**
    * **GET all Posts:**
        * **Purpose:** To retrieve a list of all available posts from the API.
        * **Tests:**
            * Verifies that the HTTP status code is `200 OK`.
            * Confirms the response body is an array.
            * Ensures the returned array is not empty, indicating data retrieval.
    * **GET Single Post by ID:**
        * **Purpose:** To fetch a specific post using its unique identifier.
        * **Tests:**
            * Asserts an HTTP status code of `200 OK`.
            * Checks that the response body is a JSON object.
            * Validates the presence of essential properties (`id`, `title`, `body`, `userId`) within the returned post object.
            * Confirms that the `id` in the response matches the requested ID.
    * **Create New Post (POST):**
        * **Purpose:** To add a new post resource to the API.
        * **Tests:**
            * Confirms a `201 Created` status code (or `200 OK`, depending on the API's specific success response for creation).
            * Verifies that the response body contains the data of the newly created post.
            * Validates that the `id` and other submitted properties are present in the response.
    * **Update Existing Post (PUT):**
        * **Purpose:** To modify the details of an existing post.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Ensures the response body reflects the updated data sent in the request.
            * Confirms that the `id` in the response matches the ID of the updated post.
    * **Delete Existing Post (DELETE):**
        * **Purpose:** To remove a post resource from the API.
        * **Tests:**
            * Verifies a `200 OK` status code.
            * Asserts that the response body is either completely empty or an empty JSON object, which is the standard indication of a successful deletion for this API.

### Swagger Petstore Simplified Tests

This collection is tailored for the Swagger Petstore API, a mock service. It demonstrates advanced testing strategies required when interacting with APIs that do not persist data.

* **API Under Test:** [Swagger Petstore API](https://petstore.swagger.io/v2)
* **Key Challenge & Approach:** The Swagger Petstore is a **mock API** and **does NOT persist data**. This is a critical distinction from a live API. Any data submitted via `POST` requests is processed and a success response is returned, but the data itself is immediately "forgotten" by the server. Consequently, attempting to retrieve or manipulate this dynamically created data in subsequent requests will result in a `404 Not Found` error.

    To overcome this, this collection employs a strategic approach:
    * **Fixed, Known IDs for `GET`, `PUT`, `DELETE`:** For operations that require interacting with existing data, the collection uses hardcoded, pre-defined IDs (e.g., `7`, `8`, `9` for pets/orders, `user1` for users) that are part of the mock API's static dataset. This ensures these tests consistently pass by targeting known resources.
    * **Dynamic Data Generation for `POST`:** `POST` requests still utilize pre-request scripts to generate unique data. This demonstrates the capability of dynamic data generation for creation flows, even though the created resources are not subsequently referenced due to the API's non-persistence.

* **What's Tested & How:**

    **Pet Endpoints:**
    * **Add a new pet to the store (POST /pet):**
        * **Purpose:** To create a new pet entry in the store.
        * **Testing Approach:** A pre-request script dynamically generates a unique `id`, `name`, `categoryName`, and `tagName` for the new pet. This dynamic data is then used to construct the request body.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates that the response body is a JSON object with expected properties (`id`, `name`, `status`).
            * Verifies that the `id` and `name` returned in the response match the dynamically generated values sent in the request.
    * **Update an existing pet (PUT /pet):**
        * **Purpose:** To modify the details of an existing pet.
        * **Testing Approach:** The request body directly specifies a hardcoded, known-to-exist pet ID (`7`) along with updated `name`, `status`, and other details.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, ensuring it contains `id`, `name`, and `status`.
            * Confirms that the returned `id`, `name`, and `status` match the updated values provided in the request body.
    * **Find pet by ID (GET /pet/{petId}):**
        * **Purpose:** To retrieve the details of a specific pet by its ID.
        * **Testing Approach:** The request URL directly uses a hardcoded, known-to-exist pet ID (`7`).
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, confirming properties like `id`, `name`, and `status`.
            * Verifies that the returned `id` is `7` and the default name associated with this ID is 'doggie'.
    * **Find Pets by status (GET /pet/findByStatus):**
        * **Purpose:** To retrieve a list of pets filtered by their status (e.g., 'available').
        * **Testing Approach:** The request uses a query parameter `status=available`.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Confirms the response is a non-empty array.
            * Verifies that every pet object in the array has the `available` status and contains all required fields (`id`, `category`, `name`, `photoUrls`, `tags`, `status`).
    * **Find Pets by tags (GET /pet/findByTags):**
        * **Purpose:** To retrieve a list of pets filtered by specific tags.
        * **Testing Approach:** The request uses multiple query parameters for tags (e.g., `tags=Rap Superstar,B rate Movie Actor`).
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Confirms the response is an array.
            * Verifies that each pet in the response has at least one of the specified tags and contains all required fields.
    * **Deletes a pet (DELETE /pet/{petId}):**
        * **Purpose:** To remove a pet entry from the store.
        * **Testing Approach:** The request URL directly specifies a hardcoded, known-to-exist pet ID (`8`). An `api_key` header is included for authorization.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates that the response is a JSON object containing `code`, `type`, and `message` properties.
            * Confirms that the `message` property includes the deleted pet ID (`8`), indicating successful processing by the mock API.
    * **Uploads an image (POST /pet/{petId}/uploadImage):**
        * **Purpose:** To upload an image associated with a pet.
        * **Testing Approach:** The request URL directly specifies a hardcoded, known-to-exist pet ID (`9`). The image data (a base64 string) is sent within the `formdata` body, explicitly set as `type: 'text'` with `contentType: 'image/png'` to ensure compatibility with Postman's sandbox environment for inline file uploads.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, checking for `code`, `type`, and `message` properties.
            * Confirms that the `message` property indicates a successful file upload.

    **Store Endpoints:**
    * **Place an order for a pet (POST /store/order):**
        * **Purpose:** To place a new purchase order for a pet.
        * **Testing Approach:** A pre-request script dynamically generates an `orderId` (within the 1-10 range for valid mock API responses), `petId`, `quantity`, and `shipDate`. The request body is then constructed with this data.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, checking for `id`, `petId`, `quantity`, and `status`.
            * Verifies that the returned `id` matches the dynamically sent ID and the `status` is 'placed'.
    * **Find purchase order by ID (GET /store/order/{orderId}):**
        * **Purpose:** To retrieve the details of a specific purchase order.
        * **Testing Approach:** The request URL directly specifies a hardcoded, known-to-exist order ID (`9`).
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, confirming properties like `id`, `petId`, `quantity`, `shipDate`, `status`, and `complete`.
            * Verifies that the returned `id` matches the requested ID (`9`).
    * **Returns pet inventories by status (GET /store/inventory):**
        * **Purpose:** To get a map of pet statuses to their respective quantities in the inventory.
        * **Testing Approach:** This request requires an `api_key` header for authorization.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Confirms the response is a JSON object (representing a map of string to integer).
            * Verifies that all values within the inventory map are numbers and that the map is not empty.
    * **Delete purchase order by ID (DELETE /store/order/{orderId}):**
        * **Purpose:** To remove a purchase order.
        * **Testing Approach:** The request URL directly specifies a hardcoded, known-to-exist order ID (`2`).
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Verifies that the response body is an empty JSON object, indicating successful deletion.

    **User Endpoints:**
    * **Create user (POST /user):**
        * **Purpose:** To register a new user in the system.
        * **Testing Approach:** A pre-request script dynamically generates all user details (`id`, `username`, `firstName`, `lastName`, `email`, `password`, `phone`, `userStatus`). The request body is then populated with this data.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, checking for `code`, `type`, and `message` properties.
            * Confirms that the `message` property includes the `id` of the newly created user.
    * **Logs user into the system (GET /user/login):**
        * **Purpose:** To authenticate a user and obtain a session token.
        * **Testing Approach:** The request uses hardcoded `username` (`user1`) and `password` (`password123`) as query parameters.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Confirms the response is a string (representing a session token).
            * Verifies that the response string indicates a successful login session.
    * **Get user by user name (GET /user/{username}):**
        * **Purpose:** To retrieve the details of a specific user by their username.
        * **Testing Approach:** The request URL directly specifies a hardcoded, known-to-exist `username` (`user1`).
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, confirming properties like `id`, `username`, and `email`.
            * Verifies that the returned `username` matches the requested `username` (`user1`).
    * **Updated user (PUT /user/{username}):**
        * **Purpose:** To modify the details of an existing user.
        * **Testing Approach:** The request URL directly specifies a hardcoded `username` (`user1`). The request body contains hardcoded updated user details.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, checking for `code`, `type`, and `message` properties.
            * Confirms that the `message` property includes the updated `username` (`user1`).
    * **Delete user (DELETE /user/{username}):**
        * **Purpose:** To remove a user from the system.
        * **Testing Approach:** The request URL directly specifies a hardcoded `username` (`user1`).
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, checking for `code`, `type`, and `message` properties.
            * Confirms that the `message` property includes the deleted `username` (`user1`), indicating successful processing by the mock API.
    * **Logs out current logged in user session (GET /user/logout):**
        * **Purpose:** To end the current user's session.
        * **Testing Approach:** A simple GET request to the logout endpoint.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, checking for `code`, `type`, and `message` properties.
            * Confirms that the `message` property indicates successful logout (e.g., contains 'ok').
    * **Creates list of users with given input array (POST /user/createWithList):**
        * **Purpose:** To create multiple user accounts in a single API call.
        * **Testing Approach:** A pre-request script dynamically generates an array containing two complete user objects. This array is then sent as the request body.
        * **Tests:**
            * Asserts a `200 OK` status code.
            * Validates the response structure, confirming it indicates a successful operation (e.g., has `code`, `type`, `message`).

## Getting Started

To utilize these Postman collections, you will need the Postman Desktop Application installed on your system.

### Prerequisites

* **Postman Desktop App:** Download and install the latest version from [Postman's official website](https://www.postman.com/downloads/).

### Importing Collections into Postman

Follow these steps to import the Postman collections into your Postman workspace:

1. **Download the Collection Files:**

   * `My Demo API Test Project.postman_collection.json`

   * `Swagger Petstore Simplified Tests.postman_collection.json`
     You can find these files directly in this GitHub repository.

2. **Open Postman:** Launch the Postman Desktop Application.

3. **Initiate Import:**

   * Click on the **"Import"** button, typically located in the top-left corner of the Postman interface.

   * In the import dialog, select the **"Upload Files"** tab.

   * Drag and drop both `.json` collection files from your downloaded location into the designated upload area, or click "Choose Files" and browse to select them.

   * Click the **"Import"** button to complete the process.

The collections will now be visible and accessible within your "Collections" sidebar in Postman.

### Configuring Collection Variables (Crucial for Petstore)

For the `Swagger Petstore Simplified Tests` collection to execute successfully, it is **imperative** that the `baseUrl` and `apiKey` variables are correctly configured as **Collection Variables**.

1. **Select the Collection:** In the Postman sidebar, locate and click on the **`Swagger Petstore Simplified Tests`** collection name.

2. **Access Variables Tab:** Click on the **"Variables"** tab, which appears in the main Postman window when the collection is selected.

3. **Verify and Set Values:** Ensure the following variables are present and their **Current Value** fields are populated as specified:

   * `baseUrl`: `https://petstore.swagger.io/v2`

   * `apiKey`: `special-key`
     *(Optional: You may also set these values in the `Initial Value` column if you intend to share the collection, as `Current Value` is local to your Postman instance.)*

4. **Save Changes:** After setting the values, click the **"Save"** button (usually a floppy disk icon or a "Save" text button next to the collection name) to apply the changes.

## How to Run the Tests

You have several options for running the tests within Postman:

1. **Run Individual Request:**

   * In the Postman sidebar, expand a collection to view its individual requests.

   * Click on any specific request (e.g., "GET all Posts").

   * Click the large **"Send"** button in the request tab.

   * The API response will appear in the response section, and the "Test Results" tab (usually located below the response) will display the pass/fail status of all associated tests for that request.

2. **Run Entire Collection:**

   * In the Postman sidebar, click directly on the collection name (e.g., "My Demo API Test Project").

   * Click the **"Run"** button (often represented by an arrow icon or explicitly labeled "Run Collection") that appears in the collection's overview.

   * The **Collection Runner** window will open. From here, you can select which requests within the collection you wish to execute, configure iteration counts, set delays, and view a comprehensive summary of all test results.

   * Click the final **"Run \[Collection Name\]"** button within the Collection Runner to begin the execution.

## Important Considerations for Swagger Petstore

It is crucial to understand the behavior of the `petstore.swagger.io` API, as it is a **mock service** and **does not persist any data** that you create or modify.

* **Non-Persistence of `POST` Data:** When you send a `POST` request (e.g., to create a new pet or order), the API will return a `200 OK` or `201 Created` status, indicating that your request was successfully processed and understood. However, the data you submitted is **not saved** to any permanent storage. If you immediately attempt to `GET` or `DELETE` the resource using the dynamically generated ID from the `POST` response, you will almost certainly receive a `404 Not Found` error.

* **Reliance on Static IDs for `GET`/`PUT`/`DELETE`:** To ensure the tests for `GET`, `PUT`, and `DELETE` operations consistently pass, this collection leverages a set of **pre-defined, static IDs and usernames** (`7`, `8`, `9` for pets/orders, `user1` for users) that are hardcoded into the mock API's internal logic. These IDs represent existing, static data that the mock API is programmed to respond to. This strategy allows the tests to verify the expected response formats and status codes for these operations reliably.

## Contact

For any inquiries, feedback, or further discussion regarding this API testing demo, please feel free to reach out via my GitHub profile.

## License

This project is open-sourced under the [MIT License](https://opensource.org/licenses/MIT).

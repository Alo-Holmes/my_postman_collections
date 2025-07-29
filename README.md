API Testing Demo with Postman
This repository serves as a comprehensive demonstration of my API testing skills using Postman. It includes two distinct Postman collections, each showcasing different aspects of API interaction, data handling, and robust test automation.

Table of Contents
Project Overview

Collections

My Demo API Test Project

Swagger Petstore Simplified Tests

Getting Started

Prerequisites

Importing Collections into Postman

Configuring Collection Variables (Crucial for Petstore)

How to Run the Tests

Important Considerations for Swagger Petstore

Contact

License

Project Overview
This project demonstrates proficiency in API testing by providing well-structured Postman collections with detailed test scripts. It covers fundamental CRUD (Create, Read, Update, Delete) operations, various assertion types, dynamic data generation, and handling API-specific nuances like mock server behavior and authentication.

Technologies Used:

Postman: For API request management, execution, and test automation.

JavaScript (in Postman): For pre-request scripts (data generation, setup) and test scripts (assertions).

Chai.js Assertion Library (built into Postman): For writing expressive and readable assertions in test scripts.

Collections
My Demo API Test Project
This collection showcases general API testing against a public, well-behaved API (JSONPlaceholder). It focuses on fundamental API interactions and common validation patterns.

API Under Test: JSONPlaceholder (a free fake API for testing and prototyping).

What's Tested & How:

GET all Posts:

Purpose: Retrieve a list of all existing posts.

Tests: Verifies a 200 OK status, confirms the response is an array, and ensures the array is not empty.

GET Single Post by ID:

Purpose: Retrieve a specific post using its ID.

Tests: Asserts a 200 OK status, checks that the response is an object, and validates that the returned post object has expected properties (e.g., id, title, body, userId). It also verifies the id matches the requested ID.

Create New Post (POST):

Purpose: Add a new post to the resource.

Tests: Confirms a 201 Created status (or 200 OK if the API returns that for creation), checks that the response contains the newly created post's data, and validates that the id and other properties are present.

Update Existing Post (PUT):

Purpose: Modify an existing post.

Tests: Verifies a 200 OK status, ensures the response reflects the updated data, and confirms the id matches the updated post.

Delete Existing Post (DELETE):

Purpose: Remove a post from the resource.

Tests: Asserts a 200 OK status and verifies that the response body is empty or contains an empty object, indicating successful deletion.

Swagger Petstore Simplified Tests
This collection targets the Swagger Petstore API, which is a mock API. This collection highlights strategies for testing APIs that do not persist data, requiring a different approach to ensure consistent test passes.

API Under Test: Swagger Petstore API

Key Challenge & Approach: The Swagger Petstore is a mock API and does NOT persist data. This means any data created via POST requests is immediately "forgotten" by the server. To ensure GET, PUT, and DELETE operations consistently pass, this collection uses fixed, known-to-exist IDs and usernames that are pre-defined within the mock API's static data. This allows for reliable testing of response formats and status codes for these operations. Dynamic data generation is still used for POST requests to demonstrate that capability.

What's Tested & How:

Pet Endpoints:

Add a new pet to the store (POST /pet):

Purpose: Create a new pet entry.

Testing Approach: Uses a pre-request script to dynamically generate a unique petId, petName, categoryName, and tagName. The request body is constructed with this dynamic data.

Tests: Asserts 200 OK status, validates the response structure (object with id, name, status), and verifies that the returned id and name match the dynamically sent values.

Update an existing pet (PUT /pet):

Purpose: Modify details of an existing pet.

Testing Approach: The request body directly specifies a hardcoded, known-to-exist pet ID (7) and updated details.

Tests: Asserts 200 OK status, validates the response structure, and confirms the returned id, name, and status match the updated values sent in the request.

Find pet by ID (GET /pet/{petId}):

Purpose: Retrieve details of a specific pet.

Testing Approach: The URL directly specifies a hardcoded, known-to-exist pet ID (7).

Tests: Asserts 200 OK status, validates the response structure, and confirms the returned id matches the requested ID (7). It also checks for the default name ('doggie') associated with this static ID.

Find Pets by status (GET /pet/findByStatus):

Purpose: Retrieve pets based on their status (e.g., 'available').

Testing Approach: Uses a query parameter for status.

Tests: Asserts 200 OK status, confirms the response is a non-empty array, and verifies that all returned pets have the specified 'available' status and expected properties.

Find Pets by tags (GET /pet/findByTags):

Purpose: Retrieve pets based on specified tags.

Testing Approach: Uses multiple query parameters for tags.

Tests: Asserts 200 OK status, confirms the response is an array, and verifies that all returned pets have at least one of the specified tags and expected properties.

Deletes a pet (DELETE /pet/{petId}):

Purpose: Remove a pet entry.

Testing Approach: The URL directly specifies a hardcoded, known-to-exist pet ID (8). Includes an api_key header for authorization.

Tests: Asserts 200 OK status, validates that the response is a JSON object with code, type, and message properties, and confirms the message includes the deleted ID (8).

Uploads an image (POST /pet/{petId}/uploadImage):

Purpose: Upload an image for a pet.

Testing Approach: The URL directly specifies a hardcoded, known-to-exist pet ID (9). The image data is sent as a base64 string within the formdata body, explicitly set as type: 'text' with contentType: 'image/png' to work around Postman sandbox limitations for inline file uploads.

Tests: Asserts 200 OK status, validates the response structure, and confirms the message indicates a successful upload.

Store Endpoints:

Place an order for a pet (POST /store/order):

Purpose: Create a new order.

Testing Approach: Uses a pre-request script to dynamically generate an orderId, petId, quantity, and shipDate.

Tests: Asserts 200 OK status, validates the response structure, and verifies the returned id and status ('placed') match the sent data.

Find purchase order by ID (GET /store/order/{orderId}):

Purpose: Retrieve details of a specific order.

Testing Approach: The URL directly specifies a hardcoded, known-to-exist order ID (9).

Tests: Asserts 200 OK status, validates the response structure, and confirms the returned id matches the requested ID (9).

Returns pet inventories by status (GET /store/inventory):

Purpose: Get a map of pet statuses to their quantities.

Testing Approach: No specific parameters needed.

Tests: Asserts 200 OK status, confirms the response is an object (map), and verifies that all values in the map are numbers and the inventory is not empty.

Delete purchase order by ID (DELETE /store/order/{orderId}):

Purpose: Remove an order.

Testing Approach: The URL directly specifies a hardcoded, known-to-exist order ID (2).

Tests: Asserts 200 OK status and verifies that the response body is an empty object.

User Endpoints:

Create user (POST /user):

Purpose: Register a new user.

Testing Approach: Uses a pre-request script to dynamically generate user details (userId, username, firstName, email, password, phone, userStatus).

Tests: Asserts 200 OK status, validates the response structure, and confirms the message includes the newly created userId.

Logs user into the system (GET /user/login):

Purpose: Authenticate a user.

Testing Approach: Uses hardcoded username (user1) and password (password123) as query parameters.

Tests: Asserts 200 OK status, confirms the response is a string (session token), and verifies the message indicates successful login.

Get user by user name (GET /user/{username}):

Purpose: Retrieve user details by username.

Testing Approach: The URL directly specifies a hardcoded, known-to-exist username (user1).

Tests: Asserts 200 OK status, validates the response structure, and confirms the returned username matches the requested one (user1).

Updated user (PUT /user/{username}):

Purpose: Modify an existing user's details.

Testing Approach: The URL directly specifies a hardcoded username (user1). The request body contains hardcoded updated user details.

Tests: Asserts 200 OK status, validates the response structure, and confirms the message includes the updated username.

Delete user (DELETE /user/{username}):

Purpose: Remove a user.

Testing Approach: The URL directly specifies a hardcoded username (user1).

Tests: Asserts 200 OK status, validates the response structure, and confirms the message includes the deleted username (user1).

Logs out current logged in user session (GET /user/logout):

Purpose: End the current user session.

Testing Approach: No specific parameters needed.

Tests: Asserts 200 OK status, validates the response structure, and confirms the message indicates successful logout.

Creates list of users with given input array (POST /user/createWithList):

Purpose: Create multiple users in a single request.

Testing Approach: Uses a pre-request script to dynamically generate an array of two user objects.

Tests: Asserts 200 OK status and validates the response structure, confirming successful operation.

Getting Started
To use these Postman collections, you will need the Postman Desktop Application.

Prerequisites
Postman Desktop App: Download and install from Postman's official website.

Importing Collections into Postman
Download the Collection Files:

My Demo API Test Project.postman_collection.json

Swagger Petstore Simplified Tests.postman_collection.json
(You can find these files in this GitHub repository.)

Open Postman: Launch the Postman Desktop Application.

Import:

Click on the "Import" button in the top-left corner of the Postman interface.

Select the "Upload Files" tab.

Drag and drop both .json collection files into the upload area, or click "Choose Files" and navigate to them.

Click "Import".

The collections will now appear in your "Collections" sidebar.

Configuring Collection Variables (Crucial for Petstore)
For the Swagger Petstore Simplified Tests collection to run correctly, you must ensure the baseUrl and apiKey variables are properly configured as Collection Variables.

Select the Collection: In the Postman sidebar, click on the Swagger Petstore Simplified Tests collection name.

Go to Variables Tab: Click on the "Variables" tab.

Verify/Set Variables: Ensure the following variables are present and have these Current Values:

baseUrl: https://petstore.swagger.io/v2

apiKey: special-key

(Optional) You can also set these as Initial Value if you plan to share the collection, but Current Value is what Postman uses locally.

Save Changes: Click the "Save" button (usually next to the collection name, or the floppy disk icon).

How to Run the Tests
You can run tests in Postman in a few ways:

Run Individual Request:

Expand a collection in the sidebar.

Click on any request (e.g., "GET all Posts").

Click the "Send" button. The response will appear, and the "Test Results" tab (usually below the response) will show the pass/fail status of the tests for that specific request.

Run Entire Collection:

Click on the collection name in the sidebar (e.g., "My Demo API Test Project").

Click the "Run" button (often an arrow icon or labeled "Run Collection").

The Collection Runner will open. You can select which requests to run, set iteration counts, and view a summary of all test results. Click "Run [Collection Name]" to start.

Important Considerations for Swagger Petstore
As mentioned, the petstore.swagger.io is a mock API and does not persist data.

POST requests will always return 200 OK (or 201 Created), but the data you send is NOT saved. This means if you create a pet with ID X and then immediately try to GET pet X, the GET request will likely return a 404 Not Found.

To ensure tests for GET, PUT, and DELETE operations pass consistently, this collection uses pre-defined, static IDs and usernames (7, 8, 9 for pets/orders, user1 for users) that the mock API is configured to recognize and respond to. This is a common strategy when testing against non-persistent mock services.

Contact
For any questions or feedback, please feel free to reach out via my GitHub profile.

License
This project is open-sourced under the MIT License.

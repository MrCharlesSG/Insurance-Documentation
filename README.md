<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->
<!-- TOC --><a name="insurance-documentation"></a>
- [1. Introduction](#1-introduction)
   * [1.1. Overview](#11-overview)
   * [1.2. Purpose](#12-purpose)
   * [1.3. Features](#13-features)
- [2. Project Structure](#2-project-structure)
   * [2.1. Backend (Spring Boot)](#21-backend-spring-boot)
   * [2.2. Frontend (JavaFX)](#22-frontend-javafx)
   * [2.3. Frontend (React Native)](#23-frontend-react-native)
   * [2.4. Documents](#24-documents)
- [3. Getting Started](#3-getting-started)
   * [3.1. Backend](#31-backend)
   * [3.2. JavaFX Frontend](#32-javafx-frontend)
   * [3.3. React Native Frontend](#33-react-native-frontend)
- [4. Spring Boot Application Architecture](#4-spring-boot-application-architecture)
   * [4.1. Authentication](#41-authentication)
      + [**4.1.1. Short-lived `accessToken`**](#411-short-lived-accesstoken)
      + [**4.1.2. Long-lived `token`**](#412-long-lived-token)
      + [**4.1.3. Refreshing the `accessToken`**](#413-refreshing-the-accesstoken)
   * [4.2. Domain](#42-domain)
   * [4.3. Endpoints](#43-endpoints)
- [5. Frontend Integration](#5-frontend-integration)
   * [5.1. JavaFX Frontend](#51-javafx-frontend)
      + [5.1.1. Authentication Flow](#511-authentication-flow)
      + [5.1.2. Handling API Responses](#512-handling-api-responses)
      + [5.1.3. Appearance](#513-appearance)
   * [5.2. React Native Frontend](#52-react-native-frontend)
      + [5.2.1. Authentication Flow](#521-authentication-flow)
      + [5.2.2. Handling API Reponses](#522-handling-api-reponses)
      + [5.2.3. Appearance](#523-appearance)
- [6. Authentication](#6-authentication)
   * [6.1. Overview of JWT](#61-overview-of-jwt)
- [6. API Endpoints](#6-api-endpoints)
   * [6.1. List of Endpoints](#61-list-of-endpoints)
   * [6.2. Auth](#62-auth)
      + [6.2.1. Login](#621-login)
      + [6.2.2. Register](#622-register)
      + [6.2.3. Refresh Token](#623-refresh-token)
      + [6.2.4.  Logout](#624-logout)
   * [6.3. Vehicle](#63-vehicle)
      + [6.3.1. Get Vehicle by ID](#631-get-vehicle-by-id)
      + [6.3.2. Get Vehicle by Plate](#632-get-vehicle-by-plate)
      + [6.3.3. Get All Vehicles](#633-get-all-vehicles)
      + [6.3.4. Create Vehicle](#634-create-vehicle)
      + [6.3.5. Update Vehicle](#635-update-vehicle)
      + [6.3.6. Delete Vehicle](#636-delete-vehicle)

<!-- TOC end -->
# Insurance Documentation


<!-- TOC --><a name="1-introduction"></a>
# 1. Introduction

<!-- TOC --><a name="11-overview"></a>
## 1.1. Overview

The project consists of a Spring Boot server consumed by both JavaFX and React Native frontends. The users, who are vehicles, can manage reports against other vehicles and manage their information. The backend provides a RESTful API that handles authentication via JWT tokens and various operations related to the vehicles and reports. The JavaFX frontend is a desktop application, while the React Native frontend is designed for mobile devices.

<!-- TOC --><a name="12-purpose"></a>
## 1.2. Purpose

This project was developed to fulfill the requirements of three subjects: Secure Coding, Advanced Information System Interoperability, and Advanced Programming Paradigms. The general documentation of the project is provided in this document, while the documentation specific to each subject is located in the documents folder.

The audience for this document includes anyone interested in the development process, as well as those looking to learn about Spring Boot and how to consume its services using JavaFX or React Native.

<!-- TOC --><a name="13-features"></a>
## 1.3. Features

The user stories for the project are:

- As a vehicle, I want to be able to open a report against a specific vehicle, so I can document incidents or issues.
- As a vehicle, I want to be able to log in, log out, and register, so I can access crucial information and functionalities.
- As a vehicle, I want to be able to see all my reports and filter them, so I can clearly review my activities and incidents.
- As a vehicle, I want to be able to associate and dissociate drivers (existing or new), so I can select different drivers for different reports.
- As a vehicle, I want to be able to close a report by rejecting or accepting it, so I can finalize the report's status.
- As a vehicle, I want to be able to see all the details of a report, so I can accurately remember what happened.

<!-- TOC --><a name="2-project-structure"></a>
# 2. Project Structure

<!-- TOC --><a name="21-backend-spring-boot"></a>
## 2.1. Backend (Spring Boot)

- **Repository Link**
    - https://github.com/MrCharlesSG/Insurance-Backend
- **Overview**
    - **Description**: The backend is responsible for handling all business logic and data persistence. It exposes RESTful APIs consumed by the frontends.
    - **Architecture**: Follows the layered architecture pattern with controllers, services, and repositories.
    - **Key Components**:
        - **Controllers**: Handle HTTP requests and responses.
        - **Services**: Contain business logic and interact with repositories.
        - **Repositories**: Interface with the database using JPA.
        - **Domain**: Define the data model and map to database tables.
    - **Technologies**:
        - Spring Boot
        - Hibernate
        - JWT for security
        - Mock H2 for the database

<!-- TOC --><a name="22-frontend-javafx"></a>
## 2.2. Frontend (JavaFX)

- **Repository Link**
    - https://github.com/MrCharlesSG/Insurance-Frontend-JavaFX
- **Overview**
    - **Description**: The JavaFX frontend is a desktop application providing a graphical user interface for users to interact with the system.
    - **Architecture**: Uses the MVC pattern to separate concerns. The controllers are directly related to its views and uses a service layer to access the backend.
    - **Key Components**:
        - **Controllers**: Manage user input, update views, and uses the services to access the backend.
        - **Services**: Implemented as singletons, allows controller access the backend via functions.
        - **Views**: Define the UI layout using FXML.
        - **Models**: Represent the data, has similar entities as the backend.
    - **Technologies**:
        - JavaFX
        - FXML for UI design

<!-- TOC --><a name="23-frontend-react-native"></a>
## 2.3. Frontend (React Native)

- **Repository Link**
    - https://github.com/MrCharlesSG/Insurance-Frontend-React-Native
- **Overview**
    - **Description**: The React Native frontend is a mobile application providing an interface for users to interact with the system on mobile devices.
    - **Architecture**: Follows the component-based architecture.
    - **Key Components**:
        - **Screens**: Represent different views in the application.
        - **Navigation**: Manages the routing between screens.
        - **State Management**: Manages the state using Context API. All of the views are inside a `GlobalProvider` component that allows them to access the Context.
        - **Context**: Provide the global information such as the tokens or the user
    - **Technologies**:
        - React Native
        - React Navigation
        - Context API for state management

<!-- TOC --><a name="24-documents"></a>
## 2.4. Documents

As said before, the project has fulfill the requirements of three different subjects. Each of the subject requieres several specifications that are documented in three different documents located in the documents folder. The content of each of the documents are:

1. **Secure Coding**: JWT tokens, authorization and authentication, secure serialization, SQL Injection, SonarLint Report and OWASP ZAP scan.
2. **Interoperability**: JavaFX frontend, the consumption of an endpoint by Mule ESB, ActiveMQ and jBPM.
3. **Paradigms**: testing, Aspect Oriented Programming (AOP), Git, SOLID principle, Function Programming (FP), metrics and Docker.

Good to highlight that the present document summarize everything is needed for understanding the project.

<!-- TOC --><a name="3-getting-started"></a>
# 3. Getting Started

<!-- TOC --><a name="31-backend"></a>
## 3.1. Backend

There are two ways to set up the backend:

1. **Pulling from the repository**:
    - Clone the repo from [GitHub](https://github.com/MrCharlesSG/Insurance-Backend) and run it using IntelliJ IDEA.
2. **Using Docker**:
    1. **Pull the Image**:
        
        ```bash
        docker pull mrcharlessg/insurance-backend
        ```
        
        This command pulls the `insurance-backend` image from Docker Hub.
        
    2. **Run the Image**:
        
        ```bash
        docker run -p 8082:8081 mrcharlessg/insurance-backend
        ```
        
        This command runs a container from the `mrcharlessg/insurance-backend` image, mapping port 8082 on the host to port 8081 in the container.
        

<!-- TOC --><a name="32-javafx-frontend"></a>
## 3.2. JavaFX Frontend

To run the JavaFX frontend, ensure the backend is running. IntelliJ IDEA is also required.

1. **Run the JavaFX Frontend**:
    - Pull the code from [GitHub](https://github.com/MrCharlesSG/Insurance-Frontend-JavaFX) and run it.
    - The JavaFX application will automatically connect to the server if the port is 8082.
    - If the server is on a different port, update the file: `src/main/java/hr/algebra/javafxinsurance/configuration/ApiConfig.java`.

<!-- TOC --><a name="33-react-native-frontend"></a>
## 3.3. React Native Frontend

1. **Clone the repository**:
    
    ```bash
    git clone <https://github.com/MrCharlesSG/Insurance-Frontend-React-Native.git>
    ```
    
2. **Create a `.env` file** in the root directory of the project.
3. **Add the following line** to the `.env` file (replace `YOUR_IP` with your server's IP address):
    
    ```
    SPRING_BACKEND_URL=http://YOUR_IP:8082/
    ```
    
4. **Navigate into the project directory**:
    
    ```bash
    cd Insurance-Frontend-React-Native
    ```
    
5. **Install dependencies**:
    
    ```bash
    npm install
    ```
    
6. **Start the app**:
    
    ```bash
    npx expo start
    ```
    

<!-- TOC --><a name="4-spring-boot-application-architecture"></a>
# 4. Spring Boot Application Architecture

The Spring Boot application follows the layered architecture pattern with controllers, services, and repositories, where controllers expose and handle the HTTP request, services contain the business process and interact with repositories. This last ones access the database using JPA.

<!-- TOC --><a name="41-authentication"></a>
## 4.1. Authentication

All endpoints in the server, except for **`/auth`** and **`/vehicles`**, are protected. Users need an **`accessToken`** to access the protected endpoints. Nevertheless there are other endpoints not protected. Here’s why those specific endpoints aren’t protected:

- **`/auth`**: This endpoint must be publicly accessible so users can authenticate. Nevertheless the logout feature needs authentication because is the way the server knows who is the user logging out.
- **`/vehicles`**: This endpoint is used in a MuleProject, and managing authentication for all calls through the MuleProject was deemed unnecessary. Therefore, this endpoint is "public" in the current version of the project, as it handles non-sensitive information. It has also been stripped of some key features to make it more manageable. These key features are used by other services but not by the **`VehiclesController`**.

To access a protected endpoint, the user must provide a header with the keyword **`Authorization`** and the value **`Bearer *accessToken*`**.

JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. In this project, JWT is used to manage user authentication and authorization.

Upon successful login or registration, the user receives a data structure like this:

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ1c2VyMSIsImV4cCI6MTY0Nzk4OTU4NX0.Dw7U2YOZH8F4-tUNr1ZQV3QntTe19VR6FN5W_mmfT3s",
  "token": "dGhpcyBpcyBhIHNhbXBsZSB0b2tlbg=="
}
```

When registering, the user also receives their information wrapped in a wrapper.

The **`accessToken`** allows the user to access protected endpoints. However, it is valid for only 10 minutes, while the **`token`** is valid for seven days. Here’s why:

<!-- TOC --><a name="411-short-lived-accesstoken"></a>
### **4.1.1. Short-lived `accessToken`**

The **`accessToken`** is valid for 10 minutes to minimize security risks. Since the **`accessToken`** is used frequently in almost every request to protected endpoints, there is a higher risk of it being intercepted or stolen. By limiting its lifespan, the window of opportunity for an attacker to use a stolen token is significantly reduced, enhancing overall security.

<!-- TOC --><a name="412-long-lived-token"></a>
### **4.1.2. Long-lived `token`**

The **`token`**, on the other hand, is valid for seven days. This longer lifespan provides a more convenient user experience. Users don’t have to log in repeatedly throughout the day. Instead, they can use the **`token`** to refresh their **`accessToken`** when it expires. This way, users only need to log in once a week, balancing security and convenience.

<!-- TOC --><a name="413-refreshing-the-accesstoken"></a>
### **4.1.3. Refreshing the `accessToken`**

To refresh the **`accessToken`**, the user calls the **`/auth/api/v1/refreshToken`** endpoint with the **`token`** in the request body. This process extends the user's session without requiring them to re-enter their credentials frequently, thus maintaining a seamless and secure user experience.

**Sequence Charts in Backend**

**Login**

![https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Secure%20Coding/images/AuthDiagrams-Login.drawio.png](https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Secure%20Coding/images/AuthDiagrams-Login.drawio.png)

**Register**

![https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Secure%20Coding/images/Untitled.png](https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Secure%20Coding/images/Untitled.png)

The JwtReponseAspect is an aspect executed arround the function in the AuthController encharge of registering a vehicle. The scope of this aspect is to standarize the registering by wrapping the info of the just created user in a wrapper. The main reason is because in the future there will be a register of drivers.

**Refresh Token**

![https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Secure%20Coding/images/Untitled%201.png](https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Secure%20Coding/images/Untitled%201.png)

<!-- TOC --><a name="42-domain"></a>
## 4.2. Domain

![AssuranceDiagrams3.png](readme-resources/readme-images/AssuranceDiagrams3.png)

<!-- TOC --><a name="43-endpoints"></a>
## 4.3. Endpoints

As previously mentioned, almost all endpoints are protected. When an endpoint is called, a filter determines whether the user is authenticated by checking for a valid `accessToken` in the Authentication header.

All endpoints are validated using aspects that are triggered before the corresponding functions execute. There are five aspects responsible for validating various inputs: `AuthRequestValidatorAspect`, `SignUpVehicleValidatorAspect`, `DriverValidatorAspect`, `EmailValidatorAspect`, and `ReportValidationAspect`.

The server does not return domain entities directly; instead, it returns Data Transfer Objects (DTOs). This approach provides an abstraction layer between the database and the client.

The typical flow of a RESTful call is as follows:

1. The HTTP call is made and the controller handles it.
2. An aspect checks the input of the call (if required).
3. The controller calls the relevant service. Sometimes, the service needs to call other services and eventually its repository (e.g., `ReportService` calls `ReportRepository` at the end of its function). If a controller needs to access a new repository, it must do so through a service, even if the required data is raw.
4. The service calls the repository and returns the data mapped into a DTO.

![Untitled Diagram.drawio.png](readme-resources/readme-images/Untitled_Diagram.drawio.png)

It is worth highlighting that all endpoints have comprehensive integration tests to ensure their functionality and reliability. The entire project adheres to SOLID principles, promoting maintainable and scalable code. Additionally, several services are developed using the functional programming paradigm, enhancing code readability and reducing side effects.

The backend also exposes various metrics for monitoring and performance analysis. For more detailed information, please refer to the document located at  https://github.com/MrCharlesSG/Insurance-Documentation/blob/main/documents/Paradigms/Paradigms.md

<!-- TOC --><a name="5-frontend-integration"></a>
# 5. Frontend Integration

<!-- TOC --><a name="51-javafx-frontend"></a>
## 5.1. JavaFX Frontend

The JavaFX frontend is a desktop application providing a graphical user interface for users to interact with the system. Uses the MVC pattern to separate concerns. The controllers are directly related to its views and uses a service layer to access the backend. 

<!-- TOC --><a name="511-authentication-flow"></a>
### 5.1.1. Authentication Flow

The tokens in the frontend are mainly managed by the `TokenService` class. This class is encharge of providing the `accessToken`, initializing the session and refreshing the `accessToken` automatically till the `token` gets invalid. When closing the application and opening again the user won’t need to login again (if the `token` is valid), how is done this?

Every time we login or we refresh the tokens, the `TokenService` will serialize and save the tokens and its expirations days. So whenever we initialize the app, the `TokenService` will look to this serialized tokens and refresh them if the `token` is valid. If there is not token serialized or is expired the login screen is shown.

To see how the frontend securely serialize the information see this section: [https://github.com/MrCharlesSG/Insurance-Documentation/blob/main/documents/Secure Coding/Secure Coding.md#serialization](https://github.com/MrCharlesSG/Insurance-Documentation/blob/main/documents/Secure%20Coding/Secure%20Coding.md#serialization)

The automatically refresh token works as follow: a client ask for the `accessToken` by calling the `getAccessToken` function in the `TokenService` class which will return an String with the `accessToken`, and we have 4 options:

1. There is no access token in these case there is two possibilities:
    1. The token is serialized, so we call the `loadToken`
    2. There is no serialized token and the client miss call the function. In this case a `RuntimeException` is thrown.
2. The `token` has expired, so we throw an `IllegalAccessException` that must be managed by all the clients. The normal flow is that the clients send a message to the user and open the login screen.
3. The `accessToken` has expired but the `token` not, so we refresh the token by calling the REST API.
4. Everything is ok and we just give the client the `accessToken`.

<!-- TOC --><a name="512-handling-api-responses"></a>
### 5.1.2. Handling API Responses

Each endpoint from the REST API that the frontend consumes has its own singleton service for three main reasons:

1. **Shared Functionality:** The **`/drivers`** and **`/reports`** endpoints are protected, leading to some common functions for both.
2. **Controller Reuse:** Each view in the application needs a controller, and some controllers will use the same functions.
3. **Separation of Concerns:** The primary function of the controllers is to connect the views with the backend. In this small application, the controllers also manage view logic, but the backend connection and underlying operations are handled by the services.

Here is an example of how functions in the services are structured:

```java
public enum DriverService { 
    INSTANCE;

    private final String BASIC_URL = API_URL + "/driver";
    private final String GET_BY_EMAIL_URL = BASIC_URL + "/byEmail?email=";
    private final String ASSOCIATE_BY_EMAIL_URL = BASIC_URL + "/associate?email=";
    private final String DISASSOCIATE_BY_EMAIL_URL = BASIC_URL + "/disassociate?email=";

    ...

    public DriverDTO getDriverByEmail(String email) throws IllegalAccessException {
        HttpEntity<String> entity = new HttpEntity<>(AuthService.INSTANCE.getAuthHeader());

        RestTemplate restTemplate = new RestTemplate();
        ParameterizedTypeReference<DriverDTO> responseType = new ParameterizedTypeReference<DriverDTO>() {};

        ResponseEntity<DriverDTO> responseEntity = restTemplate.exchange(
                GET_BY_EMAIL_URL + email,
                HttpMethod.GET,
                entity,
                responseType
        );
        return responseEntity.getBody();
    }

    public void createDriverAndAssociate(DriverDTO driver) throws Exception {
        HttpEntity<DriverDTO> requestEntity = new HttpEntity<>(driver, AuthService.INSTANCE.getAuthHeader());

        RestTemplate restTemplate = new RestTemplate();
        ResponseEntity<DriverDTO> response = restTemplate.exchange(
                BASIC_URL,
                HttpMethod.POST,
                requestEntity,
                DriverDTO.class
        );

        if (response.getStatusCode().isError()) {
            throw new Exception("Failed to create: " + response.getBody());
        }
    }

    ...
}
```

All functions follow the same structure:

1. **Prepare the Header and Body:** Create the **`requestEntity`**.
2. **Instantiate the `RestTemplate`:** Set up the **`RestTemplate`** instance.
3. **Make the Request:** Use the **`RestTemplate`** to make an exchange, specifying:
    - The URL
    - The HTTP method
    - The **`requestEntity`**
    - The expected response class
4. **Handle the Response:** Check for errors or return the response body directly.

Since `/drivers` and `/reports` are protected endpoints, the `AuthService` sets the authorization header. 

The date when is logged is stored twice, one for the `accessToken` and the other for the `refreshToken`. This date serves the frontend to see:

1. When the `accessToken` is expired, so it can be refreshed and it date is updated
2. When the `refreshToken` is expired, so can be thrown an `IllegalAccessException` and the controller moves the user to the login screen.

<!-- TOC --><a name="513-appearance"></a>
### 5.1.3. Appearance

<p align="center">
  <img src="https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Interoperability/images/Untitled%202.png" width="45%" height="45%" />
  <img src="https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Interoperability/images/Untitled%203.png" width="45%" height="45%" />
</p>
<p align="center">
  <img src="https://github.com/MrCharlesSG/Insurance-Documentation/raw/main/documents/Interoperability/images/Untitled%205.png" width="45%" height="45%" />
</p>


For more images see the section [https://github.com/MrCharlesSG/Insurance-Documentation/blob/main/documents/Interoperability/Interoperability.md#appearance](https://github.com/MrCharlesSG/Insurance-Documentation/blob/main/documents/Interoperability/Interoperability.md#appearance)

<!-- TOC --><a name="52-react-native-frontend"></a>
## 5.2. React Native Frontend

The React Native frontend is a mobile application providing an interface for users to interact with the system on mobile devices. Follows the component-based architecture, where each of the elements of the views are components. The components uses the `GlobalProvider` to call the functions that consume the RESTful API. 

This `GlobalProvider` is the component where all the views are wrapped and enable them to use the Context. This context store `useState` for managing the user logged, the tokens , and to set if the user is logged or not. 

<!-- TOC --><a name="521-authentication-flow"></a>
### 5.2.1. Authentication Flow

The way the React Native application authenticate is similar to the JavaFX frontend. 

1. The user log in and the session is saved. As session we refer to the `token`, `accessToken`, `date` for both tokens (so can check quickly its validity) and the `username` to access its information. 
    1. In the Context, the `isLogged` is set to true, and the token and the user is saved
    2. The session is stored in the `AsyncStorage`
2. The user request something and the context is send in the params of the functions
3. The functions call the `getAccessToken` of the `sb-auth.js` file. This function
    1. Check if the `token` of the context is expired, if so it remove session (from context and storage) and send the user to the log in screen.
    2. Check if the `accessToken` is expired. If so it refresh the session.
    3. The access token is returned
4. The functions call the API with the `accessToken` if everything went correctly.

<!-- TOC --><a name="522-handling-api-reponses"></a>
### 5.2.2. Handling API Reponses

All functions that access the backend need the context for getting the tokens and are separated in files according to what they access: `sb-driver.js`,`sb-report.js`,`sb-vehicle.js`,`sb-auth`, and `sb-token.js` (this file also store the logic for “storing” the sessions). All the functions uses `axios` to call the REST API.

A normal function could be like this ones:

```jsx
export async function acceptReport(context, reportId, damages){
  const accessToken = await getAccessToken(context)
  const authHeader = 'Bearer ' + accessToken
  const url = SPRING_BACKEND_URL+'report/accept/' + reportId
  const body = {
    damages
  }
  const header = {
    Authorization: authHeader
  }
  await axios.delete(url, {headers: header, data: body})
      .then(response => (response.data))
      .catch(error => {
        throw new error.message
      })
}
```

<!-- TOC --><a name="523-appearance"></a>
### 5.2.3. Appearance

To see better how the app works here is a video of 1 minute of all the use cases: [Link to video](https://github.com/MrCharlesSG/Insurance-Documentation/blob/main/readme-resources/readme-videos/react-native-usecase.mp4)

<p align="center">
  <img src="readme-resources/readme-images/WhatsApp_Image_2024-07-27_at_18.50.00_(2).jpeg" width="45%" height="45%" />
  <img src="readme-resources/readme-images/WhatsApp_Image_2024-07-27_at_18.49.59.jpeg" width="45%" height="45%" />
</p>
<p align="center">
  <img src="readme-resources/readme-images/WhatsApp_Image_2024-07-27_at_18.50.00_(1).jpeg" width="45%" height="45%" />
  <img src="readme-resources/readme-images/WhatsApp_Image_2024-07-27_at_18.49.59_(1).jpeg" width="45%" height="45%" />
</p>



<!-- TOC --><a name="6-authentication"></a>
# 6. Authentication

<!-- TOC --><a name="61-overview-of-jwt"></a>
## 6.1. Overview of JWT

JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. In this project, JWT is used to manage user authentication and authorization.

<!-- TOC --><a name="6-api-endpoints"></a>
# 6. API Endpoints

<!-- TOC --><a name="61-list-of-endpoints"></a>
## 6.1. List of Endpoints

- Auth
    - Login
    - Register
    - Refresh Token
    - Log Out
- Vehicles
    - **Vehicle by ID**
    - **Vehicle by Plate**
    - **All Vehicles**
    - **Create Vehicle**
    - **Update Vehicle**
    - **Delete Vehicle**
- Reports
    - **All Reports**
    - **Create Report**
    - **Waiting Reports**
    - **Rejected Reports**
    - **Accepted Reports**
- Driver
    - **Driver by ID**
    - **Driver by Vehicle**
    - **Driver by Email**
    - **Associate Driver**
    - **Disassociate Driver**

<!-- TOC --><a name="62-auth"></a>
## 6.2. Auth

<!-- TOC --><a name="621-login"></a>
### 6.2.1. Login

- **URL**: `/auth/api/v1/login`
- **Method**: POST
- **Request Format**:
    
    ```json
    {
      "username": "string",
      "password": "string"
    }
    
    ```
    
- **Response Format**:
    
    ```json
    {
      "accessToken": "string",
      "token": "string"
    }
    
    ```
    

<!-- TOC --><a name="622-register"></a>
### 6.2.2. Register

The register in the V1.0 of the application is only available for vehicles, nevertheless for scalability measures the response return the wrapped information of the new vehicle. 

- **URL**: `/auth/api/v1/register/vehicle`
- **Method**: POST
- **Request Format**:
    
    ```json
    {
      "username": "string",
      "password": "string",
      "matchingPassword": "string",
      "vehicle": {
        "plate": "string",
        "brand": "string",
        "model": "string",
        "manufacturingYear": {
          "value": "integer",
          "leap": "boolean"
        }
      }
    }
    
    ```
    
- **Response Format**:
    
    ```json
    {
      "token":{
    	  "accessToken": "string",
    	  "token": "string"
      }
      "wrapped": {
        "plate": "string",
        "brand": "string",
        "model": "string",
        "manufacturingYear": {
          "value": "integer",
          "leap": "boolean"
        }
      }
    }
    
    ```
    

<!-- TOC --><a name="623-refresh-token"></a>
### 6.2.3. Refresh Token

- **URL**: `/auth/api/v1/refresh`
- **Method**: POST
- **Request Format**:
    
    ```json
    {
      "refreshToken": "string"
    }
    
    ```
    
- **Response Format**:
    
    ```json
    {
      "accessToken": "string",
      "refreshToken": "string"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X POST "<http://localhost:8082/auth/api/v1/refresh>" -H "Content-Type: application/json" -d '{
      "refreshToken": "some-refresh-token"
    }'
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "accessToken": "new-access-token",
      "refreshToken": "new-refresh-token"
    }
    
    ```
    

<!-- TOC --><a name="624-logout"></a>
### 6.2.4.  Logout

This endpoint is authenticated, so that is how the backend knows who is the user logging out

- **URL**: `/auth/api/v1/logout`
- **Method**: POST
- **Request Format**: None
- **Response Format**:
    
    ```json
    {
      "message": "Logged out successfully"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X POST "<http://localhost:8082/auth/api/v1/logout>"
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "message": "Logged out successfully"
    }
    ```
    

<!-- TOC --><a name="63-vehicle"></a>
## 6.3. Vehicle

<!-- TOC --><a name="631-get-vehicle-by-id"></a>
### 6.3.1. Get Vehicle by ID

- **URL**: `/vehicles/{id}`
- **Method**: GET
- **Request Format**:
    - Path Parameter:
        
        ```json
        {
          "id": "integer"
        }
        
        ```
        
- **Response Format**:
    
    ```json
    {
      "plate": "string",
      "brand": "string",
      "model": "string",
      "manufacturingYear": {
        "value": "integer",
        "leap": "boolean"
      },
      "id": "integer",
      "username": "string"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/vehicles/1>"
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "plate": "ABC123",
      "brand": "Toyota",
      "model": "Corolla",
      "manufacturingYear": {
        "value": 2020,
        "leap": false
      },
      "id": 1,
      "username": "user1"
    }
    
    ```
    

<!-- TOC --><a name="632-get-vehicle-by-plate"></a>
### 6.3.2. Get Vehicle by Plate

- **URL**: `/vehicles/byPlate`
- **Method**: GET
- **Request Format**:
    - Query Parameter:
        
        ```json
        {
          "plate": "string"
        }
        
        ```
        
- **Response Format**:
    
    ```json
    {
      "plate": "string",
      "brand": "string",
      "model": "string",
      "manufacturingYear": {
        "value": "integer",
        "leap": "boolean"
      },
      "id": "integer",
      "username": "string"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/vehicles/byPlate?plate=ABC123>"
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "plate": "ABC123",
      "brand": "Toyota",
      "model": "Corolla",
      "manufacturingYear": {
        "value": 2020,
        "leap": false
      },
      "id": 1,
      "username": "user1"
    }
    
    ```
    

<!-- TOC --><a name="633-get-all-vehicles"></a>
### 6.3.3. Get All Vehicles

- **URL**: `/vehicles`
- **Method**: GET
- **Request Format**: None
- **Response Format**:
    
    ```json
    [
      {
        "plate": "string",
        "brand": "string",
        "model": "string",
        "manufacturingYear": {
          "value": "integer",
          "leap": "boolean"
        },
        "id": "integer",
        "username": "string"
      }
    ]
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/vehicles>"
    
    ```
    
- **Sample Response**:
    
    ```json
    [
      {
        "plate": "ABC123",
        "brand": "Toyota",
        "model": "Corolla",
        "manufacturingYear": {
          "value": 2020,
          "leap": false
        },
        "id": 1,
        "username": "user1"
      },
      {
        "plate": "XYZ789",
        "brand": "Honda",
        "model": "Civic",
        "manufacturingYear": {
          "value": 2019,
          "leap": false
        },
        "id": 2,
        "username": "user2"
      }
    ]
    
    ```
    

<!-- TOC --><a name="634-create-vehicle"></a>
### 6.3.4. Create Vehicle

- **URL**: `/vehicles`
- **Method**: POST
- **Request Format**:
    
    ```json
    {
      "plate": "string",
      "brand": "string",
      "model": "string",
      "manufacturingYear": {
        "value": "integer",
        "leap": "boolean"
      }
    }
    
    ```
    
- **Response Format**:
    
    ```json
    {
      "plate": "string",
      "brand": "string",
      "model": "string",
      "manufacturingYear": {
        "value": "integer",
        "leap": "boolean"
      },
      "id": "integer",
      "username": "string"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X POST "<http://localhost:8082/vehicles>" -H "Content-Type: application/json" -d '{
      "plate": "ABC123",
      "brand": "Toyota",
      "model": "Corolla",
      "manufacturingYear": {
        "value": 2020,
        "leap": false
      }
    }'
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "plate": "ABC123",
      "brand": "Toyota",
      "model": "Corolla",
      "manufacturingYear": {
        "value": 2020,
        "leap": false
      },
      "id": 1,
      "username": "user1"
    }
    
    ```
    

<!-- TOC --><a name="635-update-vehicle"></a>
### 6.3.5. Update Vehicle

- **URL**: `/vehicles/{id}`
- **Method**: PUT
- **Request Format**:
    - Path Parameter:
        
        ```json
        {
          "id": "integer"
        }
        
        ```
        
    - Request Body:
        
        ```json
        {
          "plate": "string",
          "brand": "string",
          "model": "string",
          "manufacturingYear": {
            "value": "integer",
            "leap": "boolean"
          }
        }
        
        ```
        
- **Response Format**:
    
    ```json
    {
      "plate": "string",
      "brand": "string",
      "model": "string",
      "manufacturingYear": {
        "value": "integer",
        "leap": "boolean"
      },
      "id": "integer",
      "username": "string"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X PUT "<http://localhost:8082/vehicles/1>" -H "Content-Type: application/json" -d '{
      "plate": "ABC123",
      "brand": "Toyota",
      "model": "Corolla",
      "manufacturingYear": {
        "value": 2020,
        "leap": false
      }
    }'
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "plate": "ABC123",
      "brand": "Toyota",
      "model": "Corolla",
      "manufacturingYear": {
        "value": 2020,
        "leap": false
      },
      "id": 1,
      "username": "user1"
    }
    
    ```
    

<!-- TOC --><a name="636-delete-vehicle"></a>
### 6.3.6. Delete Vehicle

- **URL**: `/vehicles/{id}`
- **Method**: DELETE
- **Request Format**:
    - Path Parameter:
        
        ```json
        {
          "id": "integer"
        }
        
        ```
        
- **Response Format**:
    
    ```json
    {
      "message": "Vehicle deleted successfully"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X DELETE "<http://localhost:8082>
    
    ```
    

/vehicles/1"
```

- **Sample Response**:
    
    ```json
    {
      "message": "Vehicle deleted successfully"
    }
    
    ```
    

## 6.4. Reports

### 6.4.1. Get All Reports

- **URL**: `/reports`
- **Method**: GET
- **Request Format**: None
- **Response Format**:
    
    ```json
    [
      {
        "id": "integer",
        "description": "string",
        "status": "string",
        "vehicleId": "integer",
        "createdAt": "string",
        "updatedAt": "string"
      }
    ]
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/reports>"
    
    ```
    
- **Sample Response**:
    
    ```json
    [
      {
        "id": 1,
        "description": "Report 1",
        "status": "Pending",
        "vehicleId": 1,
        "createdAt": "2024-07-28T14:30:00Z",
        "updatedAt": "2024-07-28T14:30:00Z"
      },
      {
        "id": 2,
        "description": "Report 2",
        "status": "Accepted",
        "vehicleId": 2,
        "createdAt": "2024-07-28T14:30:00Z",
        "updatedAt": "2024-07-28T14:30:00Z"
      }
    ]
    
    ```
    

### 6.4.2. Create Report

- **URL**: `/reports`
- **Method**: POST
- **Request Format**:
    
    ```json
    {
      "description": "string",
      "status": "string",
      "vehicleId": "integer"
    }
    
    ```
    
- **Response Format**:
    
    ```json
    {
      "id": "integer",
      "description": "string",
      "status": "string",
      "vehicleId": "integer",
      "createdAt": "string",
      "updatedAt": "string"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X POST "<http://localhost:8082/reports>" -H "Content-Type: application/json" -d '{
      "description": "Report 1",
      "status": "Pending",
      "vehicleId": 1
    }'
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "id": 1,
      "description": "Report 1",
      "status": "Pending",
      "vehicleId": 1,
      "createdAt": "2024-07-28T14:30:00Z",
      "updatedAt": "2024-07-28T14:30:00Z"
    }
    
    ```
    

### 6.4.3. Get Waiting Reports

- **URL**: `/reports/waiting`
- **Method**: GET
- **Request Format**: None
- **Response Format**:
    
    ```json
    [
      {
        "id": "integer",
        "description": "string",
        "status": "string",
        "vehicleId": "integer",
        "createdAt": "string",
        "updatedAt": "string"
      }
    ]
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/reports/waiting>"
    
    ```
    
- **Sample Response**:
    
    ```json
    [
      {
        "id": 1,
        "description": "Report 1",
        "status": "Pending",
        "vehicleId": 1,
        "createdAt": "2024-07-28T14:30:00Z",
        "updatedAt": "2024-07-28T14:30:00Z"
      }
    ]
    
    ```
    

### 6.4.4. Get Rejected Reports

- **URL**: `/reports/rejected`
- **Method**: GET
- **Request Format**: None
- **Response Format**:
    
    ```json
    [
      {
        "id": "integer",
        "description": "string",
        "status": "string",
        "vehicleId": "integer",
        "createdAt": "string",
        "updatedAt": "string"
      }
    ]
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/reports/rejected>"
    
    ```
    
- **Sample Response**:
    
    ```json
    [
      {
        "id": 1,
        "description": "Report 1",
        "status": "Rejected",
        "vehicleId": 1,
        "createdAt": "2024-07-28T14:30:00Z",
        "updatedAt": "2024-07-28T14:30:00Z"
      }
    ]
    
    ```
    

### 6.4.5. Get Accepted Reports

- **URL**: `/reports/accepted`
- **Method**: GET
- **Request Format**: None
- **Response Format**:
    
    ```json
    [
      {
        "id": "integer",
        "description": "string",
        "status": "string",
        "vehicleId": "integer",
        "createdAt": "string",
        "updatedAt": "string"
      }
    ]
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/reports/accepted>"
    
    ```
    
- **Sample Response**:
    
    ```json
    [
      {
        "id": 1,
        "description": "Report 1",
        "status": "Accepted",
        "vehicleId": 1,
        "createdAt": "2024-07-28T14:30:00Z",
        "updatedAt": "2024-07-28T14:30:00Z"
      }
    ]
    
    ```
    

## 6.5. Driver

### 6.5.1. Get Driver by ID

- **URL**: `/drivers/{id}`
- **Method**: GET
- **Request Format**:
    - Path Parameter:
        
        ```json
        {
          "id": "integer"
        }
        
        ```
        
- **Response Format**:
    
    ```json
    {
      "id": "integer",
      "name": "string",
      "email": "string",
      "vehicleId": "integer"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/drivers/1>"
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "id": 1,
      "name": "John Doe",
      "email": "john.doe@example.com",
      "vehicleId": 1
    }
    
    ```
    

### 6.5.2. Get Driver by Vehicle

- **URL**: `/drivers/byVehicle`
- **Method**: GET
- **Request Format**:
    - Query Parameter:
        
        ```json
        {
          "vehicleId": "integer"
        }
        
        ```
        
- **Response Format**:
    
    ```json
    {
      "id": "integer",
      "name": "string",
      "email": "string",
      "vehicleId": "integer"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/drivers/byVehicle?vehicleId=1>"
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "id": 1,
      "name": "John Doe",
      "email": "john.doe@example.com",
      "vehicleId": 1
    }
    
    ```
    

### 6.5.3. Get Driver by Email

- **URL**: `/drivers/byEmail`
- **Method**: GET
- **Request Format**:
    - Query Parameter:
        
        ```json
        {
          "email": "string"
        }
        
        ```
        
- **Response Format**:
    
    ```json
    {
      "id": "integer",
      "name": "string",
      "email": "string",
      "vehicleId": "integer"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X GET "<http://localhost:8082/drivers/byEmail?email=john.doe@example.com>"
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "id": 1,
      "name": "John Doe",
      "email": "john.doe@example.com",
      "vehicleId": 1
    }
    
    ```
    

### 6.5.4. Associate Driver

- **URL**: `/drivers/associate`
- **Method**: POST
- **Request Format**:
    
    ```json
    {
      "driverId": "integer",
      "vehicleId": "integer"
    }
    
    ```
    
- **Response Format**:
    
    ```json
    {
      "message": "Driver associated successfully"
    }
    
    ```
    
- **Sample Request**:
    
    ```bash
    curl -X POST "<http://localhost:8082/drivers/associate>" -H "Content-Type: application/json" -d '{
      "driverId": 1,
      "vehicleId": 1
    }'
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "message": "Driver associated successfully"
    }
    
    ```
    

### 6.5.5. Disassociate Driver

- **URL**: `/drivers/disassociate`
- **Method**: POST
- **Request Format**:
    
    ```json
    {
      "driverId": "integer",
      "vehicleId": "integer"
    }
    
    ```
    
- *Response

Format**:
`json     {       "message": "Driver disassociated successfully"     }`

- **Sample Request**:
    
    ```bash
    curl -X POST "<http://localhost:8082/drivers/disassociate>" -H "Content-Type: application/json" -d '{
      "driverId": 1,
      "vehicleId": 1
    }'
    
    ```
    
- **Sample Response**:
    
    ```json
    {
      "message": "Driver disassociated successfully"
    }
    
    ```

# Business Module API Documentation

## Overview

The Business Module provides comprehensive API endpoints for managing business operations, employees, business types, configurations, and connections. All endpoints use RESTful HTTP POST requests with JSON payloads.

## Base Information

- **Base URL**: Configured via `AppServer.shared().getRestApiEndPointFor()`
- **Content-Type**: `application/json`
- **Authentication**: User ID and Business ID required in most requests
- **Response Format**: JSON with standard structure including status, message, and payload

## Standard Response Format

All API responses follow this structure:

```json
{
  "status": "success|error",
  "message": "Response message",
  "payload": "Response data",
  "Event": "Event name (optional)"
}
```

## 1. Business Management API

### 1.1 Retrieve Business

**Endpoint**: `POST /business/retrieve`

**Description**: Retrieves all businesses for a user

**Request Model**:
```kotlin
@Serializable
data class RetrieveBusinessRequest(
    @SerialName("user_id") val UserID: String,
    @SerialName("mobile_number") val MobileNumber: String,
    @SerialName("dial_code") val dialCode: String,
)
```

**Response Model**:
```kotlin
@Serializable
data class RetrieveBusinessResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("payload") val payload: List<Business>,
)
```

**Usage Example**:
```kotlin
val request = RetrieveBusinessRequest(
    UserID = "user123",
    MobileNumber = "9876543210",
    dialCode = "+91"
)

BusinessNetwork.shared().retrieve(request) { response ->
    response?.let { 
        // Handle successful response
        val businesses = it.payload
    } ?: run {
        // Handle error
    }
}
```

### 1.2 Retrieve Single Business

**Endpoint**: `POST /business/retrieve-single`

**Description**: Retrieves a specific business by ID

**Request Model**:
```kotlin
@Serializable
data class RetrieveSingleBusinessRequest(
    @SerialName("_id") val id: String = "",
    @SerialName("Number") val number: Long = 0,
)
```

**Response Model**:
```kotlin
@Serializable
data class RetrieveSingleBusinessResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("payload") val payload: Business,
)
```

### 1.3 Create Business

**Endpoint**: `POST /business/create`

**Description**: Creates a new business

**Request Model**:
```kotlin
@Serializable
data class CreateBusinessRequest(
    @SerialName("business_type_id") val businessTypeID: String,
    @SerialName("user_id") val UserID: String,
    @SerialName("UserName") val UserName: String,
    @SerialName("mobile_number") val MobileNumber: String,
    @SerialName("dial_code") val dialCode: String,
    @SerialName("country_code") val countryCode: String,
    @SerialName("email_id") val email: String,
    @SerialName("name") val name: String,
    @SerialName("address") val Address: Address,
    @SerialName("note_one") val noteOne: String,
    @SerialName("note_two") val noteTwo: String,
)
```

**Response Model**:
```kotlin
@Serializable
data class CreateBusinessResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("payload") val payload: Business?,
)
```

### 1.4 Update Business

**Endpoint**: `POST /business/update`

**Description**: Updates an existing business

**Request Model**:
```kotlin
@Serializable
data class UpdateBusinessRequest(
    @SerialName("_id") val _id: String,
    @SerialName("mobile_number") val mobile: String,
    @SerialName("dial_code") val dialCode: String,
    @SerialName("email_id") val email: String,
    @SerialName("name") val name: String,
    @SerialName("address") val address: Address,
)
```

### 1.5 Find Business

**Endpoint**: `POST /business/find`

**Description**: Searches for businesses

**Request Model**:
```kotlin
@Serializable
data class FindBusinessRequest(
    @SerialName("user_id") val UserID: String,
    @SerialName("mobile_number") val mobile: String,
    @SerialName("dial_code") val dialCode: String,
)
```

### 1.6 Find Nearby Business

**Endpoint**: `POST /business/nearby`

**Description**: Finds businesses near a specific location

**Request Model**:
```kotlin
@Serializable
data class FindNearbyBusinessRequest(
    @SerialName("user_id") val UserID: String,
    @SerialName("Latitude") val latitude: Double,
    @SerialName("Longitude") val longitude: Double,
)
```

## 2. Employee Management API

### 2.1 Find User

**Endpoint**: `POST /employee/find-user`

**Description**: Searches for users by criteria

**Request Model**:
```kotlin
@Serializable
data class FindUserRequest(
    @SerialName("user_id") val UserID: String,
    @SerialName("mobile_number") val mobile: String,
    @SerialName("dial_code") val dialCode: String,
)
```

**Response Model**:
```kotlin
@Serializable
data class FindUserResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("payload") val payload: List<Employee>,
)
```

### 2.2 Create Employee

**Endpoint**: `POST /employee/create`

**Description**: Creates a new employee

**Request Model**:
```kotlin
@Serializable
data class CreateEmployeeRequest(
    @SerialName("user_id") val UserID: String,
    @SerialName("business_id") val BusinessID: String,
    @SerialName("name") val name: String,
    @SerialName("email") val email: String,
    @SerialName("mobile_number") val mobile: String,
    @SerialName("dial_code") val dialCode: String,
    @SerialName("role") val role: String,
)
```

**Response Model**:
```kotlin
@Serializable
data class CreateEmployeeResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("payload") val payload: Employee?,
)
```

### 2.3 Update Employee

**Endpoint**: `POST /employee/update`

**Description**: Updates an existing employee

**Request Model**:
```kotlin
@Serializable
data class UpdateEmployeeRequest(
    @SerialName("_id") val _id: String,
    @SerialName("user_id") val UserID: String,
    @SerialName("business_id") val BusinessID: String,
    @SerialName("name") val name: String,
    @SerialName("email") val email: String,
    @SerialName("mobile_number") val mobile: String,
    @SerialName("dial_code") val dialCode: String,
    @SerialName("role") val role: String,
)
```

### 2.4 Retrieve Business Employees

**Endpoint**: `POST /employee/retrieve`

**Description**: Retrieves all employees for a business

**Request Model**:
```kotlin
@Serializable
data class FetchBusinessEmployeeRequest(
    @SerialName("business_id") val BusinessID: String,
    @SerialName("user_id") val UserID: String,
)
```

### 2.5 Retrieve User Employees

**Endpoint**: `POST /employee/retrieve-for-user`

**Description**: Retrieves all employees for a user

**Request Model**:
```kotlin
@Serializable
data class FetchEmployeeRequest(
    @SerialName("user_id") val UserID: String,
)
```

## 3. Business Type API

### 3.1 Retrieve Business Types

**Endpoint**: `POST /business-type/retrieve`

**Description**: Retrieves all business types

**Request Model**:
```kotlin
@Serializable
data class BusinessTypeRequest(
    @SerialName("business_id") val BusinessID: String,
)
```

**Response Model**:
```kotlin
@Serializable
data class BusinessTypeResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("payload") val payload: List<BusinessType>,
)
```

## 4. Business Connection API

### 4.1 Retrieve Business Connections

**Endpoint**: `POST /business-connection/retrieve`

**Description**: Retrieves business connections

**Request Model**:
```kotlin
@Serializable
data class BusinessConnectionRetrieveRequest(
    @SerialName("business_id") val BusinessID: String,
    @SerialName("user_id") val UserID: String,
)
```

**Response Model**:
```kotlin
@Serializable
data class BusinessConnectionRetrieveResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("payload") val payload: List<BusinessConnection>,
)
```

### 4.2 Create Business Connection

**Endpoint**: `POST /business-connection/create`

**Description**: Creates a new business connection

**Request Model**:
```kotlin
@Serializable
data class BusinessConnectionCreateRequest(
    @SerialName("from_user_id") val fromUserID: String,
    @SerialName("from_business_id") val fromBusinessID: String,
    @SerialName("to_user_id") val toUserID: String,
    @SerialName("to_business_id") val toBusinessID: String,
    @SerialName("name") val name: String,
)
```

**Response Model**:
```kotlin
@Serializable
data class BusinessConnectionCreateResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("payload") val payload: Notification,
)
```

## 5. Business Configuration API

### 5.1 Create Business Config

**Endpoint**: `POST /business-config/create`

**Description**: Creates business configuration

**Request Model**:
```kotlin
@Serializable
data class CreateBusinessConfigRequest(
    @SerialName("user_id") val UserID: String,
    @SerialName("business_id") val BusinessID: String,
    @SerialName("name") val name: String,
    @SerialName("dial_code") val dialCode: String,
    @SerialName("mobile_number") val mobile: String,
    @SerialName("email_id") val email: String,
    @SerialName("bar_code") val Barcode: String,
    @SerialName("device_id") val deviceID: String,
)
```

**Response Model**:
```kotlin
@Serializable
data class CreateBusinessConfigResponse(
    @SerialName("status") val status: String,
    @SerialName("message") val message: String,
    @SerialName("Event") val event: String,
    @SerialName("payload") val payload: BusinessConfig?,
)
```

### 5.2 Update Business Config

**Endpoint**: `POST /business-config/update`

**Description**: Updates business configuration

**Request Model**:
```kotlin
@Serializable
data class UpdateBusinessConfigRequest(
    @SerialName("_id") val id: String,
    @SerialName("user_id") val UserID: String,
    @SerialName("business_id") val BusinessID: String,
    @SerialName("currency") var currency: Currency? = null,
)
```

### 5.3 Retrieve Business Config

**Endpoint**: `POST /business-config/retrieve`

**Description**: Retrieves business configuration

**Request Model**:
```kotlin
@Serializable
data class FetchBusinessConfigRequest(
    @SerialName("business_id") val BusinessID: String,
    @SerialName("user_id") val UserID: String,
    @SerialName("LastSyncDate") val LastSyncDate: String
)
```

### 5.4 Retrieve All Currency

**Endpoint**: `POST /currency/retrieve`

**Description**: Retrieves all available currencies

**Request Model**:
```kotlin
@Serializable
data class FetchAllCurrencyRequest(
    @SerialName("user_id") val UserID: String,
)
```

### 5.5 Retrieve All Products

**Endpoint**: `POST /product/retrieve`

**Description**: Retrieves all products for a business

**Request Model**:
```kotlin
@Serializable
data class RetrieveProductRequest(
    @SerialName("business_id") val BusinessID: String,
    @SerialName("user_id") val UserID: String,
)
```

## Data Models

### Business Model
```kotlin
@Serializable
data class Business(
    @SerialName("_id") var _id: String = UniqueueID(),
    @SerialName("business_id") var BusinessId: Long = Clock.System.now().epochSeconds,
    @SerialName("user_id") var UserID: String = "",
    @SerialName("business_type_id") var BusinessTypeID: String = "",
    @SerialName("name") var Name: String = "",
    @SerialName("address") var Address: Address? = null,
    @SerialName("mobile_number") var MobileNumber: String = "",
    @SerialName("dial_code") var DialCode: String = "",
    @SerialName("email_id") var Email: String = "",
    @SerialName("is_deleted") var IsDeleted: Int = 0,
    @SerialName("is_primary") var IsPrimary: Int = 0,
    @SerialName("created_at") var CreatedAt: @Contextual Instant = Clock.System.now(),
    @SerialName("updated_at") val UpdatedAt: @Contextual Instant = Clock.System.now(),
    @SerialName("__v") var _v: Int = 0
)
```

### Employee Model
```kotlin
@Serializable
data class Employee(
    @SerialName("_id") var _id: String = UniqueueID(),
    @SerialName("user_id") var UserID: String = "",
    @SerialName("business_id") var BusinessID: String = "",
    @SerialName("name") var Name: String = "",
    @SerialName("email") var Email: String = "",
    @SerialName("mobile_number") var MobileNumber: String = "",
    @SerialName("dial_code") var DialCode: String = "",
    @SerialName("role") var Role: String = "",
    @SerialName("is_deleted") var IsDeleted: Int = 0,
    @SerialName("created_at") var CreatedAt: @Contextual Instant = Clock.System.now(),
    @SerialName("updated_at") val UpdatedAt: @Contextual Instant = Clock.System.now(),
    @SerialName("__v") var _v: Int = 0
)
```

### Business Type Model
```kotlin
@Serializable
data class BusinessType(
    @SerialName("_id") var _id: String = UniqueueID(),
    @SerialName("name") var name: String = "",
    @SerialName("description") var description: String = "",
    @SerialName("image") var image: ArrayList<String> = arrayListOf(),
    @SerialName("business_type_id") var business_type_id: Int = 0,
    @SerialName("created_at") val created_at: @Contextual Instant = Clock.System.now(),
    @SerialName("updated_at") val updated_at: @Contextual Instant = Clock.System.now(),
    @SerialName("__v") var _v: Int = 0
)
```

### Business Connection Model
```kotlin
@Serializable
data class BusinessConnection(
    @SerialName("_id") var _id: String = UniqueueID(),
    @SerialName("from_user_id") var fromUserID: String = "",
    @SerialName("from_business_id") var fromBusinessID: String = "",
    @SerialName("to_user_id") var toUserID: String = "",
    @SerialName("to_business_id") var toBusinessID: String = "",
    @SerialName("name") var name: String = "",
    @SerialName("status") var status: String = "",
    @SerialName("created_at") var CreatedAt: @Contextual Instant = Clock.System.now(),
    @SerialName("updated_at") val UpdatedAt: @Contextual Instant = Clock.System.now(),
    @SerialName("__v") var _v: Int = 0
)
```

### Business Config Model
```kotlin
@Serializable
data class BusinessConfig(
    @SerialName("_id") var _id: String = UniqueueID(),
    @SerialName("user_id") var UserID: String = "",
    @SerialName("business_id") var BusinessID: String = "",
    @SerialName("name") var name: String = "",
    @SerialName("dial_code") var dialCode: String = "",
    @SerialName("mobile_number") var mobile: String = "",
    @SerialName("email_id") var email: String = "",
    @SerialName("bar_code") var Barcode: String = "",
    @SerialName("device_id") var deviceID: String = "",
    @SerialName("currency") var currency: Currency? = null,
    @SerialName("created_at") var CreatedAt: @Contextual Instant = Clock.System.now(),
    @SerialName("updated_at") val UpdatedAt: @Contextual Instant = Clock.System.now(),
    @SerialName("__v") var _v: Int = 0
)
```

## Error Handling

All API calls include error handling:

```kotlin
// Example error handling
BusinessNetwork.shared().retrieve(request) { response ->
    response?.let { 
        // Success case
        when (it.status) {
            "success" -> {
                // Handle success
                val businesses = it.payload
            }
            "error" -> {
                // Handle API error
                val errorMessage = it.message
            }
        }
    } ?: run {
        // Handle network/parsing error
        // Response is null
    }
}
```

## Usage Examples

### Creating a Business
```kotlin
val address = Address(
    House = "123 Main St",
    Area = "Downtown",
    City = "New York",
    State = "NY",
    Country = "USA"
)

val request = CreateBusinessRequest(
    businessTypeID = "type123",
    UserID = "user456",
    UserName = "John Doe",
    MobileNumber = "9876543210",
    dialCode = "+1",
    countryCode = "US",
    email = "john@example.com",
    name = "My Business",
    Address = address,
    noteOne = "Additional notes",
    noteTwo = "More notes"
)

BusinessNetwork.shared().create(request) { response ->
    response?.let { 
        if (it.status == "success") {
            val newBusiness = it.payload
            // Handle successful business creation
        }
    }
}
```

### Managing Employees
```kotlin
// Create employee
val createRequest = CreateEmployeeRequest(
    UserID = "user123",
    BusinessID = "business456",
    name = "Jane Smith",
    email = "jane@example.com",
    mobile = "9876543210",
    dialCode = "+1",
    role = "Manager"
)

EmployeeNetwork.shared().createEmployee(createRequest) { response ->
    response?.let { 
        if (it.status == "success") {
            val newEmployee = it.payload
            // Handle successful employee creation
        }
    }
}

// Retrieve employees
val retrieveRequest = FetchBusinessEmployeeRequest(
    BusinessID = "business456",
    UserID = "user123"
)

EmployeeNetwork.shared().retrieve(retrieveRequest) { response ->
    response?.let { 
        if (it.status == "success") {
            val employees = it.payload
            // Handle employee list
        }
    }
}
```

## Network Classes

### BusinessNetwork
- **Singleton Pattern**: Use `BusinessNetwork.shared()`
- **Methods**: retrieve, retrieveSingleBusiness, create, update, findBusiness, findNearby
- **Base Class**: Extends `BaseNetwork`

### EmployeeNetwork
- **Singleton Pattern**: Use `EmployeeNetwork.shared()`
- **Methods**: findUser, createEmployee, updateEmployee, retrieve
- **Base Class**: Extends `AppNetwork`

### BusinessTypeNetwork
- **Methods**: retrieve
- **Features**: JSON decoding with error handling

### BusinessConnectionNetwork
- **Methods**: retrieve, create
- **Features**: JSON serialization with custom configuration

### BusinessConfigNetwork
- **Singleton Pattern**: Use `BusinessConfigNetwork.shared()`
- **Methods**: createBusinessConfig, updateBusinessConfig, retrieveBusinessConfig, retrieveAllCurrency, retrieveAllProduct

## Best Practices

1. **Always handle null responses** - Network calls can fail
2. **Check response status** - API may return error status
3. **Use suspend functions** - For better coroutine integration
4. **Handle exceptions** - Network and parsing errors
5. **Validate input data** - Before making API calls
6. **Use proper error messages** - For better user experience

## Rate Limiting

- Standard REST API rate limiting applies
- Implement exponential backoff for retries
- Handle 429 (Too Many Requests) responses

## Authentication

- User ID and Business ID required for most endpoints
- Session management handled at application level
- Token-based authentication for secure endpoints 

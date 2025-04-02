# Documentation: `supplierService.js`

## Overview
This file contains utility functions for interacting with a supplier-related API. It provides methods to fetch all suppliers and create a new supplier. The API is hosted on a server defined by the `serverUrl` constant.

---

## Functions

### `getAllSuppliers()`
Fetches all suppliers from the API.

#### **Usage**
```javascript
const suppliers = await getAllSuppliers();
```

#### **Details**
- **Endpoint**: `GET /api/suppliers`
- **Returns**: A JSON array of supplier objects.
- **Error Handling**: Throws an error if the response status is not OK (`response.ok` is `false`).

---

### `createSupplier(supplier)`
Creates a new supplier by sending a POST request to the API.

#### **Usage**
```javascript
const newSupplier = {
    name: 'Supplier Name',
    cnpj: '12.345.678/0001-99',
    address: '123 Supplier St.'
};

const createdSupplier = await createSupplier(newSupplier);
```

#### **Details**
- **Input**: 
  - `supplier` (Object): Contains supplier details such as `name`, `cnpj`, and other relevant fields.
  - The `cnpj` field is sanitized to remove all non-numeric characters before sending the request.
- **Endpoint**: `POST /api/suppliers`
- **Headers**: 
  - `Content-Type: application/json`
- **Body**: JSON representation of the sanitized supplier object.
- **Returns**: The created supplier object as JSON.
- **Error Handling**: Throws an error if the response status is not OK (`response.ok` is `false`).

---

## Constants

### `serverUrl`
- **Value**: `'http://localhost:8081'`
- **Purpose**: Base URL for the API endpoints.

---

## Insights

1. **Error Handling**: Both functions implement basic error handling by checking the `response.ok` property. This ensures that failed API calls are caught and reported.

2. **Data Sanitization**: The `createSupplier` function sanitizes the `cnpj` field by removing all non-numeric characters using a regular expression (`/\D/g`). This ensures that the data sent to the API is clean and consistent.

3. **Asynchronous Operations**: Both functions use `async/await` for handling asynchronous API calls, making the code more readable and easier to maintain.

4. **API Design**: The file assumes a RESTful API design with endpoints for fetching (`GET`) and creating (`POST`) suppliers.

5. **Scalability**: The `serverUrl` constant allows for easy modification of the base URL, making the code adaptable to different environments (e.g., development, staging, production).

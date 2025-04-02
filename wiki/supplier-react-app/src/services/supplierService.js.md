# Documentation: Supplier Service

## Overview
This file, `supplierService.js`, provides functionality to interact with a supplier management API. It includes methods for retrieving all suppliers and creating new supplier records. The service communicates with a backend server hosted at `http://localhost:8081`.

---

## Functions

### 1. `getAllSuppliers()`
#### Description:
Fetches a list of all suppliers from the API.

#### Key Details:
- **Endpoint:** `/api/suppliers`
- **HTTP Method:** `GET`
- **Error Handling:** Throws an error if the response is not successful (`response.ok` is `false`).
- **Return Value:** Returns the list of suppliers in JSON format.

#### Use Case:
This function is used to retrieve the complete list of suppliers for display or further processing.

---

### 2. `createSupplier(supplier)`
#### Description:
Creates a new supplier record in the system.

#### Key Details:
- **Input:** 
  - `supplier` (Object): Contains supplier details, including `cnpj` (a Brazilian company identifier).
- **Data Transformation:**
  - The `cnpj` field is sanitized by removing all non-numeric characters before sending the data to the API.
- **Endpoint:** `/api/suppliers`
- **HTTP Method:** `POST`
- **Headers:** 
  - `Content-Type: application/json`
- **Body:** JSON representation of the sanitized supplier object.
- **Error Handling:** Throws an error if the response is not successful (`response.ok` is `false`).
- **Return Value:** Returns the created supplier record in JSON format.

#### Use Case:
This function is used to add a new supplier to the system, ensuring the `cnpj` field is properly formatted before submission.

---

## Insights

### API Interaction
- The service interacts with a backend API hosted at `http://localhost:8081`. This URL is hardcoded, which may limit flexibility in deployment environments. Consider externalizing the server URL into a configuration file or environment variable for better scalability.

### Error Handling
- Both functions include basic error handling by checking the `response.ok` property. However, the error messages are generic (`Failed to fetch suppliers`, `Failed to create supplier`). Enhancing error messages with more specific details (e.g., HTTP status codes or response body) could improve debugging and user feedback.

### Data Validation
- The `createSupplier` function ensures the `cnpj` field is sanitized by removing non-numeric characters. This is a critical step for maintaining data integrity when interacting with the backend.

### Potential Enhancements
- **Pagination:** The `getAllSuppliers` function does not account for pagination. If the supplier list grows large, consider implementing pagination support to optimize performance.
- **Error Logging:** Implementing a logging mechanism for errors could help track issues more effectively.
- **Input Validation:** Additional validation for supplier fields (e.g., name, address) could be added to ensure data quality before submission.

---

## Metadata

| **File Name** | `supplierService.js` |
|---------------|-----------------------|

# SupplierForm.js Documentation

## Overview

The `SupplierForm` component is a React-based form designed to create supplier records. It includes input fields for supplier details, validation logic for the Brazilian CNPJ (Cadastro Nacional da Pessoa Jurídica), and error handling for invalid or incomplete submissions. The form integrates with a service function (`createSupplier`) to persist supplier data.

---

## Features

### Functionalities
- **Form Fields**:
  - `nome`: Supplier's name.
  - `cnpj`: Supplier's CNPJ (validated for correctness).
  - `nomeContato`: Contact person's name.
  - `emailContato`: Contact person's email.
  - `telefoneContato`: Contact person's phone number.

- **Validation**:
  - Ensures all fields are filled.
  - Validates the CNPJ format and checksum.

- **Error Handling**:
  - Displays error messages for invalid CNPJ or missing fields.
  - Handles service errors during supplier creation.

- **Integration**:
  - Uses `createSupplier` service to save supplier data.

---

## Code Structure

### Data Structures
The component uses two state objects:
1. **`supplier`**: Stores form input values.
   ```javascript
   const [supplier, setSupplier] = useState({
       nome: '',
       cnpj: '',
       nomeContato: '',
       emailContato: '',
       telefoneContato: ''
   });
   ```
2. **`error`**: Stores error messages for form validation or service failures.
   ```javascript
   const [error, setError] = useState('');
   ```

### Logic
#### 1. **Event Handlers**
- **`handleChange`**:
  Updates the `supplier` state when an input field changes.
  ```javascript
  const handleChange = (e) => {
      const { name, value } = e.target;
      setSupplier({ ...supplier, [name]: value });
  };
  ```

- **`handleSubmit`**:
  Validates form inputs and submits the data to the `createSupplier` service.
  ```javascript
  const handleSubmit = async (e) => {
      e.preventDefault();
      setError('');

      if (!supplier.nome || !supplier.cnpj || !supplier.nomeContato || !supplier.emailContato || !supplier.telefoneContato) {
          setError('All fields are required');
          return;
      }

      if (!validateCNPJ(supplier.cnpj)) {
          setError('Invalid CNPJ');
          return;
      }

      try {
          await createSupplier(supplier);
          setSupplier({
              nome: '',
              cnpj: '',
              nomeContato: '',
              emailContato: '',
              telefoneContato: ''
          });
      } catch (err) {
          setError('Failed to create supplier');
      }
  };
  ```

#### 2. **Validation**
- **`validateCNPJ`**:
  Validates the CNPJ format and checksum using custom logic.
  ```javascript
  const validateCNPJ = (cnpj) => {
      const cleanedCNPJ = cnpj.replace(/\D/g, '');
      if (cleanedCNPJ.length !== 14) return false;

      let length = cleanedCNPJ.length - 2;
      let numbers = cleanedCNPJ.substring(0, length);
      let digits = cleanedCNPJ.substring(length);
      let sum = 0;
      let pos = length - 7;

      for (let i = length; i >= 1; i--) {
          sum += numbers.charAt(length - i) * pos--;
          if (pos < 2) pos = 9;
      }

      let result = sum % 11 < 2 ? 0 : 11 - sum % 11;
      if (result != digits.charAt(0)) return false;

      length = length + 1;
      numbers = cleanedCNPJ.substring(0, length);
      sum = 0;
      pos = length - 7;

      for (let i = length; i >= 1; i--) {
          sum += numbers.charAt(length - i) * pos--;
          if (pos < 2) pos = 9;
      }

      result = sum % 11 < 2 ? 0 : 11 - sum % 11;
      if (result != digits.charAt(1)) return false;

      return true;
  };
  ```

---

## Insights

### Validation Insights
- The CNPJ validation logic ensures compliance with Brazilian standards, including checksum verification. This is critical for applications dealing with Brazilian businesses.

### Error Handling
- The component provides user-friendly error messages for both validation and service errors, improving the user experience.

### Input Masking
- The `InputMask` library is used to format the CNPJ input field, ensuring users enter data in the correct format (`99.999.999/9999-99`).

### State Management
- The use of React's `useState` hook simplifies state management for form inputs and error messages.

### Service Integration
- The `createSupplier` function is abstracted, allowing for easy integration with backend APIs or other data persistence mechanisms.

---

## Dependencies

| Dependency       | Purpose                                      |
|------------------|----------------------------------------------|
| `react`          | Core library for building the component.    |
| `react-input-mask` | Provides input masking for the CNPJ field. |

---

## Usage

1. Import the `SupplierForm` component:
   ```javascript
   import SupplierForm from './SupplierForm';
   ```

2. Render the component in your application:
   ```javascript
   <SupplierForm />
   ```

3. Ensure the `createSupplier` service is properly implemented and connected to your backend.

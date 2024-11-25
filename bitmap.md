# Q: Custom Bitmap Implementation with Field Numbers

Design a program to handle a simple bitmap representation for a custom protocol with 8 fields. Each bit in the bitmap (1 for presence, 0 for absence) represents whether the corresponding field is included in the data.

## The final output string structure:

- **First 8 characters**: Bitmap indicating the presence of fields.
- **For each field (if present)**:
  - **First 2 characters**: Field size (length of the actual field value).
  - **Remaining characters**: Field value.

## Fields

| Field Number | Field Name     | Data Type |
|--------------|----------------|-----------|
| 1            | Transaction ID | String    |
| 2            | Amount         | String    |
| 3            | Currency       | String    |
| 4            | Merchant ID    | String    |
| 5            | Timestamp      | String    |
| 6            | Card Number    | String    |
| 7            | Expiry Date    | String    |
| 8            | CVV            | String    |

## Input Format

Write a function that:

- Accepts a JSON object where the keys are field numbers (1 to 8), and the values are the field data (or absent if the field is not present).
- Returns a formatted string as described above.

## Examples

### Example 1:

**Input JSON:**

```json
{
    "1": "TX12345",
    "2": "100.00",
    "3": "USD",
    "4": "MERCHANT"
}
```

**Output ACSII:**

```text
1111000007TX1234506100.0003USD08MERCHANT
```

#### Explanation:

- Bitmap: 11110000 (Fields 1, 2, 3, and 4 are present).
- Field Encodings:
    - Field 1: 07TX12345 (07 for size, TX12345 for value).
    - Field 2: 06100.00 (06 for size, 100.00 for value).
    - Field 3: 03USD (03 for size, USD for value).
    - Field 4: 08MERCHANT (08 for size, MERCHANT for value).

### Example 2:


**Input JSON:**

```json
{
    "5": "20231122",
    "6": "1234567890123456",
    "8": "123"
}
```

**Output ACSII:**

```text
00001011082023112216123456789012345603123
```

#### Explanation:

- Bitmap: 00001011 (Fields 5, 6, and 8 are present).
- Field Encodings:
    - Field 5: 0820231122 (08 for size, 20231122 for value).
    - Field 6: 161234567890123456 (16 for size, 1234567890123456 for value).
    - Field 8: 03123 (03 for size, 123 for value).    



### Example 3:

**Input JSON:**

```json
{
    "1": "TRX789",
    "3": "EUR"
}
```

**Output ACSII:**

```text
1010000006TRX78903EUR
```

#### Explanation:

- Bitmap: 10100000 (Fields 1 and 3 are present).
- Field Encodings:
    - Field 1: 06TRX789 (06 for size, TRX789 for value).
    - Field 3: 03EUR (03 for size, EUR for value).

## Expectations
Implement the function to convert the JSON input (using field numbers) to the specified string format.

### Ensure:

- Fields are prefixed with their size (2 characters).
- The bitmap correctly represents the presence/absence of fields.

- Missing fields are omitted from the final output.


## Extended Requirement: Create a REST API Service

Design and implement a REST API service for the bitmap application. The service should:

- Accept a JSON payload in the request body containing the field data (similar to the input format specified earlier).
- Process the input using the bitmap generation logic.
- Return a JSON response containing the generated ASCII string representation in the following format:

```json
{
  "text": "xxx"
}
```

Where `xxx` is the formatted output string generated from the input data.
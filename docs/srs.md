# MarketStock: Software Requirements Specification

## 1. Introduction
MarketStock is a phone-friendly web app for Mami Ngum's shop. It records
products, sales, stock and credit, and reports profit.

## 2. Overall description
Stakeholders: Mami Ngum (owner), Divine (assistant), customers,
suppliers in Douala, the development group.
Users: owner and assistant, basic Android phone, busy all day.

## 3. Functional requirements
| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01 | The system shall let the owner add a product with name, unit, buying price, selling price, quantity and low-stock level. | Must |
| FR-02 | The system shall let the owner edit a product's prices and low-stock level. | Must |
| FR-03 | The system shall let the owner delete a product. | Should |
| FR-04 | The system shall let a user restock a product by entering the quantity received. | Must |
| FR-05 | The system shall let a user record a sale of one or more products. | Must |
| FR-06 | The system shall reduce stock for each product when a sale is saved. | Must |
| FR-07 | The system shall reject a sale whose quantity is greater than the quantity in stock, and show the quantity available. | Must |
| FR-08 | The system shall log every stock change with the user, date and time. | Must |
| FR-09 | The system shall alert the owner when a product reaches its low-stock level. | Should |
| FR-10 | The system shall let a user record a sale as credit with the customer's name and phone. | Should |
| FR-11 | The system shall let a user record a repayment against a customer's balance. | Should |
| FR-12 | The system shall list every customer with an unpaid credit balance, largest first. | Should |
| FR-13 | The system shall show the owner a daily report of sales total and profit. | Should |
| FR-14 | The system shall require login with a username and password. | Must |

## 4. Non-functional requirements
| ID | Requirement | Type |
|----|-------------|------|
| NFR-01 | Recording a sale of up to 3 products shall take no more than 30 seconds for a trained user. | Usability |
| NFR-02 | Every page shall load in under 3 seconds on a 3G connection. | Performance |
| NFR-03 | All amounts shall be shown in XAF with no decimals. | Constraint |
| NFR-04 | Only the owner shall be able to change prices or delete records. | Security |
| NFR-05 | The interface shall work in a phone browser 360 px wide. | Compatibility |

## 5. Use case diagram and descriptions
See the diagram: docs/diagrams/usecases.drawio.svg

### UC-03 Record a sale
| Field | Description |
|-------|-------------|
| Actors | Assistant, Owner |
| Preconditions | User is logged in; at least one product exists |
| Main flow | 1. User opens "New sale". 2. User picks a product and enters the quantity. 3. System shows the line total. 4. User repeats 2 to 3 for more products. 5. User picks the payment method. 6. User confirms. 7. System saves the sale, reduces stock for each product and shows the receipt. |
| Alternative flow 2a | Quantity is more than stock: system shows "Only N in stock" and stays on step 2. |
| Alternative flow 5a | Payment is credit: system asks for the customer's name and phone, then adds the total to their balance. |
| Postconditions | Sale recorded with date, time and user; stock reduced; credit balance updated if credit. |
| Requirements covered | FR-05, FR-06, FR-07, FR-10 |

### UC-02 Restock a product
| Field | Description |
|-------|-------------|
| Actors | Assistant, Owner |
| Preconditions | User is logged in; the product exists |
| Main flow | 1. User opens "Restock". 2. User picks a product. 3. User enters the quantity received. 4. User confirms. 5. System adds the quantity to stock and logs the change with user, date and time. |
| Alternative flow 3a | Quantity is zero or negative: system shows "Enter a quantity above 0" and stays on step 3. |
| Postconditions | Stock increased; stock log entry created. |
| Requirements covered | FR-04, FR-08 |

### UC-06 Record a repayment
| Field | Description |
|-------|-------------|
| Actors | Assistant, Owner |
| Preconditions | User is logged in; the customer has an unpaid balance |
| Main flow | 1. User opens "Credit book". 2. User picks a customer. 3. User enters the amount paid. 4. User confirms. 5. System reduces the customer's balance and saves the repayment with date, time and user. |
| Alternative flow 3a | Amount is more than the balance: system shows "Balance is only N XAF" and stays on step 3. |
| Postconditions | Balance reduced; repayment recorded. |
| Requirements covered | FR-11, FR-12 |

### UC-03 Record a sale
| Field | Description |
|-------|-------------|
| Actors | Assistant, Owner |
| Preconditions | User is logged in; at least one product exists |
| Main flow | 1. User opens "New sale". 2. User picks a product and enters the quantity. 3. System shows the line total. 4. User repeats 2 to 3 for more products. 5. User picks the payment method. 6. User confirms. 7. System saves the sale, reduces stock for each product and shows the receipt. |
| Alternative flow 2a | Quantity is more than stock: system shows "Only N in stock" and stays on step 2. |
| Alternative flow 5a | Payment is credit: system asks for the customer's name and phone, then adds the total to their balance. |
| Postconditions | Sale recorded with date, time and user; stock reduced; credit balance updated if credit. |
| Requirements covered | FR-05, FR-06, FR-07, FR-10 |

### UC-02 Restock a product
| Field | Description |
|-------|-------------|
| Actors | Assistant, Owner |
| Preconditions | User is logged in; the product exists |
| Main flow | 1. User opens "Restock". 2. User picks a product. 3. User enters the quantity received. 4. User confirms. 5. System adds the quantity to stock and logs the change with user, date and time. |
| Alternative flow 3a | Quantity is zero or negative: system shows "Enter a quantity above 0" and stays on step 3. |
| Postconditions | Stock increased; stock log entry created. |
| Requirements covered | FR-04, FR-08 |

### UC-06 Record a repayment
| Field | Description |
|-------|-------------|
| Actors | Assistant, Owner |
| Preconditions | User is logged in; the customer has an unpaid balance |
| Main flow | 1. User opens "Credit book". 2. User picks a customer. 3. User enters the amount paid. 4. User confirms. 5. System reduces the customer's balance and saves the repayment with date, time and user. |
| Alternative flow 3a | Amount is more than the balance: system shows "Balance is only N XAF" and stays on step 3. |
| Postconditions | Balance reduced; repayment recorded. |
| Requirements covered | FR-11, FR-12 |

## 6. Priorities (MoSCoW)
Must: products, restock, sales, stock log, login.
Should: credit book, low-stock alert, daily report.
Could: weekly sales charts.
Won't: Mobile Money payments.

## 7. Out of scope
Mobile Money payments, multiple shops, supplier ordering.
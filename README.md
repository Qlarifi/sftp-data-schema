# Data Exchange Schema

## Events
### [TransactionCreated](./events/transactionCreated.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the transaction was issued from the lender's perspective.

**transactionId**: A unique identifier for the transaction.

**accountId**: A unique identifier for the account this transaction is associated with.

**consumerId**: A unique identifier for the consumer who owes this transaction (as defined by the lender).

**amount.number**: The monetary amount of this transaction.

**amount.currency**: The currency of the amount.

**transactionType**: The type of transaction product, Possible values are: `PayIn3`, `PayIn4`, `PayIn30Days`.

### [InstallmentIssued](./events/installmentIssued.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the instalment was issued from the lender's perspective.

**installmentId**: A unique identifier for this specific instalment.

**transactionId**: A unique identifier for the transaction this instalment belongs to.

**accountId**: A unique identifier for the account this instalment is associated with.

**consumerId**: A unique identifier for the consumer who owes this instalment (as defined by the lender).

**amount.number**: The monetary amount due for this instalment.

**amount.currency**: The currency of the amount.

**dueTimestamp**: The date and time (UTC) when this instalment is due.

**index**: The sequential order of this instalment within the transaction.

### [InstallmentPaid](./events/installmentPaid.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the payment was made from the lender's perspective.

**installmentId**: The unique identifier of the instalment that received the payment.

**transactionId**: The unique identifier of the transaction the instalment belongs to.

**accountId**: A unique identifier for the account this instalment is associated with.

**consumerId** [optional]: The unique identifier of the consumer who made the payment.

**amount.number**: The amount paid towards the instalment.

**amount.currency**: The currency of the payment.

### [InstallmentPostponed](./events/installmentPostponed.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the installment was postponed from the lender's perspective.

**installmentId**: The unique identifier of the instalment postponed.

**transactionId**: The unique identifier of the transaction the instalment belongs to.

**accountId**: A unique identifier for the account this instalment is associated with.

**consumerId**: The unique identifier of the consumer the instalment belongs to.

**newDueTimestamp**: The new date and time (UTC) when this instalment is due.

**postponedType**: The type of postponed action. Possible values are `ConsumerRequest`, `LenderCommercial`, `LenderTechnical`.

### [FeeIssued](./events/feeIssued.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the penalty was assessed from the lender's perspective.

**installmentId**: The unique identifier of the instalment with the assessed penalty.

**transactionId**: The unique identifier of the transaction the instalment belongs to.

**accountId**: A unique identifier for the account this instalment is associated with.

**consumerId**: The unique identifier of the consumer who incurred the penalty.

**penaltyAmount.number**: The amount of the penalty assessed.

**penaltyAmount.currency**: The currency of the penalty.

**penaltyType**: The type of penalty fee that was added to the transaction amount. Possible values are: `LateFee`, `PostponedFee`, `Interest`, `Other`.

### [FeePaid](./events/feePaid.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the fee was paid from the lender's perspective.

**installmentId** [optional]: The unique identifier of the instalment associated with the fee.

**transactionId**: The unique identifier of the transaction associated with the fee.

**accountId**: A unique identifier for the account this fee is associated with.

**consumerId**: The unique identifier of the consumer who incurred the fee.

**amount.number**: The amount paid.

**amount.currency**: The currency of the payment.

### [RefundIssued](./events/refundIssued.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the refund was issued from the lender's perspective.

**installmentId**: The unique identifier of the instalment that was refunded.

**transactionId**: The unique identifier of the transaction the instalment belongs to.

**accountId**: A unique identifier for the account this instalment is associated with.

**consumerId**: The unique identifier of the consumer who received the refund.

**refundAmount.number**: The amount of the refund issued.

**refundAmount.currency**: The currency of the refund.

### [InstallmentRebalanced](./events/installmentRebalanced.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the rebalance occurred from the lender's perspective.

**installmentId**: The unique identifier of the instalment that was rebalanced.

**transactionId**: The unique identifier of the transaction the instalment belongs to.

**accountId**: A unique identifier for the account this instalment is associated with.

**consumerId** [optional]: The unique identifier of the consumer affected by the rebalance.

**newAmountDue.number**: The new amount due on the instalment after the rebalance.

**newAmountDue.currency**: The currency of the new amount due.

### [InstallmentWrittenOff](./events/installmentWrittenOff.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the write-off occurred from the lender's perspective.

**installmentId**: The unique identifier of the instalment that was written off.

**transactionId**: The unique identifier of the transaction the instalment belongs to.

**accountId**: A unique identifier for the account this instalment is associated with.

**consumerId**: The unique identifier of the consumer whose instalment was written off.

**reason**: The reason for the write-off. Possible values are: `ConsumerDefault` (sent to collections), `LenderCommercial`, `LenderTechnical`.

### [InstallmentCanceled](./events/installmentCanceled.json)

**type**: Indicates the event type.

**effectiveTimestamp**: The timestamp (UTC) when the cancellation occurred from the lender's perspective.

**installmentId**: The unique identifier of the cancelled instalment.

**transactionId**: The unique identifier of the transaction the instalment belonged to.

**accountId**: A unique identifier for the account this instalment is associated with.

**consumerId**: The unique identifier of the consumer affected by the cancellation.

## Scenarios
We have put together a few scenarios to align on the data exchange schema. These scenarios are based on the payment plan types and the possible transactions that can occur within them. The scenarios are not exhaustive but should cover the most common cases.

### Scenario 1
Transaction with 4 installments, all paid on time.

| Effective Timestamp | Type               | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index |
| ------------------- | ------------------ | -------- | -------------- | ---------- | ---------------- | ------------------- | ----- |
| 2025-02-03 00:00:00 | transactionCreated |          |                | 200.00 USD | PayIn4           | \-                  | \-    |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-1  | 50.00 USD  | \-               | 2025-02-03 00:00:00 | 0     |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-2  | 50.00 USD  | \-               | 2025-02-17 00:00:00 | 1     |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-3  | 50.00 USD  | \-               | 2025-03-03 00:00:00 | 2     |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-4  | 50.00 USD  | \-               | 2025-03-17 00:00:00 | 3     |
| 2025-02-03 00:00:00 | installmentPaid    | order-1  | installment-1  | 50.00 USD  | \-               | \-                  | \-    |
| 2025-02-17 00:00:00 | installmentPaid    | order-1  | installment-2  | 50.00 USD  | \-               | \-                  | \-    |
| 2025-03-03 00:00:00 | installmentPaid    | order-1  | installment-3  | 50.00 USD  | \-               | \-                  | \-    |
| 2025-03-17 00:00:00 | installmentPaid    | order-1  | installment-4  | 50.00 USD  | \-               | \-                  | \-    |

### Scenario 2
Transaction with 4 installments, postponed (with fee) just before 3rd installment, all paid on time.

| Effective Timestamp | Type                 | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index | Additional Details |                                                   |
| ------------------- | -------------------- | -------- | -------------- | ---------- | ---------------- | ------------------- | ----- | ------------------ | ------------------------------------------------- |
| 2025-02-03 00:00:00 | transactionCreated   |          |                | 200.00 USD | PayIn4           | \-                  | \-    | \-                 |                                                   |
| 2025-02-03 00:00:00 | installmentIssued    | order-1  | installment-1  | 50.00 USD  | \-               | 2025-02-03 00:00:00 | 0     | \-                 |                                                   |
| 2025-02-03 00:00:00 | installmentIssued    | order-1  | installment-2  | 50.00 USD  | \-               | 2025-02-17 00:00:00 | 1     | \-                 |                                                   |
| 2025-02-03 00:00:00 | installmentIssued    | order-1  | installment-3  | 50.00 USD  | \-               | 2025-03-03 00:00:00 | 2     | \-                 |                                                   |
| 2025-02-03 00:00:00 | installmentIssued    | order-1  | installment-4  | 50.00 USD  | \-               | 2025-03-17 00:00:00 | 3     | \-                 |                                                   |
| 2025-02-03 00:00:00 | installmentPaid      | order-1  | installment-1  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                   |
| 2025-02-17 00:00:00 | installmentPaid      | order-1  | installment-2  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                   |
| 2025-03-02 00:00:00 | installmentPostponed | order-1  | installment-3  | \-         | \-               | 2025-03-10 00:00:00 | \-    | Consumer-initiated | Consumer postponed the 3rd installment by 7 days. |
| 2025-03-02 00:00:00 | installmentPostponed | order-1  | installment-4  | \-         | \-               | 2025-03-24 00:00:00 | \-    | Consumer-initiated | 4th installment due date is also pushed 7 days.   |
| 2025-03-02 00:00:00 | feeIssued            | order-1  | installment-2  | 2.00 USD   | \-               | \-                  | \-    | Postponed Fee      | Fee associated with a single postpone request     |
| 2025-03-10 00:00:00 | installmentPaid      | order-1  | installment-3  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                   |
| 2025-03-10 00:00:00 | feePaid              | order-1  | installment-2  | 2.00 USD   | \-               | \-                  | \-    | \-                 | Postponed fee being paid.                         |
| 2025-03-24 00:00:00 | installmentPaid      | order-1  | installment-4  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                   |

### Scenario 3
Transaction with 4 installments, 2nd installment late (with fee), others paid on time.

| Effective Timestamp | Type               | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index | Additional Details |                                                                 |
| ------------------- | ------------------ | -------- | -------------- | ---------- | ---------------- | ------------------- | ----- | ------------------ | --------------------------------------------------------------- |
| 2025-02-03 00:00:00 | transactionCreated |          |                | 200.00 USD | PayIn4           | \-                  | \-    | \-                 |                                                                 |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-1  | 50.00 USD  | \-               | 2025-02-03 00:00:00 | 0     | \-                 |                                                                 |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-2  | 50.00 USD  | \-               | 2025-02-17 00:00:00 | 1     | \-                 |                                                                 |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-3  | 50.00 USD  | \-               | 2025-03-03 00:00:00 | 2     | \-                 |                                                                 |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-4  | 50.00 USD  | \-               | 2025-03-17 00:00:00 | 3     | \-                 |                                                                 |
| 2025-02-03 00:00:00 | installmentPaid    | order-1  | installment-1  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                                 |
| 2025-02-20 00:00:00 | feeIssued          | order-1  | installment-2  | 3.00 USD   | \-               | \-                  | \-    | Late Fee           | Late fee is applied after the installment is late for > 3 days. |
| 2025-02-21 00:00:00 | installmentPaid    | order-1  | installment-2  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                                 |
| 2025-02-21 00:00:00 | feePaid            | order-1  | installment-2  | 3.00 USD   | \-               | \-                  | \-    | \-                 | Late fee is being paid.                                         |
| 2025-03-03 00:00:00 | installmentPaid    | order-1  | installment-3  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                                 |
| 2025-03-17 00:00:00 | installmentPaid    | order-1  | installment-4  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                                 |

### Scenario 4
Transaction with 4 installments, with $65 refund, all paid on time.

| Effective Timestamp | Type                  | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index | Additional Details |                                                                                                                                                                               |
| ------------------- | --------------------- | -------- | -------------- | ---------- | ---------------- | ------------------- | ----- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2025-02-03 00:00:00 | transactionCreated    |          |                | 200.00 USD | PayIn4           | \-                  | \-    | \-                 |                                                                                                                                                                               |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-1  | 50.00 USD  | \-               | 2025-02-03 00:00:00 | 0     | \-                 |                                                                                                                                                                               |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-2  | 50.00 USD  | \-               | 2025-02-17 00:00:00 | 1     | \-                 |                                                                                                                                                                               |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-3  | 50.00 USD  | \-               | 2025-03-03 00:00:00 | 2     | \-                 |                                                                                                                                                                               |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-4  | 50.00 USD  | \-               | 2025-03-17 00:00:00 | 3     | \-                 |                                                                                                                                                                               |
| 2025-02-03 00:00:00 | installmentPaid       | order-1  | installment-1  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                                                                                                                                               |
| 2025-02-05 00:00:00 | refundIssued          |          |                | 65.00 USD  |                  | \-                  | \-    |                    | The $65 refund is applied from the last installment backwards.<br>It means the 3rd installment is fully refunded ($50) and<br>the 2nd installment is partialy refunded ($15). |
| 2025-02-05 00:00:00 | installmentRebalanced | order-1  | installment-4  | 0.00 USD   | \-               | \-                  | \-    | Refunded           |                                                                                                                                                                               |
| 2025-02-05 00:00:00 | installmentRebalanced | order-1  | installment-3  | 35.00 USD  | \-               | \-                  | \-    | Refunded           | The amount in the rebalanced event represents the new amount due.                                                                                                             |
| 2025-02-17 00:00:00 | installmentPaid       | order-1  | installment-2  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                                                                                                                                                               |
| 2025-03-03 00:00:00 | installmentPaid       | order-1  | installment-3  | 35.00 USD  | \-               | \-                  | \-    | \-                 |                                                                                                                                                                               |

### Scenario 5
Transaction with 1 installment (PayIn30Days), paid on time.

| Effective Timestamp | Type               | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index |                                                                   |
| ------------------- | ------------------ | -------- | -------------- |------------| ---------------- | ------------------- | ----- |-------------------------------------------------------------------|
| 2025-02-03 00:00:00 | transactionCreated |          |                | 150.00 USD | PayIn30Days      |                     |       | Here this is a payment plan that only contains 1 installment.     |
| 2025-02-03 00:00:00 | installmentIssued  | order-1  | installment-1  | 150.00 USD |                  | 2025-03-05 00:00:00 | 0     | The installment is issued on Feb 3rd but it is pay 30 days later. |
| 2025-03-05 00:00:00 | installmentPaid    | order-1  | installment-1  | 150.00 USD |                  |                     |       |                                                                   |

### Scenario 6
Transaction that results in 2 orders of 4 installments, with pay on ship.

| Effective Timestamp | Type               | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index |                                                             |
| ------------------- | ------------------ | -------- | -------------- | ---------- | ---------------- | ------------------- | ----- | ----------------------------------------------------------- |
| 2025-02-03 00:00:00 | transactionCreated |          |                | 200.00 USD | PayIn4           | \-                  | \-    |                                                             |
| 2025-02-04 00:00:00 | installmentIssued  | order-1  | installment-1  | 33.33 USD  | \-               | 2025-02-04 00:00:00 | 0     | First shipment is made 1 day after the decision.            |
| 2025-02-04 00:00:00 | installmentIssued  | order-1  | installment-2  | 33.33 USD  | \-               | 2025-02-18 00:00:00 | 1     | 4 installments for order 1 are created.                     |
| 2025-02-04 00:00:00 | installmentIssued  | order-1  | installment-3  | 33.33 USD  | \-               | 2025-03-04 00:00:00 | 2     |                                                             |
| 2025-02-04 00:00:00 | installmentIssued  | order-1  | installment-4  | 33.34 USD  | \-               | 2025-03-18 00:00:00 | 3     |                                                             |
| 2025-02-04 00:00:00 | installmentPaid    | order-1  | installment-1  | 33.33 USD  | \-               | \-                  | \-    | 1st installment for order 1 is paid on the day of shipping. |
| 2025-02-08 00:00:00 | installmentIssued  | order-2  | installment-1  | 16.66 USD  | \-               | 2025-02-08 00:00:00 | 0     | Second shipment is made 5 days after the decision.          |
| 2025-02-08 00:00:00 | installmentIssued  | order-2  | installment-2  | 16.66 USD  | \-               | 2025-02-22 00:00:00 | 1     | 4 installments for order 2 are created.                     |
| 2025-02-08 00:00:00 | installmentIssued  | order-2  | installment-3  | 16.66 USD  | \-               | 2025-03-08 00:00:00 | 2     |                                                             |
| 2025-02-08 00:00:00 | installmentIssued  | order-2  | installment-4  | 16.67 USD  | \-               | 2025-03-22 00:00:00 | 3     |                                                             |
| 2025-02-08 00:00:00 | installmentPaid    | order-2  | installment-1  | 16.66 USD  | \-               | \-                  | \-    | 1st installment for order 2 is paid on the day of shipping. |
| 2025-02-18 00:00:00 | installmentPaid    | order-1  | installment-2  | 33.33 USD  | \-               | \-                  | \-    |                                                             |
| 2025-02-22 00:00:00 | installmentPaid    | order-2  | installment-2  | 16.66 USD  | \-               | \-                  | \-    |                                                             |
| 2025-03-04 00:00:00 | installmentPaid    | order-1  | installment-3  | 33.34 USD  | \-               | \-                  | \-    |                                                             |
| 2025-03-08 00:00:00 | installmentPaid    | order-2  | installment-3  | 16.67 USD  | \-               | \-                  | \-    |                                                             |
| 2025-03-18 00:00:00 | installmentPaid    | order-1  | installment-4  | 33.34 USD  | \-               | \-                  | \-    |                                                             |
| 2025-03-22 00:00:00 | installmentPaid    | order-2  | installment-4  | 16.67 USD  | \-               | \-                  | \-    |                                                             |

### Scenario 7
Transaction of $200 with 4 installments, with $110 refund before 3rd installment.
Refund is applied from the last installment backwards.

| Effective Timestamp | Type                  | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index | Additional Details |                                      |
| ------------------- | --------------------- | -------- | -------------- | ---------- | ---------------- | ------------------- | ----- | ------------------ | ------------------------------------ |
| 2025-02-03 00:00:00 | transactionCreated    |          |                | 200.00 USD | PayIn4           | \-                  | \-    | \-                 |                                      |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-1  | 50.00 USD  | \-               | 2025-02-03 00:00:00 | 0     | \-                 |                                      |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-2  | 50.00 USD  | \-               | 2025-02-17 00:00:00 | 1     | \-                 |                                      |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-3  | 50.00 USD  | \-               | 2025-03-03 00:00:00 | 2     | \-                 |                                      |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-4  | 50.00 USD  | \-               | 2025-03-17 00:00:00 | 3     | \-                 |                                      |
| 2025-02-03 00:00:00 | installmentPaid       | order-1  | installment-1  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                      |
| 2025-02-17 00:00:00 | installmentPaid       | order-1  | installment-2  | 50.00 USD  | \-               | \-                  | \-    | \-                 |                                      |
| 2025-02-26 00:00:00 | refundIssued          |          |                | 110 USD    | \-               | \-                  | \-    | \-                 |                                      |
| 2025-02-26 00:00:00 | installmentRebalanced | order-1  | installment-4  | 0.00 USD   | \-               | \-                  | \-    | Refunded           |                                      |
| 2025-02-26 00:00:00 | installmentRebalanced | order-1  | installment-3  | 0.00 USD   | \-               | \-                  | \-    | Refunded           |                                      |
| 2025-02-26 00:00:00 | installmentRebalanced | order-1  | installment-2  | 40.00 USD  | \-               | \-                  | \-    | Refunded           | Lender refunds the consumer card $10 |

### Scenario 8
Transaction of $200 with 4 installments, with $110 refund before 3rd installment.
Refund is evenly split across the 3 installments.

| Effective Timestamp | Type                  | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index | Additional Details |
| ------------------- | --------------------- | -------- | -------------- | ---------- | ---------------- | ------------------- | ----- | ------------------ |
| 2025-02-03 00:00:00 | transactionCreated    |          |                | 200.00 USD | PayIn4           | \-                  | \-    | \-                 |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-1  | 50.00 USD  | \-               | 2025-02-03 00:00:00 | 0     | \-                 |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-2  | 50.00 USD  | \-               | 2025-02-17 00:00:00 | 1     | \-                 |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-3  | 50.00 USD  | \-               | 2025-03-03 00:00:00 | 2     | \-                 |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-4  | 50.00 USD  | \-               | 2025-03-17 00:00:00 | 3     | \-                 |
| 2025-02-03 00:00:00 | installmentPaid       | order-1  | installment-1  | 50.00 USD  | \-               | \-                  | \-    | \-                 |
| 2025-02-17 00:00:00 | installmentPaid       | order-1  | installment-2  | 50.00 USD  | \-               | \-                  | \-    | \-                 |
| 2025-02-26 00:00:00 | refundIssued          |          |                | 110.00 USD | \-               | \-                  | \-    | \-                 |
| 2025-02-03 00:00:00 | installmentRebalanced | order-1  | installment-1  | 22.50 USD  | \-               | \-                  | \-    | Refunded           |
| 2025-02-05 00:00:00 | installmentRebalanced | order-1  | installment-2  | 22.50 USD  | \-               | \-                  | \-    | Refunded           |
| 2025-02-05 00:00:00 | installmentRebalanced | order-1  | installment-3  | 22.50 USD  | \-               | \-                  | \-    | Refunded           |
| 2025-02-05 00:00:00 | installmentRebalanced | order-1  | installment-4  | 22.50 USD  | \-               | \-                  | \-    | Refunded           |
| 2025-03-03 00:00:00 | installmentPaid       | order-1  | installment-3  | 22.50 USD  | \-               | \-                  | \-    | \-                 |
| 2025-03-17 00:00:00 | installmentPaid       | order-1  | installment-4  | 22.50 USD  | \-               | \-                  | \-    | \-                 |

### Scenario 9
Transaction of $200 with 4 installments, with 3rd & 4th installment late, then written off (sent to collections).

| Effective Timestamp | Type                  | Order ID | Installment ID | Amount     | Transaction Type | Due Timestamp       | Index | Additional Details |
| ------------------- | --------------------- | -------- | -------------- | ---------- | ---------------- | ------------------- | ----- | ------------------ |
| 2025-02-03 00:00:00 | transactionCreated    |          |                | 150.00 USD | PayIn4           | \-                  | \-    | \-                 |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-1  | 50.00 USD  | \-               | 2025-02-03 00:00:00 | 0     | \-                 |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-2  | 50.00 USD  | \-               | 2025-02-17 00:00:00 | 1     | \-                 |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-3  | 50.00 USD  | \-               | 2025-03-03 00:00:00 | 2     | \-                 |
| 2025-02-03 00:00:00 | installmentIssued     | order-1  | installment-4  | 50.00 USD  | \-               | 2025-03-17 00:00:00 | 3     | \-                 |
| 2025-02-03 00:00:00 | installmentPaid       | order-1  | installment-1  | 50.00 USD  | \-               | \-                  | \-    | \-                 |
| 2025-02-17 00:00:00 | installmentPaid       | order-1  | installment-2  | 50.00 USD  | \-               | \-                  | \-    | \-                 |
| 2025-09-01 00:00:00 | installmentWrittenOff | order-1  | installment-3  | 50.00 USD  | \-               | \-                  | \-    | ConsumerDefault    |
| 2025-09-01 00:00:00 | installmentWrittenOff | order-1  | installment-4  | 50.00 USD  | \-               | \-                  | \-    | ConsumerDefault    |

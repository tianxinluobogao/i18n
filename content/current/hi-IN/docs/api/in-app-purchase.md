# एेप में से खरीददारी

> एेप के अन्दर से ही मैक-स्टोर से खरीददारी

Process: [Main](../glossary.md#main-process)

## इवेंट्स

`InAppPurchase` मौड्यूल से निम्नलिखित events भेजे जाते हैं

### Event: 'transactions-updated'

Emitted when one or more transactions have been updated.

Returns:

* `event` Event
* `transactions` Transaction[] - Array of [`Transaction`](structures/transaction.md) objects.

## मेथड्स

The `inAppPurchase` module has the following methods:

### `inAppPurchase.purchaseProduct(productID[, quantity])`

* `productID` String - The identifiers of the product to purchase. (The identifier of `com.example.app.product1` is `product1`).
* `quantity` Integer (optional) - The number of items the user wants to purchase.

Returns `Promise<Boolean>` - Returns `true` if the product is valid and added to the payment queue.

You should listen for the `transactions-updated` event as soon as possible and certainly before you call `purchaseProduct`.

### `inAppPurchase.getProducts(productIDs)`

* `productIDs` String[] - The identifiers of the products to get.

Returns `Promise<Product[]>` - Resolves with an array of [`Product`](structures/product.md) objects.

Retrieves the product descriptions.

### `inAppPurchase.canMakePayments()`

Returns `Boolean`, whether a user can make a payment.

### `inAppPurchase.getReceiptURL()`

Returns `String`, the path to the receipt.

### `inAppPurchase.finishAllTransactions()`

Completes all pending transactions.

### `inAppPurchase.finishTransactionByDate(date)`

* `date` String - The ISO formatted date of the transaction to finish.

Completes the pending transactions corresponding to the date.

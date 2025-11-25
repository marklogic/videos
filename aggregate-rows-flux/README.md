Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

# Aggregate Rows with MarkLogic Flux

## Example: Aggregate Rows

### macOS/Linux
```
./bin/flux import-jdbc \
    --query "select c.*, p.payment_id, p.amount, p.payment_date from customer c inner join payment p on c.customer_id = p.customer_id" \
    --group-by customer_id \
    --aggregate "payments=payment_id,amount,payment_date" \
    --connection-string "flux-example-user:password@localhost:8004" \
    --permissions flux-example-role,read,flux-example-role,update
```

### Windows PowerShell
```
bin\flux import-jdbc ^
    --query "select c.*, p.payment_id, p.amount, p.payment_date from customer c inner join payment p on c.customer_id = p.customer_id" ^
    --group-by customer_id ^
    --aggregate "payments=payment_id,amount,payment_date" ^
    --connection-string "flux-example-user:password@localhost:8004" ^
    --permissions flux-example-role,read,flux-example-role,update
```

## Example: Aggregate Rows with Multiple Joins

### macOS/Linux
```
./bin/flux import-jdbc \
    --query "SELECT c.*, p.payment_id, p.amount, p.payment_date, r.rental_id, r.rental_date, r.return_date FROM customer c INNER JOIN payment p ON c.customer_id = p.customer_id INNER JOIN rental r ON c.customer_id = r.customer_id" \
    --group-by customer_id \
    --aggregate "payments=payment_id,amount,payment_date" \
    --aggregate "rentals=rental_id,rental_date,return_date" \
    --connection-string "flux-example-user:password@localhost:8004"
```

### Windows PowerShell
```
bin\flux import-jdbc ^
    --query "SELECT c.*, p.payment_id, p.amount, p.payment_date, r.rental_id, r.rental_date, r.return_date FROM customer c INNER JOIN payment p ON c.customer_id = p.customer_id INNER JOIN rental r ON c.customer_id = r.customer_id" ^
    --group-by customer_id ^
    --aggregate "payments=payment_id,amount,payment_date" ^
    --aggregate "rentals=rental_id,rental_date,return_date" ^
    --connection-string "flux-example-user:password@localhost:8004"
```

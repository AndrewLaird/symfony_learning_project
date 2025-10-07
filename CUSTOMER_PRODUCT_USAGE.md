# Customer-Product Relationship Usage

This document explains how to use the Customer and Product entities with their one-to-many relationship.

## Entity Structure

### Customer Entity
- **Fields**: id, name, email
- **Relationship**: One customer can have many products (OneToMany)

### Product Entity
- **Fields**: id, name, price, customer_id (foreign key)
- **Relationship**: Many products belong to one customer (ManyToOne)

## Usage Examples

### Creating a Customer with Products

```php
use App\Entity\Customer;
use App\Entity\Product;

// Create a new customer
$customer = new Customer();
$customer->setName('John Doe');
$customer->setEmail('john@example.com');

// Create products
$product1 = new Product();
$product1->setName('Laptop');
$product1->setPrice(999.99);

$product2 = new Product();
$product2->setName('Mouse');
$product2->setPrice(29.99);

// Add products to customer
$customer->addProduct($product1);
$customer->addProduct($product2);

// Persist to database
$entityManager->persist($customer);
$entityManager->persist($product1);
$entityManager->persist($product2);
$entityManager->flush();
```

### Accessing Customer's Products

```php
// Get all products for a customer
$products = $customer->getProducts();

foreach ($products as $product) {
    echo $product->getName() . ": $" . $product->getPrice() . "\n";
}
```

### Accessing Product's Customer

```php
// Get the customer for a product
$customer = $product->getCustomer();
echo $customer->getName(); // Output: John Doe
```

### Removing a Product from a Customer

```php
$customer->removeProduct($product1);
$entityManager->flush();
```

## Database Schema

```sql
-- Customer table
CREATE TABLE customer (
    id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL
);

-- Product table with foreign key to customer
CREATE TABLE product (
    id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
    customer_id INTEGER DEFAULT NULL,
    name VARCHAR(255) NOT NULL,
    price DOUBLE PRECISION NOT NULL,
    CONSTRAINT FK_D34A04AD9395C3F3 FOREIGN KEY (customer_id) 
        REFERENCES customer (id)
);
```

## Running Migrations

To apply the database schema:

```bash
php bin/console doctrine:migrations:migrate
```

## Verifying the Setup

```bash
# Check entity mappings
php bin/console doctrine:mapping:info

# Validate schema
php bin/console doctrine:schema:validate
```

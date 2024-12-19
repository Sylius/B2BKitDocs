# Sylius B2B Kit
## Installation

- [⬅️ Back](../README.md)

### 1. Requirements

We work on stable, supported and up-to-date versions of packages. We recommend you to do the same.

| Package       | Version |
|---------------|---------|
| PHP           | \>= 8.1 |
| sylius/sylius | 1.13.x  |
| MySQL         | \>= 5.7 |

### 2. Require plugin with composer ###

```bash
$ composer require sylius/b2b-kit --no-scripts
```
### 3. Configure Sylius B2B Kit ###

- [Plugin Configuration](./01.1-configuration.md)

### 4. Add traits to your entities ###
- [App/Entity/Addressing/Address.php](./01.2-address.md)
- [App/Entity/Customer/Customer.php](./01.3-customer.md)
- [App/Entity/Customer/CustomerGroup.php](./01.4-customer-group.md)
- [App/Entity/Order/Order.php](./01.5-order.md)
- [App/Entity/ShopUser/ShopUser.php](./01.6-shop-user.md)

<p align="center">
    <a href="https://sylius.com" target="_blank">
        <img src="https://demo.sylius.com/assets/shop/img/logo.png" />
    </a>
</p>

<h1 align="center">Sylius B2B Kit</h1>

<p align="center">A set of powerful extensions for B2B customers.</p>

Sylius B2B Kit is a dedicated B2B solution. It's a bundle of already configured features that allows you to quickly build B2B experience. It is build on top of Sylius and Symfony, because of that all the features are also very easily customizable and extendable. Plugin has been tested with Behat, PHPSpec and PHPUnit.

Features included in B2B Kit:
- **Organization Management** - *allows you to create account for organization (business customer). Organization account shares the same billing data, address book and order history.*
- **Quick Shopping** - *allows you to add multiple products to cart at once, using product codes.*
- **Elasticsearch** - *allows you to search and filter products by options, attributes, taxons and name on the product listing page*
- **Order Management** - *allows you to create and manage customer orders in the admin panel*
- **Wishlists** - *allows you to integrate wishlist features in your shop*
- **ImportExport** - *allows you to import and export products, orders, customers, etc.*
- **Customer Groups Pricing Lists** - *allows you to create pricing lists for customer groups*
- **Organization Pricing Lists** - *allows you to create pricing lists for organizations*

More details can be found [here](./docs/functionalities.md)

<p align="center"></p>

⚙️ Installation
---------------

This guide covers Sylius B2BKit installation process using Flex recipes.

Prerequisites
-------------

You need to have installed SyliusRecipes. You can find how to install it here:
- [Sylius/SyliusRecipes](https://github.com/Sylius/SyliusRecipes)

We work on stable, supported and up-to-date versions of packages. We recommend you to do the same.

| Package       | Version |
|---------------|---------|
| PHP           | \>= 8.1 |
| sylius/sylius | 1.13.x  |
| MySQL         | \>= 5.7 |

Installing Sylius B2BKit
-----------------

1. ### Run ###

    ```bash
    composer require sylius/b2b-kit --no-scripts
    ```

2. Configure Sylius B2B Kit

   Please follow instructions included in [plugin configuration](./docs/01.1-configuration.md) file.

3. Add traits to your entities

   If you extend in your project any of entities listed below, please follow the instructions regarding to how to add B2BKit required traits and interfaces into your entities:

   - [App/Entity/Addressing/Address.php](./docs/01.2-address.md)
   - [App/Entity/Customer/Customer.php](./docs/01.3-customer.md)
   - [App/Entity/Customer/CustomerGroup.php](./docs/01.4-customer-group.md)
   - [App/Entity/Order/Order.php](./docs/01.5-order.md)
   - [App/Entity/ShopUser/ShopUser.php](./docs/01.6-shop-user.md)

Documentation
-------------

The full documentation of Sylius B2BKit is available in the [docs](./docs) directory.

---
layout: post
title: "Data Modeling Lab"
date: 2026-05-10
author: Hays Laidlaw
---
For this lab, I modeled a database for an online grocery shopping application. The basic idea is that a grocery store owner can list products, and customers can register, create pickup or delivery orders, add grocery items, and finalize those orders.

## Assumptions

Before making the diagrams, I made a few assumptions about how the app should work:

- A customer has to register before placing an order.
- A customer can place many orders.
- Each order belongs to one customer.
- An order can contain many grocery items.
- A product can appear in many different orders.
- Because orders and products have a many-to-many relationship, I need an OrderItem table between them.
- Each product has one manufacturer.
- One manufacturer can make many products.
- A finalized order should no longer be editable by the customer.
- I am not modeling payments, employees, coupons, or multiple store locations yet.

## User Stories

### User Story 1

As a store owner, I want to add grocery items with a name, price, description, quantity available, and manufacturer so customers can see what is for sale.

Size: Medium

Connections: This connects to the Product and Manufacturer entities. It also has to exist before customers can add items to an order.

### User Story 2

As a customer, I want to register with my name, address, and phone number so I can create an account and place orders.

Size: Small

Connections: This connects to the Customer entity. A customer account must exist before an order can be created.

### User Story 3

As a customer, I want to start an order for pickup or delivery so I can choose how I will receive my groceries.

Size: Medium

Connections: This connects Customer to CustomerOrder. It depends on the customer registration story.

### User Story 4

As a customer, I want to add food items to my order with a quantity, special instructions, and substitution preference so the store knows exactly what I want.

Size: Large

Connections: This connects CustomerOrder, OrderItem, and Product. It depends on the product listing story and the order creation story.

### User Story 5

As a customer, I want to finalize my order with a fulfillment date and time so the store knows when to prepare it.

Size: Medium

Connections: This connects to CustomerOrder. It depends on the order having selected items. Once finalized, the order should no longer be editable by the customer.

## Entities

The main entities in my model are:

- Customer
- CustomerOrder
- OrderItem
- Product
- Manufacturer

I used CustomerOrder instead of just Order because order can be a reserved word in SQL.

## LucidChart ER Diagram

This diagram shows the conceptual relationships between the main entities.

![LucidChart ER diagram]({{ "/assets/images/lucid.png" | relative_url }})

The most important relationship in this diagram is the connection between CustomerOrder, OrderItem, and Product. An order can contain many products, and the same product can appear in many orders. Because of that, I used OrderItem as a bridge table. This table stores details that belong to a product only when it is part of a specific order, like quantity, special instructions, and whether substitutions are allowed.

## Redgate Data Model

This diagram shows the database schema version of the same model.

![Redgate schema]({{ "/assets/images/redgate.png" | relative_url }})

In the Redgate model, the entities become database tables with primary keys, foreign keys, and data types. The main relationships are:

- One Customer can have many CustomerOrder records.
- One CustomerOrder can have many OrderItem records.
- One Product can appear in many OrderItem records.
- One Manufacturer can make many Product records.

## Reflection

I think this model works well for the basic version of an online grocery shopping app. It covers the main things the application needs: customers, grocery products, manufacturers, orders, and individual items inside each order.

The part that made the most sense to me was using OrderItem as the bridge between orders and products. At first, it might seem like products could just be listed directly inside an order, but that would not work well in a relational database. Since one order can have many products and one product can be used in many orders, the separate OrderItem table makes the design cleaner.

The most complicated part was deciding what belongs in CustomerOrder versus what belongs in OrderItem. For example, the fulfillment date and order status belong to the whole order, so they belong in CustomerOrder. Quantity, special instructions, and substitution preference only apply to a specific item in the order, so they belong in OrderItem.

There are also some features that this model does not handle yet. A real grocery app would probably need payment information, order totals, employee fulfillment tracking, delivery addresses that can change per order, product categories, and maybe multiple store locations. Inventory could also get complicated because the quantity available might change while customers are building orders.

Overall, this lab helped me see how user stories connect to database design. The stories describe what users need to do, and the data model shows what information has to be stored to make those actions possible.
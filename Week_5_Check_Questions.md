# Week 5: Preliminary Design — Check Questions

## 1. What is the difference between preliminary and detailed design?

Preliminary design explains how the whole system will be divided into its main parts and how those parts will communicate with one another. It defines the system architecture, data model and interfaces. Detailed design comes after preliminary design and explains how the inside of each part will work.

## 2. Why is microservices a poor choice for a five-person student project?

Microservices divide an application into many small services that communicate over a network, often with separate databases. For a five-person student group, managing the network communication, deployment and separate services would create too much extra work. The notes explain that this overhead could take attention away from building the actual project.

## 3. In which layer does “a sale cannot exceed stock” belong, and why not in the route?

This rule belongs in the **Services layer** because services contain the business rules of the application. The route layer should only map a URL and HTTP method to the correct controller function. Keeping the rule in the service layer also makes it possible to test the rule directly without starting the web server.

## 4. A student stores a sale with columns `item1`, `qty1`, `item2`, `qty2`. Which normal form does this break, and how do you fix it?

This design breaks **First Normal Form (1NF)** because it uses repeating groups of product and quantity columns in one table. The notes recommend creating a separate `sale_items` table, with one row for each product in a sale. Each row can contain fields such as `sale_id`, `product_id`, `quantity`, `unit_price` and `unit_cost`. The `sale_id` links each item to its sale.

## 5. Which status code should `POST /api/sales` return when stock is too low: 400, 404, 409 or 500? Justify it.

It should return **409 Conflict**. The request may be correctly formatted, but it conflicts with the current stock available in the system. The notes give insufficient stock as an example of a conflict and list `409` as the main error for `POST /api/sales`.



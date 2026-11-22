
Design Decision Records

Design decision records explain the important technical choices made during the development of MarketStock.

D-01: Use a Layered Architecture

Date: 2026-11-22

Context: MarketStock needs to manage products, sales, stock, customers and reports. The system should also be easy for team members to develop and maintain.

Decision: Use a layered architecture that separates the presentation layer, application layer and data layer.

Consequences: Each layer has a specific responsibility, making the system easier to understand, test and maintain. Changes in one layer can be made with less effect on the others.

D-02: Use SQLite Instead of MySQL

Date: 2026-11-22

Context: MarketStock is designed for one shop and will run on one server. The development team needs a database that is simple to set up and manage.

Decision: Use SQLite through Node’s built-in node:sqlite module.

Consequences: SQLite requires little setup and stores the database in one file, making it easy to back up. However, it is not suitable for many shops writing to the database at the same time. PostgreSQL could be considered if the system grows.

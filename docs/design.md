MarketStock – Preliminary Design

1. Introduction

MarketStock is a web-based stock and sales management system designed to help shop owners and assistants manage products, monitor stock levels, record sales, manage customer credit and repayments, and generate daily sales reports.

The purpose of this document is to describe the preliminary design of MarketStock, including its architecture, database structure, API endpoints and design decisions.

2. System Architecture

2.1 Architectural Style

MarketStock will use a client-server architecture with a layered monolithic server.

The client will be accessed through a web browser on a phone or computer. The server will handle requests, process business rules and communicate with the SQLite database.

The server will be divided into four layers:

1. Routes
2. Controllers
3. Services
4. Repositories

2.2 Justification

A layered architecture is suitable because it separates the responsibilities of the system. Routes handle incoming requests, controllers validate requests and prepare responses, services handle business rules, and repositories manage database operations. This makes the system easier to develop, test and maintain. The client-server approach also allows users to access MarketStock through a browser without installing a separate desktop application.

The design also considers performance requirement NFR-02, which requires the system to respond within three seconds on a 3G connection. A lightweight web interface is therefore preferred over a heavy front-end framework.

2.3 Architecture Diagram

The architecture consists of a browser client, four server layers and a SQLite database.

2.4 Deployment Design

During development, the MarketStock server will run on a student’s laptop. A phone connected to the same Wi-Fi hotspot will access the application through its browser.

For real-world use, the server could be deployed on a small cloud server.

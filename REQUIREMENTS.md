# Requirements

## Business Use Cases

### 1. Music Album CRUD Management

The core use case: a REST API for creating, reading, updating, and deleting album records, backed by a single-page AngularJS frontend.

### 2. Multi-Database Portability

Run the same application against different persistence backends (H2, MySQL, Postgres, MongoDB, Redis) by switching a Spring profile, allowing teams to evaluate or migrate databases without code changes.

### 3. Cloud Foundry Deployment with Auto-Configuration

Automatically detect bound cloud services and activate the correct database profile, enabling a cloud-native deployment pattern where infrastructure choices are externalized from the application.

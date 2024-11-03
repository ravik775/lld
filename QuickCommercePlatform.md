# Design Class Diagram for a Quick Commerce Platform

## Requirements:

- There are multiple **stores** which have items that **users** need delivered to their doorsteps.
- There are multiple **delivery partners** who can pick up items from a store and deliver them to a user's home.
- Some partners can be **online** (i.e., they should be considered for making a delivery), whereas some can be **offline** (these partners should not be considered for making a delivery).
- At any given point in time, we can have multiple available partners and multiple **orders** (representing users trying to order grocery items) live in the system. 

### Partner-Task Matching:
- To match a partner with a task, we consider the **distance** between the partner's current location and the distance from the pickup store. 
- The partner closest to the store should be assigned the task.
- The criteria for matching a partner and a task may change in the future.

### Inventory Management:
- Every store has a **limited inventory** of items. Before accepting an order, we need to ensure there is sufficient stock to fulfill the order.

### Partner Location Tracking:
- The partner's mobile app sends their **latest location** every few seconds. We need to store this information to track partners effectively.

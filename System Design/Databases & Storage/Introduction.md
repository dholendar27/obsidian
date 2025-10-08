## Why does each service has its own database?
## Data isolation
1. Minimizes the risk of data corruption and accidental modifications
2. Tailored data models and schemas for each service
3. Enhanced security through limited access to specific databases.
### Scalability & performance
1. Shared databases can lead to connection and performance issues
2. separate database for each service optimize performance and scalability
3. Avoid conflicts and bottlenecks
### Loose Coupling & Independence
1. sharing a database created tight coupling between services
2. Separate databases enable independent evolution of each service
3. Independent evolution allows easier modification, testing, and deployment
4. Loose coupling supports a flexible microservice architecture
# Advanced server architecture

The server system is composed of three main components: the authentication server, the REST API server and the database server. The authentication server is responsible for authenticating users and issuing JSON Web Tokens (JWTs). The REST API server is responsible for handling requests from the client and communicating with the database server. The database server is responsible for storing and retrieving data.

```mermaid
graph LR
    client(Client) -->|requests with credentials| authServer(Authentication Server)
    authServer -->|validates and returns JWT| client
    client -->|requests with JWT| apiServer(REST API Server)
    apiServer -->|verifies JWT| authServer
    apiServer -->|queries| dbServer(Database Server)
    client -->|requests| fileServer(File Server)
    apiServer -->|stores/retrieves| fileServer

    subgraph Authentication
    authServer
    end

    subgraph Data Handling
    apiServer
    dbServer
    end

    subgraph File Management
    fileServer
    end
```

1. The Client makes requests to the Authentication Server for authentication.
1. Upon successful authentication, the Authentication Server returns a JWT to the Client. (All of the servers share the same secret key for verifying JWTs.)
1. The Client makes direct requests to the REST API Server for various operations. API server may check the validity of the shared JWT with the authentication server. 
1. The REST API Server queries the Database Server for data-related operations.
1. The Client might directly request resources from the File Server based on the data returned by the REST API Server.
1. The REST API Server can also interact with the File Server for storing (or retrieving) files.

Separating the authentication, database, REST API, and file server into distinct components in a system architecture offers several advantages:

- **Enhanced Security:**
  - Isolation of Concerns: Each server (authentication, database, etc.) can focus on a specific set of security concerns. For instance, the authentication server can be optimized for securing user credentials and managing tokens, while the database server can be focused on protecting data.
  - Reduced Risk of Cascading Failures: If one component is compromised (e.g., the file server), the other components (like the database or authentication server) remain secure, limiting the overall impact.
- **Scalability and Performance Optimization:**
  - Independent Scaling: Each component can be scaled based on its specific load and requirements. For example, you might need more resources for the file server but not for the authentication server.
  - Load Management: By distributing the workload across multiple servers, you can manage and allocate resources more efficiently.
  - Dedicated Resources: Each server can be optimized for its specific tasks, leading to better performance. For example, a database server can be optimized for fast data retrieval and storage, while a file server might be optimized for large data throughput.
  - Parallel Processing: Different servers can handle requests simultaneously, improving overall response times and throughput.
- **Maintainability and Flexibility:**
  - Easier Updates and Maintenance: Updating or maintaining one component, like the REST API, can be done without affecting the other parts of the system.
  - Modular Development: Different teams can work on different components simultaneously, reducing development time and complexity.
  - Technology Agnosticism: Each component can be built with the most suitable technology stack for its purpose without being constrained by the choices made for other components.
- **Fault Tolerance and Reliability:**
  - Reduced Impact of Failures: If one component fails (e.g., the file server), the other components (like the REST API and authentication server) can continue functioning, possibly with reduced functionality.
  - Easier Troubleshooting and Recovery: Isolating components makes it easier to identify and fix issues without impacting the entire system.

## Task: Implementing a more advanced server architecture

1. Use the example database
1. Clone the example servers (we'll be using TypeScript from now on).
    - [Media API server](https://github.com/ilkkamtk/hybrid-media-api)
    - [Authentication server](https://github.com/ilkkamtk/hybrid-auth-server)
    - [File server](https://github.com/ilkkamtk/hybrid-upload-server)
    - [Shared types for all servers](https://github.com/ilkkamtk/hybrid-types)
1. Create `.env` files for all servers based on the .env.sample files
    - Add your own DB settings
    - Share the same secret key for verifying JWTs between all servers
    - Note that the servers use the same database and run concurrently on different ports
1. Install dependencies and test run the servers `npm i && npm run dev`

### Authentication server

1. Read documentation: <http://localhost:AUTH-SERVER-PORT/>
1. Test the endpoints (Postman or similar)
1. Store the JWT from login response for testing other servers

### Media API server

1. Review the code and test that the existing media endpoints work as expected
   - e.g. `GET http://localhost:3000/api/v1/media`, refer to route files for other endpoints
1. Start writing missing endpoints
   - for the _models_ refer to `@sharedTypes/DBTypes` (`hybrid-types/DBTypes.ts`, see `tsconfig.json`)

### File server

1. Review the code, figure out how uploads work and how they are integrated to media api
1. Test uploading media files 

Implement TODOs in the teacher's lecture examples (see link in Oma).

Start designing your individual project (see Oma for details) and implementing features you would need for the application.

**Returning:** Check assignment in OMA.

### Extras

- Security considerations
   - CORS
   - HTTPS
   - Rate limiting
   - Logging
   - Database access
   - Least Privilege Principle
   - Network security
   - Physical security

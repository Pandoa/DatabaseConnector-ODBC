# Code Example
This section contains several code examples to get you started using `Database - Connector (ODBC)`.

# Blueprints
!> Examples coming soon.

# C++
## Creating a Pool
A pool is a list of connected clients to a database. It allows to have multiple connections without having to manage 
them and prevents the additional connection cost.

The following code shows how to create a pool in C++:
```cpp
#include "Database/Pool.h" // For UDatabasePool class.

void UMyClass::CreateMyPool()
{
    UDatabasePool::CreatePool
    (
        TEXT("MariaDB ODBC 3.1 Driver"), // Driver Name
        TEXT("root"),                    // Username
        TEXT(""),                        // Password
        TEXT("127.0.0.1"),               // Database Server
        3306,                            // Database Port
        TEXT("my_database"),             // Database Name
        5,                               // Pool Size

        // Callback called when the pool has been created.
        FDatabasePoolCallback::CreateLambda([](EDatabaseError Error, UDatabasePool* Pool) -> void
        {
            if (Error == EDatabaseError::None)
            {
                // We have a connected pool.
            }
            else
            {
                // Pool creation failed.
                // Check Error or the Output log to know why.
            }
        })
    );
}
```
## Query your Database
To query the Database, use the `Query` method of you pool.
This method requires three parameters:
1. **Query**: The SQL query to execute. Use `?` to reference a parameter.
2. **Parameters**: The parameters for our query. Inserted following the order of the `?` in the query.
3. **Callback**: The callback called when the query is over (either success or fail).

The following code shows a basic use case:
```cpp
void UMyClass::InsertNewUser()
{
    TArray<FDatabaseValue> QueryParameters = 
	{
		TEXT("Username"),
		TEXT("MyHashedPassword"),
		FDatabaseDate::Now(),
		10
	};

	Pool->Query
	(
		TEXT("INSERT INTO tb_user VALUES (NULL, ?, ?, ?, ?)"),  // SQL query
		QueryParameters,										// SQL parameters
		
		// Callback executed when query 
		FDatabaseQueryCallback::CreateLambda([](EDatabaseError Error, const FQueryResult& Result) -> void
		{
			if (Error == EDatabaseError::None)
			{
				// Query executed.
			}
			else
			{
				// Query failed to execute.
			}
		})
	);
}
```
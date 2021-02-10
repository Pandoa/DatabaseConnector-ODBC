# Code Example
This section contains several code examples to get you started using this plugin.

## Blueprints

## C++

### Creating a Pool

```cpp
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
        5,                               // Port

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
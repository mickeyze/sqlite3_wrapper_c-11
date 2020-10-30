# SQLite3 Wrapper

This is very simple but useful wrapper around `sqlite3_*` C functions with using latest C++ standard.

# Features
* Header only library
* Very idiomatic and follows original sqlite terms
* Minimum entities(db, statement, exception, type_traits)
* Supports transactions
* Supports `std::string_view`, `std::optional`, `std::nullopt`, `nullptr`
* Extendable for any new user types via template specialization of `type_traits'

# Example
```cpp
#include <sqlite3_wrapper/sqlite3_wrapper.h>

namespace sqlite = sqlite3_wrapper;

try
{
    sqlite::db db("test.db");
    db.execute(R"(
        PRAGMA journal_mode = WAL;
        PRAGMA synchronous = NORMAL;
    )");

    auto select_statement = db.prepare(R"(
        SELECT id, login, name
        FROM accounts
        WHERE id = ?
    )");
    select_statement.execute(10);

    int id;
    std::string login;
    std::optional<std::string> name;
    if (select_statement.fetch(id, login, name))
    {
        // succeeded
    }

    auto insert_statement = db.prepare(R"(
        INSERT INTO accounts(id, login, name)
        VALUES (?, ?, ?)
    )");

    db.begin();
    insert_statement.execute(1, "login1", std::nullopt);
    insert_statement.execute(2, "login2", "Bob");
    db.commit();
}
catch (const sqlite::exception &e)
{
}
```

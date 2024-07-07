```python
from aiogram_magick.sqlite import SqliteStorage

# By default, SqliteStorage is configured to:
#    - Commit changes on 30 minute idle and on shutdown;
#    - Cache states (up to 20 entries) and data (up to 10 entries);
#    - Ignore any exceptions;
#    - To avoid file corruptions on shutdown any `sqlite3.OperationalError`s
#      are printed using `traceback.print_exception` instead of raised normally.
dp = Dispatcher(storage=SqliteStorage('aiogram.sqlite'))
```

## Configuration

### Database path

```python
SqliteStorage('/path/to/database/file.sqlite')
```

### Commit frequency

By default, [`SqliteStorage`](#aiogram_magick.sqlite.SqliteStorage) commits changes (internally saved in [`__commit`](#aiogram_magick.sqlite.SqliteStorage.__commit)) on:

* the first request of changing a state or data;
* After [`idle_to_commit`](#aiogram_magick.sqlite.SqliteStorage.idle_to_commit) seconds of idling between either [`set_state()`](#aiogram_magick.sqlite.SqliteStorage.set_state), [`set_data()`](#aiogram_magick.sqlite.SqliteStorage.set_data) or [`update_data()`](#aiogram_magick.sqlite.SqliteStorage.update_data);
* [`close()`](#aiogram_magick.sqlite.SqliteStorage.close).

### Key factory

[`SqliteStorage`](#aiogram_magick.sqlite.SqliteStorage) does not provide an option to configure the key factory that is used for storing keys in the database.
It also does not use `aiogram.fsm.storage.base.DefaultKeyFactory` nor bases on it. The reason is simple: `DefaultKeyFactory` does not provide a method to convert `str` back to `StorageKey` that can be used to save data between reloads.

[`__key_to_sqlite`](#aiogram_magick.sqlite.SqliteStorage.__key_to_sqlite) and [`__sqlite_to_key`](#aiogram_magick.sqlite.SqliteStorage.__sqlite_to_key) uses this format to work with `StorageKey`s:
```
<bot id>:<chat id>:<user id>
```

???+ warning
    `thread_id`, `business_connection_id` and `destiny` fields of `StorageKey` are **ignored**.

## API Reference

::: aiogram_magick.sqlite

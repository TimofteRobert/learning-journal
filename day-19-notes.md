# Day 19 - Refactoring with Helper Functions

## Topics learned

Today I focused on improving code structure instead of adding new features.

The main goal was to reduce duplicated code in the service layer.

## DRY principle

Learned the idea of DRY (Don't Repeat Yourself).

When the same code appears in multiple places, it can often be moved into a reusable helper function.

## Database helper function

Created a helper:

python

def get_cursor():

conn = get_connection()

cur = conn.cursor()

return conn, cur

All CRUD service functions now use:

python

conn, cur = get_cursor()

## Multiple return values

Learned that Python functions can return multiple values.

python

return conn, cur

And they can be unpacked:

python

conn, cur = get_cursor()

## Resource cleanup

Even with the helper function, each service function is still responsible for:

closing the cursor

closing the connection

using:

python

finally:

cur.close()

conn.close()

## Recognizing repeated patterns

Learned to identify repeated structures in CRUD functions instead of viewing each function separately.

This is an important step toward writing cleaner and more maintainable code.

## Next step

Continue refactoring repeated dictionary creation using another helper function.
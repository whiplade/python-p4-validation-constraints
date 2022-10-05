# SQLAlchemy Constraints

## Learning Goals

- Learn how to implement SQLAlchemy constraints

***

## Key Vocab

- **Validation**: Validation is an automatic check to ensure that data entered is sensible and feasible.

***

## Introduction

Invalid values in our database can cause all kinds of problems when we
want to use the data. To keep invalid values out of our database we can use SQLAlchemy
constraints. These constraints work at a database level to
control the data we add to the database.

***

## Unique and nullable Constraint

The nullable constraint allows us to make sure the values added to the columns are not null.
We can define this by adding the argument `nullable=False`.

If its required that the values in your columns must be unique then the unique constraint can be specified by adding the argument `unique=true` to the column definition.

***

### CheckConstraint

Check Constraints can be created for columns and tables.
The text of the check constraint is passed directly through to the database. Some databases like MySQL do not support
Check Constraints.

Here is an example of a Patient table that uses constraints to control input of
birth_year and death_year. You can find the complete code in `bin/constrains.py`

```py
class Patient(base):
    __tablename__ = 'patient'
    name = Column(String(length=50), primary_key=True)
    birth_year = Column(Integer,
                        CheckConstraint('birth_year < 2023'),
                        nullable=False)
    death_year = Column(Integer)
    __table_args__ = (
        CheckConstraint('(death_year is NULL) or (death_year >= birth_year)'),
    )

```

Here we have a column Check Constraint which makes sure we cannot add a birth_year that is greater than 2023. We also have a table
Check Constraint that makes sure the death year is after the birth year or the death_year is null meaning that the patient is still alive.

## Conclusion

SQLAlchemy constraints allow us to control input at a database level. They allow us to use any valid SQL and numeric comparisons to validate input. In the next lesson we will learn how to control input at a Python ORM level.
***

## Resources

- [SQLAlchemy constraints](https://docs.sqlalchemy.org/en/14/core/constraints.html)

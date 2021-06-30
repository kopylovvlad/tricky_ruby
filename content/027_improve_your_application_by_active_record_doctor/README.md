## Improve your application by active_record_doctor

[link to the gem](https://github.com/gregnavis/active_record_doctor)

The gem active_record_doctor helps you to improve your model and follow best practices. There are many checkers that will check your models.

For example:

- index unindexed foreign keys
- detect extraneous indexes
- detect unindexed deleted_at columns
- detect missing foreign key constraints
- detect models referencing undefined tables
- detect uniqueness validations not backed by an unique index
- detect missing non-NULL constraints
- detect missing presence validations
- detect incorrect presence validations on boolean columns
- detect incorrect values of dependent on associations
- detect primary keys having short integer types
- detect mismatched foreign key types

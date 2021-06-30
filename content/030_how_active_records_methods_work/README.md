## How active records methods work

When you need to be sure that at least one item in database exist, you don't need to use .`.present?` method or `.count > 0`. There are more useful method that will exec less load to database.

Current Rails -> "6.1.4"

|     | Method              | SQL                                    | Comment                                 |
| --- | ------------------- | -------------------------------------- | --------------------------------------- |
| 🤔  | User.all.count      | `SELECT COUNT(*) FROM "users"`         | It will collect all data from the table |
| 👎  | `User.all.present?` | `SELECT "users".\* FROM "users"`       | It takes all data from a table          |
| 👎  | `User.all.blank?`   | `SELECT "users".\* FROM "users"`       | It takes all data from a table          |
| 👍  | `User.all.any?`     | `SELECT 1 AS one FROM "users" LIMIT 1` | Better to use                           |
| 👍  | `User.all.empty?`   | `SELECT 1 AS one FROM "users" LIMIT 1` | Better to use                           |
| 👍  | `User.all.none?`    | `SELECT 1 AS one FROM "users" LIMIT 1` | Better to use                           |
| 👍  | `User.all.exists?`  | `SELECT 1 AS one FROM "users" LIMIT 1` | Better to use                           |

[Source - Euruko 2021](https://www.youtube.com/watch?v=XgA_2pJtJEM)

# SQL UNION
### Introduction
Yesterday I was updating some Ruby gems on one of the projects that we have at my company, and noticed one called [active_record_union](https://github.com/brianhempel/active_record_union). I have to admit that I had never heard about SQL's `UNION` operation, so that gem generated a lot of curiosity on me. That's why I decided to get more information about it and write this learning!

### SQL UNION
The purpose of SQL Union is to combine the results of several queries into a single one. For example, let's assume that we have the following table in our DB, representing Users. 

| UserID | Country | Age |
|:-:|:-:|:-:
| 1 | United States | 21
| 2 | Germany | 19
| 3 | Japan | 27
| 4 | Spain | 20

Let's assume that we want to search two types of users:

- The users over 23 years old
- The users located on the United States

In order to do that, you'll need the following two queries:

- `SELECT * FROM Users WHERE Country = "United States"`
- `SELECT * FROM Users WHERE Age > 23`

However, this will require two DB calls; usually you want to obtain your result using as few DB calls as possible, and therefore this won't be an optimal solution. Besides, using different queries requires a manual processing of the results to combine them into one (as each query returns a result set)

This is where `UNION` comes to play. Using `UNION` you can both two queries into a single one, and therefore obtain the same results in a single DB operation.

```sql
SELECT * FROM Users WHERE Country = "United States"
UNION
SELECT * FROM Users WHERE Age > 23
```

| UserID | Country | Age |
|:-:|:-:|:-:
| 1 | United States | 21
| 3 | Japan | 27

By doing this, we will just need one DB operation, and also we will get all the matching users in the same result without needing to do any manual combination. 

#### But, can't I just use OR?
Probably you asked yourself that question when you saw the previous section. And you're right: you could also combine both queries into one by using several conditions linked with `OR`, like this:

```sql
SELECT * FROM Users WHERE (Country = "United States") OR (Age > 23)
```

However, **the key is in the performance of each operation**. If you have indexes in the columns you're using to do the search, you will probably be better using `UNION`.

Using `OR` will make the SQL operation to use indexes just for one of the columns, or even not using them at all. On the other hand, using `UNION` resolves both queries separately, enabling the use of the most optimal query plan for each query, which probably means using the index of each of the columns if they have one. You can find a more detailed explanation about it in the following links:
- [SQL Performance UNION vs OR](https://stackoverflow.com/questions/13750475/sql-performance-union-vs-or)
- [OR vs UNION](https://www.postgresql.org/message-id/004f01c34c9d%24ce9edcc0%242766f30a%40development.greatgulfhomes.com)

### Union over different tables
Also, `UNION` enables the obtention of results of several tables, instead of limiting to a simple one. For example, let's assume that now we have two tables: one for customers and one for suppliers:

| CustomerID | Country |
|:-:|:-:|
| 1 | United States |
| 2 | Germany |
| 3 | United States |
| 4 | Spain |

| SupplierID | Country |
|:-:|:-:|
| 1 | United States |
| 2 | France |

If we wanted to obtain all the customers or suppliers in USA, we could do something like the following:

```sql
SELECT 'Customer' AS Type, CustomerID AS ID FROM Customers WHERE Country = "United States"
UNION
SELECT 'Supplier', SupplierID FROM Suppliers WHERE Country = "United States"
```

And we would obtain something like this:

| Type | ID |
|:-:|:-:|
| Customer | 1 |
| Customer | 3 |
| Supplier | 1 |

Notice how we didn't even need to specify the column names in the second query! As the columns are in the same position, `UNION` automatically knows which column name has to be used. 

*(It's important to consider that this will only be possible if both operations returned the same number of columns, and if its types were similar)*.

### UNION ALL
Something that I didn't mention is that, when combining the results of the queries, `UNION` does an extra operation to remove repeated rows. Going back to the first example, this would have been useful if instead of searching for those users older than 23, we would have wanted to search those older than 20: the user 1 matches both condition, and therefore he would have been present in both results. But to prevent us to have repeated data, `UNION` would have filtered the results and returned his row only once. 

However, let's assume that we're completely sure that we won't have repetitions. For example, if we had a table like the following one:

| UserID | Country | Continent |
|:-:|:-:|:-:
| 1 | United States | America
| 2 | Germany | Europe
| 3 | Japan | Asia
| 4 | Spain | Europe

This time, let's assume that we want to retrieve:
- The users whose Country is United States
- The users whose Continent is Asia

We are absolutely certain that there will never be a coincidence between those two groups. Therefore, letting `UNION` search for repetitions is a waste of time. 

For those cases, we can use the `UNION ALL` operation: it also combines several queries, but without checking if there are repetitions in the result. Therefore, the following query:

```sql
SELECT * FROM Users WHERE Country = "United States"
UNION
SELECT * FROM Users WHERE Continent = "Asia"
```
Will return as a result:

| UserID | Country | Continent |
|:-:|:-:|:-:
| 1 | United States | America
| 3 | Japan | Asia

And it will actually be a faster than using a simple `UNION` (how much faster will depend on the size of your result). 

However, it also requires you to be more careful about your operations,because if you combine two queries that can have repeated results...

```sql
SELECT * FROM Users WHERE Country = "Spain"
UNION
SELECT * FROM Users WHERE Continent = "Europe"
```

| UserID | Country | Continent |
|:-:|:-:|:-:
| 4 | Spain | Europe
| 2 | Germany | Europe
| 4 | Spain | Europe

You will, in fact, end with repeated results. 

## Summary
The SQL `UNION` statement is a really useful resource if you need to search by several conditions and want to take as much profit as possible from the indexes associated to the corresponding columns. It is also really useful if you want to combine results from two tables that have a similar structure (for example, if you're using Concrete Table Inheritance and only need the common data). 

However, it also has it's downsides: you need to be aware that the repetition removal step may slow down the query a little bit if you're retrieving a lot of rows; but the good part is that you can use `UNION ALL` instead to skip that part. Of course, then you need to be completely sure that you won't have any repeated data among the different queries. As Uncle Ben would say: *"With great power, comes great responsibility"*





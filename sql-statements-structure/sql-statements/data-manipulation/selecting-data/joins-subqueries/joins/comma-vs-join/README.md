# Comma vs JOIN

A query to grab the list of phone numbers for clients who ordered in the last
two weeks might be written in a couple of ways. Here are two:

```sql
SELECT *
FROM
  clients,
  orders,
  phoneNumbers
WHERE
  clients.id = orders.clientId
  AND clients.id = phoneNumbers.clientId
  AND orderPlaced >= NOW() - INTERVAL 2 WEEK;
```

```sql
SELECT *
FROM
  clients
  INNER JOIN orders ON clients.id = orders.clientId
  INNER JOIN phoneNumbers ON clients.id = phoneNumbers.clientId
WHERE
  orderPlaced >= NOW() - INTERVAL 2 WEEK;
```

Does it make a difference? Not much as written. But you should use the second
form. Why?

- <strong>Readability.</strong> Once the WHERE clause contains more than two conditions, it
  becomes tedious to pick out the difference between business logic (only dates
  in the last two weeks) and relational logic (which fields relate clients to
  orders). Using the JOIN syntax with an ON clause makes the WHERE list
  shorter, and makes it very easy to see how tables relate to each other.

- <strong>Flexibility.</strong> Let's say we need to see all clients even if they don't have
  a phone number in the system. With the second version, it's easy; just
  change `INNER JOIN phoneNumbers` to `LEFT JOIN phoneNumbers`. Try that
  with the first version, and MySQL version 5.0.12+ will issue a syntax error
  because of the change in precedence between the comma operator and the JOIN
  keyword. The solution is to rearrange the FROM clause or add parentheses to
  override the precedence, and that quickly becomes frustrating.

- <strong>Portability.</strong> The changes in 5.0.12 were made to align with SQL:2003. If
  your queries use standard syntax, you will have an easier time switching to a
  different database should the need ever arise.

## See Also

- ["MySQL joins: ON vs. USING vs. Theta-style"](http://code.openark.org/blog/mysql/mysql-joins-on-vs-using-vs-theta-style) <span>—</span> An interesting blog entry regarding this topic.

<em>The initial version of this article was copied, with permission, from [http://hashmysql.org/wiki/Comma_vs_JOIN](http://hashmysql.org/wiki/Comma_vs_JOIN) on 2012-10-05.</em>
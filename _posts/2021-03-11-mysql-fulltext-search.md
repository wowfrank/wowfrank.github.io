---
layout: post
title: "MySQL Full-Text Search"
date: 2021-03-11 00:07:00 +0800
description: "MySQL Full-Text Search" # (optional)
img: 2021-03-11-cover-image-of-mysql-fulltext-search.jpg # Add image post (optional)
fig-caption: "MySQL Full-Text Search" # Add figcaption (optional)
tags: ['Programming', 'MySQL']
categories: ['Database', 'MySQL']
---

## Introduction

If you have used search engines like Google or Bing, you used the full-text search or FTS. The search engines collect contents from websites into databases and allow you to search based on keywords.

In addition to search engines, the FTS powers search results on websites like Blogs, News, E-commerce, and more.

Full-text search is a technique to search for documents that don’t perfectly match the search criteria. Documents are database entities that contain textual data e.g., product description, blog post, and articles.

For example, you can search for Wood and Metal, FTS can return results that contain the searched words separately, just Wood or Metal, or results that contain the words in a different order Word and Metal, or Metal and Wood.

Technically speaking, MySQL supports partial text searching by using the LIKE operator and regular expression. However, when the text column is large and the number of rows in a table is increased, using LIKE or regular expressions has some limitations:

### Performance

MySQL has to scan the whole table to find the exact text based on a pattern in the LIKE  statement or pattern in the regular expressions.

### Flexible search

Both LIKE and regular expressions can search based by matching a pattern. It is difficult to have a flexible search query e.g., to find products whose descriptions contain car  but not classic.

### Relevance ranking

There is no way to specify which row in the result set is more relevant to the search terms.

Because of these limitations, MySQL started supported full-text search in 5.6.

MySQL creates a FULLTEXT index from text data and lookup on this index using a sophisticated algorithm to determine rows that match against the search query.

### MySQL full-text search features

The following are some important features of MySQL full-text search:

### Native SQL-like interface

MySQL provides the SQL-like statement to perform full-text searches.

### Fully dynamic index

MySQL automatically updates the index of the text column whenever the data of that column changes.

### Moderate index size

The size of a FULLTEXT index is relatively small.

### Speed

Last but not least, it is fast to search based on complex search queries.

Notice that not all storage engines support the full-text search feature. In MySQL version 5.6 or later, only MyISAM and InnoDB storage engines support full-text search.

## Creating FULLTEXT Indexes for Full-Text Search

Before performing a full-text search in a column of a table, you must index its data. MySQL will recreate the full-text index whenever the data of the column changes. In MySQL, the full-text index is a kind of index that has a name FULLTEXT.

MySQL supports indexing and re-indexing data automatically for full-text search enabled columns. MySQL version 5.6 or later allows you to define a full-text index for a column whose data type is CHAR, VARCHAR or TEXT in MyISAM and InnoDB table types.

Notice that MySQL only supported the full-text index for InnoDB tables since version 5.6.

MySQL allows you to define the FULLTEXT index by using the CREATE TABLEstatement when you create the table or ALTER TABLE or CREATE INDEX statement for the existing tables.

### Create FULLTEXT index using CREATE TABLE statement

Typically, you define the FULLTEXT index for a column when you create a new table using the CREATE TABLE statement as follows:

```sql
CREATE TABLE table_name(
    column_list,
    ...,
    FULLTEXT (column1,column2,..)
);
```

To create the FULLTEXT index, you place a list of comma-separated column names in parentheses after the FULLTEXT keyword.

The following statement creates a new table named posts that has a FULLTEXT index that includes the post_content column.

```sql
CREATE TABLE posts (
  id INT NOT NULL AUTO_INCREMENT,
  title VARCHAR(255) NOT NULL,
  body TEXT,
  PRIMARY KEY (id),
  FULLTEXT KEY (body )
);
```

The following syntax defines a FULLTEXT index using the ALTER TABLE statement:

```sql
ALTER TABLE table_name  
ADD FULLTEXT(column_name1, column_name2,…)
```

In this syntax,

- First, specify the name of the table that you want to create the index after the ALTER TABLE keywords.

- Second, use the ADD FULLTEXT clause to define the FULLTEXT index for one or more columns of the table.

For example, you can define a FULLTEXT index for the productDescription and productLine columns in the products table of the sample database as follows:

```sql
ALTER TABLE products  
ADD FULLTEXT(productDescription,productLine)
```

### Create FULLTEXT index using CREATE INDEX statement

You can also use the CREATE INDEX statement to create a FULLTEXT index for existing tables using the following syntax:

```sql
CREATE FULLTEXT INDEX index_name
ON table_name(idx_column_name,...)
```

For example, the following statement creates a FULLTEXT index for the addressLine1 and addressLine2 columns of the offices table:

```sql
CREATE FULLTEXT INDEX address
ON offices(addressLine1,addressLine2)
```

Notice that for a table which has many rows, it is faster to load the data into the table that has no FULLTEXT index first and then create the FULLTEXT index, than loading a large amount of data into a table that has an existing FULLTEXT index.

### Drop a FULLTEXT index

To drop a FULLTEXT index, you use the ALTER TABLE DROP INDEX statement.

```sql
ALTER TABLE table_name
DROP INDEX index_name;
```

For example, the following statement removes the address index from the offices table:

```sql
ALTER TABLE offices
DROP INDEX address;
```

## MySQL Natural Language Full-Text Searches

### Introduction to MySQL natural-language full-text searches

In natural language full-text searches, MySQL looks for rows or documents that are relevant to the free-text natural human language query, for example, “How to use MySQL natural-language full-text searches”.

Relevance is a positive floating-point number. When the relevance is zero, it means that there is no similarity. MySQL computes the relevance based on various factors including the number of words in the document, the number of unique words in the document, the total number of words in the collection, and the number of documents (rows) that contain a particular word.

To perform natural-language full-text searches, you use MATCH()  and  AGAINST() functions. The MATCH()  function specifies the column where you want to search and the AGAINST()  function determines the search expression to be used.

### MySQL natural language full-text search example

We will use the products table in the sample database for the demonstration.

![full-text search]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-products.png)

First, create a full-text search in the productLine  column of the products  table using the ALTER TABLE ADD FULLTEXT  statement:

```sql
ALTER TABLE products 
ADD FULLTEXT(productline);
```

Second, you can search for products whose product lines contain the term Classic . You use the MATCH()  and AGAINST()  functions as the following query:

```sql
SELECT 
    productName, 
    productLine 
FROM products 
WHERE 
    MATCH(productLine) 
    AGAINST('Classic');
```

![MySQL Natural Language Full-Text Search]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-Natural-Language-Full-Text-Search.png)

To search for a product whose product line contains Classic or Vintage term, you can use the following query:

```sql
SELECT 
    productName, 
    productLine 
FROM products 
WHERE 
    MATCH(productline) 
    AGAINST('Classic,Vintage')
ORDER BY productName;
```

![MySQL Natural Language Full-Text Search example]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-Natural-Language-Full-Text-Search-example.png)

The AGAINST()  function uses IN NATURAL LANGUAGE MODE  search modifier by default, therefore, you can omit it in the query. There are other search modifiers e.g.,  IN BOOLEAN MODE   for Boolean text searches.

You can explicitly use the IN NATURAL LANGUAGE MODE  search modifier in your query as follows:

```sql
SELECT 
    productName, 
    productLine 
FROM products 
WHERE 
    MATCH(productline) 
    AGAINST('Classic,Vintage' IN NATURAL LANGUAGE MODE)
```

![MySQL Natural Language Full-Text - IN NATURAL LANGUAGE MODE]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-Natural-Language-Full-Text-IN-NATURAL-LANGUAGE-MODE.png)

By default, MySQL performs searches in the case-insensitive fashion. However, you can instruct MySQL to perform case-sensitive searches using binary collation for indexed columns.

### Sort the result set by relevance

A very important feature of full-text search is how MySQL ranks the rows in the result set based on their relevance. When the MATCH()  function is used in the WHERE clause, MySQL returns the rows that are more relevant first.

The following example shows you how MySQL sorts the result set by the relevance.

First, create a full-text search for the  productName column of the products table.

```sql
ALTER TABLE products 
ADD FULLTEXT(productName);
```

Second, search for products whose names contain  Ford  and/or  1932:

```sql
SELECT 
    productName, 
    productLine 
FROM products 
WHERE 
    MATCH(productName) 
    AGAINST('1932,Ford');
```

Here is the output:

![MySQL Natural Language Full-text search - Sort The Result set By Relevance]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-Natural-Language-Full-text-search-Sort-The-Result-set-By-Relevance.png)

The products, whose names contain both 1932  and Ford are returned first and then the products whose names contains the only Ford keyword.

There are some important points you should remember when using the full-text search:

- The minimum length of the search term defined in MySQL full-text search engine is 4. It means that if you search for the keyword whose length is less than 4 e.g., car, cat, you will not get any results.

- Stop words are ignored. MySQL defines a list of stop words in the MySQL source code distribution storage/myisam/ft_static.c

## MySQL Boolean Full-Text Searches

### Introduction to MySQL Boolean full-text searches

Besides the natural language full-text search, MySQL supports an additional form of full-text search that is called Boolean full-text search. In the Boolean mode, MySQL searches for words instead of the concept like in the natural language search.

MySQL allows you to perform a full-text search based on very complex queries in the Boolean mode along with Boolean operators. This is why the full-text search in Boolean mode is suitable for experienced users.

To perform a full-text search in the Boolean mode, you use the IN BOOLEAN MODE modifier in the AGAINST  expression. The following example shows you how to search for a product whose product name contains the Truck word.

```sql
SELECT productName, productline
FROM products
WHERE MATCH(productName) 
      AGAINST('Truck' IN BOOLEAN MODE )
```

![mysql boolean tex searches - product name with keyword Truck]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-mysql-boolean-tex-searches-product-name-with-keyword-truck.png)

Two products whose product names contain the Truck  word are returned.

To find the product whose product names contain the   Truck word but not any rows that contain  Pickup , you can use the exclude Boolean operator ( - ), which returns the result that excludes the Pickup keyword as the following query:

```sql
SELECT productName, productline
FROM products
WHERE MATCH(productName) AGAINST('Truck -Pickup' IN BOOLEAN MODE )
```

![mysql boolean tex searches with Boolean operator]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-mysql-boolean-tex-searches-with-Boolean-operator.png)

### MySQL Boolean full-text search operators

The following table illustrates the full-text search Boolean operators and their meanings:

|   Operator    |   Description    |
|:-------------:|:---------------  |
|\+   |Include, the word must be present.|
|–   |Exclude, the word must not be present.|
|\>   |Include, and increase ranking value.|
|\<   |Include, and decrease the ranking value.|
|\(\)  |Group words into subexpressions (allowing them to be included, excluded, ranked, and so forth as a group).|
|\~   |Negate a word’s ranking value.|
|\*   |Wildcard at the end of the word.|
|“”  |Defines a phrase (as opposed to a list of individual words, the entire phrase is matched for inclusion or exclusion).|

The following examples illustrate how to use boolean full-text operators in the search query:

**To search for rows that contain at least one of the two words: mysql or tutorial**

```
‘mysql tutorial’
```

**To search for rows that contain both words: mysql and tutorial**

```
‘+mysql +tutorial’
```

**To search for rows that contain the word “mysql”, but put the higher rank for the rows that contain “tutorial”:**

```
‘+mysql tutorial’
```

**To search for rows that contain the word “mysql” but not “tutorial”**

```
‘+mysql -tutorial’
```

**To search for rows that contain the word “mysql” and rank the row lower if it contains the word “tutorial”.**

```
‘+mysql ~tutorial’
```

**To search for rows that contain the words “mysql” and “tutorial”, or “mysql” and “training” in whatever order, but put the rows that contain “mysql tutorial” higher than “mysql training”.**

```
‘+mysql +(>tutorial <training)’
```

**To find rows that contain words starting with “my” such as “mysql”, “mydatabase”, etc., you use the following:**

```
‘my*’
```

### MySQL boolean full-text search main features

- MySQL does not automatically sort rows by relevance in descending order in Boolean full-text search.

- To perform Boolean queries, InnoDB tables require all columns of the MATCH expression has a FULLTEXT index. Notice that MyISAM tables do not require this, although the search is quite slow.

- MySQL does not support multiple Boolean operators on a search query on InnoDB tables e.g., ‘++mysql’. MySQL will return an error if you do so. However, MyISAM behaves differently. It ignores other operators and uses the operator that is closest to the search word, for example, ‘+-mysql’ will become ‘-mysql’.

- InnoDB full-text search does not support trailing plus (+) or minus (-) sign. It only supports leading plus or minus sign. MySQL will report an error if you search word is ‘mysql+’ or ‘mysql-‘. In addition, the following leading plus or minus with wildcard are invalid: +\*, +-

- The 50% threshold means if a word appears in more than 50% of the rows, MySQL will ignore it in the search result.

## Introduction to MySQL Query Expansion

Typically, users search for information based on their knowledge. They use their experience to come up with the keywords to search for information, and sometimes, these keywords are too short.

To help users to find information based on these too-short keywords, MySQL full-text search engine introduces a concept called query expansion.

The query expansion is used to widen the search result of the full-text searches based on automatic relevance feedback (or blind query expansion). Technically, MySQL full-text search engine performs the following steps when the query expansion is used:

- First, search for all rows that match the search query.

- Second, find the relevant words in all rows from the search result.

- Third, search again based on the relevant words instead of the original keywords specified by users.

From the application perspective, you can use the query expansion when the search results are too few. You perform the searches again, but with the query expansion, to offer users more information related and relevant to what they are looking for.

To use the query expansion, you use the WITH QUERY EXPANSION search modifier in the AGAINST() function. The following illustrates the syntax of the query using the WITH QUERY EXPANSION search modifier.

```sql
SELECT column1, column2
FROM table1
WHERE MATCH(column1,column2) 
      AGAINST('keyword',WITH QUERY EXPANSION);
```

### MySQL query expansion example

Let’s look at an example of using a query expansion to see how it works.

We will use the productName column of the products table to demonstrate the query expansion feature.

![full-text search]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-products.png)

First, create the full-text search index on the productName column:

```sql
ALTER TABLE products 
ADD FULLTEXT(productName);
```

Second, search for a product whose name contains the 1992  without using query expansion.

```sql
SELECT 
    productName
FROM
    products
WHERE
    MATCH (productName) 
    AGAINST ('1992' );
```

![MySQL Query Expansion example]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-Query-Expansion-example.png)

As you see clearly from the output, the search result has two products whose names contain the term 1992 .

Third,  use query expansion to widen the search result:

```sql
SELECT 
    productName 
FROM 
    products 
WHERE 
    MATCH(productName) 
    AGAINST('1992' WITH QUERY EXPANSION);
```

![MySQL Query Expansion example]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-Query-Expansion-example-2.png)

The result set has two more rows in the when using with the query expansion. The first two rows are the most relevant and the other rows come from the relevant keyword derived from in the first two rows, e.g.,Ferrari

Notice that blind query expansion tends to increase noise significantly by returning non-relevant results. It is highly recommended that you use query expansion only when the searched keyword is short.

## Introduction to MySQL ngram full-text parser

The built-in MySQL full-text parser determines the beginning and end of words using white space. When it comes to ideographic languages such as Chinese, Japanese, and Korean, the full-text parser has a limitation that these ideographic languages do not use word delimiters.

To address this issue, MySQL provided the ngram full-text parser. Since version 5.7.6, MySQL included ngram full-text parser as a built-in server plugin, meaning that MySQL loads this plugin automatically when the MySQL database server starts. MySQL supports ngram full-text parser for both InnoDB and MyISAM storage engines.

By definition, an ngram is a contiguous sequence of a number of characters from a sequence of text. The main function of ngram full-text parser is tokenizing a sequence of text into a contiguous sequence of n characters.

The following illustrates how the ngram full-text parser tokenizes a sequence of text for different value of n:

```sql
n = 1: 'm','y','s','q','l'
n = 2: 'my', 'ys', 'sq','ql' 
n = 3: 'mys', 'ysq', 'sql'
n = 4: 'mysq', 'ysql'
n = 5: 'mysql'
```

### Creating FULLTEXT indexes with ngram parser

To create a FULLTEXT index that uses ngram full-text parser, you add the WITH PARSER ngram in the CREATE TABLE, ALTER TABLE, or CREATE INDEX statement.

Consider the following example.

First, create new posts table and adds the title and body columns to the FULLTEXT index that uses ngram full-text parser.

```sql
DROP TABLE IF EXISTS posts;

CREATE TABLE posts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    body TEXT,
    FULLTEXT ( title , body ) WITH PARSER NGRAM
)  ENGINE=INNODB CHARACTER SET UTF8MB4;
```

Second, use the SET NAMES statement sets the character set to utf8mb4.

```sql
SET NAMES utf8mb4;
```

Third, insert a new row into the posts table:

```sql
INSERT INTO posts(title,body)
VALUES('MySQL全文搜索','MySQL提供了具有许多好的功能的内置全文搜索'),
      ('MySQL教程','学习MySQL快速，简单和有趣');
```

Fourth, to see how the ngram tokenizes the text, you use the following statement:

```sql
SET GLOBAL innodb_ft_aux_table="test/posts";

SELECT 
    * 
FROM 
    information_schema.innodb_ft_index_cache
ORDER BY 
    doc_id , 
    position;
```

![MySQL ngram full-text parser example]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-ngram-full-text-parser-example.png)

This query is useful for troubleshooting purposes. For example, if a word does not include in the search results, then the word may be not indexed because it is a stopword or it could be another reason.

### Setting ngram token size

As you can see the previous example, the token size (n) in the ngram by default is 2. To change the token size, you use the ngram_token_size configuration option, which has a value between 1 and 10.

Note that a smaller token size makes smaller full-text search index and allows you to search faster.

Because ngram_token_size is a read-only variable, therefore you only can set its value using two options:

First, in the start-up string:

```sh
mysqld --ngram_token_size=1
```

Second, in the configuration file:

```sh
[mysqld]
ngram_token_size=1
```

### ngram parser phrase search

MySQL converts a phrase search into ngram phrase searches. For example, "abc" is converted into "ab bc", which returns documents that contain "ab bc" and "abc".

The following example shows you to search for the phrase 搜索 in the posts table:

```sql
SELECT 
    id, title, body
FROM
    posts
WHERE
    MATCH (title , body) AGAINST ('搜索' );
```

![MySQL ngram full-text parser phrase search]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-ngram-full-text-parser-phrase-search.png)

### Processing search result with ngram

#### Natural language mode

In NATURAL LANGUAGE MODE searches, the search term is converted to a union of ngram values. Suppose the token size is 2 or bigram, the search term mysql is converted to my ys sq and ql.

```sql
SELECT 
    *
FROM
    posts
WHERE
    MATCH (title , body)  
    AGAINST ('简单和有趣' IN natural language MODE);
```

![Natural language mode]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-ngram-full-text-parser-NATURAL-LANGUAGE-MODE.png)

#### Boolean mode

In BOOLEAN MODE searches, the search term is converted to an ngram phrase search. For example:

```sql
SELECT 
    *
FROM
    posts
WHERE
    MATCH (title , body) 
    AGAINST ('简单和有趣' IN BOOLEAN MODE);
```

![BOOLEAN MODE]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-MySQL-ngram-full-text-parser-NATURAL-LANGUAGE-MODE.png)

#### ngram wildcard search

The ngram FULLTEXT index contains only ngrams, therefore it does not know the beginning of terms. When you perform wildcard searches, it may return an unexpected result.

The following rules are applied to wildcard search using ngram FULLTEXT search indexes:

If the prefix term in the wildcard is shorter than ngram token size, the query returns all documents that contain ngram tokens starting with the prefix term. For example:

```sql
SELECT 
    id, 
    title, 
    body
FROM
    posts
WHERE
    MATCH (title , body) 
    AGAINST ('my*' );
```

![ngram wildcard search]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-ngram-wildcard-search.png)

In case the prefix term in the wildcard is longer than ngram token size, MySQL will convert the prefix term into ngram phrases and ignore the wildcard operator. See the following example:

```sql
SELECT 
    id, 
    title, 
    body
FROM
    posts
WHERE
    MATCH (title , body) 
    AGAINST ('mysqld*' );
```

![ngram wildcard search]({{site.baseurl}}/assets/img/2021-03-11/2021-03-11-ngram-wildcard-search.png)

In this example, the term “mysqld" is converted into ngram phrases: "my" "ys" "sq" "ql" "ld". Therefore all documents that contain one of these phrases are returned.

### Handling stopwords

The ngram parser excludes tokens that contain the stopword in the stopword list. For example, suppose the ngram_token_size is 2 and document contains "abc". The ngram parser will tokenize the document to "ab" and "bc".  If "b" is a stopword, ngram will exclude both "ab" and "bc" because they contain "b".

Note that you must define your own stopword list if the language is other than English. In addition, the stopwords with lengths that are greater than ngram_token_size are ignored.

## Laravel Full Text Search

```php
Product::whereRaw('MATCH (title, content) AGAINST (?)' , [$search])->get();
```

is equivalent to 

```sql
SELECT * FROM products WHERE match(title, content) againt ($search);
```

In order for this to work, you will need to add Full-Text index against your table columns in MySQL.

```php
DB::statement('ALTER TABLE products ADD FULLTEXT fulltext_index (title, content)');
```

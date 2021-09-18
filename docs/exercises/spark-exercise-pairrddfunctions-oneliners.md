== Exercise: One-liners using PairRDDFunctions

This is a set of one-liners to give you a entry point into using rdd:PairRDDFunctions.md[PairRDDFunctions].

=== Exercise

How would you go about solving a requirement to pair elements of the same key and creating a new RDD out of the matched values?

[source, scala]
----
val users = Seq((1, "user1"), (1, "user2"), (2, "user1"), (2, "user3"), (3,"user2"), (3,"user4"), (3,"user1"))

// Input RDD
val us = sc.parallelize(users)

// ...your code here

// Desired output
Seq("user1","user2"),("user1","user3"),("user1","user4"),("user2","user4"))
----

+++
date = "2016-03-02T23:34:26+01:00"
draft = true
title = "Less Work"

+++

When I receive a feature request or a bug to solve, I start thinking how I will design and implement a solution. Many developers have this tendency.

But it is better to check first if a solution is actually needed. Sometimes there is a way to remove the problem entirely. And it may be easier than implementing a solution.

At work my current project is a complex and massive data migration. In particular the migration of a specific set of objects requires deep changes in the code to accommodate the new data format for them.

Before I started to implement those changes, my boss suggested that I count how many of those objects would need to be migrated. What a good idea! It turned out that a only a dozen - out of thousand of records - had a content structure requiring them to migrate.

We found an easy way to adjust their content so that they no longer had to migrate. Thus I no longer had to change the code to deal with the migration of this set of objects. Big problem solved just by removing it :)

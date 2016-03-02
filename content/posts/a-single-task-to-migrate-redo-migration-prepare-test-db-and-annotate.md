+++
date = "2011-02-09T01:00:00+01:00"
draft = false
title = "A Single Task to Migrate, Redo Migration, Prepare Test Db and Annotate"
slug = "a-single-task-to-migrate-redo-migration-prepare-test-db-and-annotate"
aliases = [
	"a-single-task-to-migrate-redo-migration-prepare-test-db-and-annotate",
  "/2011/02/09/a-single-task-to-migrate-redo-migration-prepare-test-db-and-annotate/"
]
+++
Hi!

My first blog post to announce my first gem: [migrate-well](https://github.com/Florent2/migrate-well). It’s a very simple one, it adds a rake task `db:migrate:well` that runs the tasks `db:migrate`, `db:migrate:redo`, `rake db:test:prepare` and `annotate`. I was tired to type all those commands each time…

To install: add `gem “migrate-well”` in your Gemfile and run `bundle install`.

To run: `rake db:migrate:well`

Options (updated February 15, 2011):

* `redo=false`: don’t run the `db:migrate:redo` task
* `test=false`: don’t run the `db:test:prepare` task
* `anno=false`: don’t run the `annotate` command

Examples:

* run without annotating: `rake db:migrate:well anno=false`
* run without annotating and without redoing the migration: `rake db:migrate:well anno=false redo=false`

That’s all!

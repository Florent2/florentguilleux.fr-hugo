+++
date = "2011-05-30T01:00:00+01:00"
draft = false
title = "PostgreSQL Geometric Types in Ruby and Rails"
slug = "postgresql-geometric-types-in-ruby-and-rails"
aliases = [
	"postgresql-geometric-types-in-ruby-and-rails",
  "2011/05/30/postgresql-geometric-types-in-ruby-and-rails/"
]
+++
PostgreSQL supports several [geometric types](http://www.postgresql.org/docs/8.4/static/datatype-geometric.html) natively (point, line, rectangle, circle…) and [operations and operators](http://www.postgresql.org/docs/8.4/static/functions-geometry.html) on these types. Thus PostgreSQL can calculate the distance between 2 points, the diameter of a circle, check if 2 lines intersect or not, if a point is contained in a polygon…

Let see how to leverage these capabilities in Rails with a small demo application that stores the coordinates of different nodes in a 2D plane.

First create the application by choosing PostgreSQL for the database: `rails new demo-geo-pg -d postgresql`

and create the development database: `rake db:create`

Then generate the Node model: `rails g model Node name:string coordinates:point`

The `coordinates` attribute will store the abscisse and ordinate values of the nodes, it has the PostgreSQL `point` type. Rails has generated a migration containing the line `t.coordinates :point` to generate the corresponding column. However if we run the migration as it is, it will fail because point is not a native Rails migration type. We need to change this line into `t.column :coordinates, :point`. We can now run the migration: `rake db:migrate`

Now use the the Rails console (`rails c`) to create a few Node records:

`> Node.create! : Name => “Node # 1”: coordinates => “(1.1)”`
`> Node.create! : Name => “Node # 2”: coordinates => “(2.2)”`
`> Node.create! : Name => “Node # 3”: coordinates => “(-2, -2)”`
`> Node.create! : Name => “Node # 4”: coordinates => “(4, -2)”`

In Ruby we handle PostgreSQL geometric values with strings following a defined syntax: for the `point` type it is `(x,y)` (these syntaxes are described in [the PostgreSQL documentation](http://www.postgresql.org/docs/8.4/static/datatype-geometric.html)). If this syntax is not respected, PostgreSQL returns an error when trying to save the record in the database:

`> Node.create! : Name => “Node # 5”: coordinates => “1”`

`ActiveRecord:: StatementInvalid: PGError: ERROR: invalid input syntax for type point: “1”`

We could validate the format of the coordinate attribute with:
`validates_format_of: coordinates, :with => /\(-?\d+(?:.\d+)?,-?\d(?:.\d+)?\)/`

Now let’s use the PostgreSQL geometric operators on our nodes. Here’s how to get all the nodes located in the circle with center (2,2) and radius 2, using the `contains @>` operator:

`> Node.where(“circle ‘<(2,2),2>’ @> coordinates”)`
`=> [#<Node id: 1, name: “Node #1”, coordinates: “(1,1)”, created_at: “2011-05-28 15:00:38”, updated_at: “2011-05-28 15:00:38”>, #<Node id: 2, name: “Node #2”, coordinates: “(1,1)”, created_at: “2011-05-28 15:00:48”, updated_at: “2011-05-28 15:00:48”>]`

Implementation of an instance method to find the closest node, using the `Distance Between <->` operator:


`def nearest
  where(“id <> #{id}”).select(“*, coordinates <-> point ‘#{coordinates}’ as distance”).order(“distance asc”).limit(1).first
end`

`> Node.first.nearest`
`=> #<Node id: 2, name: “Node #2”, coordinates: “(1,1)”, created_at: “2011-05-28 15:00:48”, updated_at: “2011-05-28 15:00:48”>`


In some cases the native PostgreSQL geometry types with their functions and operators are adequate and very useful for representing and processing spatial data. When this is not the case, you should use a dedicated tool like [PostGIS](http://www.postgis.org/) (there is [an adapter for ActiveRecord](http://virtuoso.rubyforge.org/activerecord-postgis-adapter/)). See [this entry on stackoverflow](http://stackoverflow.com/questions/1023229/spatial-data-in-postgresql) for more information.

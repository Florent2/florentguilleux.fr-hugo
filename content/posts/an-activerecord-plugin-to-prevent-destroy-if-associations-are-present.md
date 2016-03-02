+++
date = "2011-08-07T01:00:00+01:00"
draft = false
title = "An ActiveRecord Plugin to Prevent Destroy if Associations Are Present"
slug = "an-activerecord-plugin-to-prevent-destroy-if-associations-are-present"
aliases = [
	"an-activerecord-plugin-to-prevent-destroy-if-associations-are-present",
  "/2011/08/07/an-activerecord-plugin-to-prevent-destroy-if-associations-are-present/"
]
+++
I’ve just released a [small plugin](https://github.com/Florent2/prevent_destroy_if_any) that adds ActiveRecord models a way to prevent destroy if specified `has_many`, `has_one` and/or `belongs_to` associations are present. This is achieved by adding a before_destroy callback that aborts the destroy and adds a base error on the instance when detecting associations.

It’s inspired from [this stackoverflow answer](http://stackoverflow.com/questions/4054112/how-do-i-prevent-deletion-of-parent-if-it-has-child-records/4054170#4054170) and was created to factorize this solution when you need it for various models in an application.

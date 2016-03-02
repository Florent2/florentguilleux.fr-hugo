+++
date = "2012-02-21T01:00:00+01:00"
draft = false
title = "Easier Usage of attr_accessible"
slug = "easier-usage-of-attr_accessible"
aliases = [
	"easier-usage-of-attr_accessible",
  "2012/02/21/easier-usage-of-attr_accessible/"
]
+++
[`attr_accessible` is the recommended method](http://guides.rubyonrails.org/security.html#mass-assignment) to protect your attributes from mass assignments in Rails. It lists for a model the attributes that can be mass assigned, other attributes will be protected. But it is tedious to build and update this list of accessible attributes while developing an application.

Fortunately there are two mechanisms which facilitate the usage of `attr_accessible`.

First you can configure your application to create an empty whitelist of attributes available for mass-assignment for all the models. This forces you to explicitly list the accessible attributes of each model, without the risk of forgetting a model or an attribute. To do this just add `config.active_record.whitelist_attributes = true` in the configuration `application.rb`.

When a protected attribute is mass assigned, the attribute value is not modified and by default Rails logs a warning. This makes sometimes hard to realize that some attribute isn’t set or modified through a create or update action because it is protected. Since the version 3.2 it is possible to have an exception raised when a protected attribute is mass assigned. Thus it is much easier to detect attributes that should be made accessible. For that just add `config.active_record.mass_assignment_sanitizer = :strict` to your application configuration, or only in `development.rb` and `test.rb` if your prefer.

By combining those two mechanisms it is easy to discover progressively the attributes you want to make accessible while developing your application.

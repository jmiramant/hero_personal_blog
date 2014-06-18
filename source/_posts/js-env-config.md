title: 'JS ENV Config'
date: 2014-06-04 08:55:48
categories: 'code'
tags:
- coffeescript
- backbone.marionette
---

When switching to JS apps from Rails, I missed the ENV declaration that's built into configuration, so I through together this solution: 

``` coffeescript

# RequireJS Declaration
define ["APP"], (APP) ->

# Set an ENV case statment from location.hostname to return the envionment type base on the url
  APP.ENV = (key) ->
    env = switch window.location.hostname
      when "localhost", "127.0.0.1" then "development"
      when "test.example.com" then "qa"
      when "staging.example.com" then "staging"
      when "example.com" then "production"

# Set ENV Keys in json variable
    envKeys =
      development:
        apiKey: <apiKey>
        fbKey: <fbKey>
      qa:
        apiKey: <apiKey>
        fbKey: <fbKey>
      staging:
        apiKey: <apiKey>
        fbKey: <fbKey>
      production:
        apiKey: <apiKey>
        fbKey: <fbKey>

    envKeys[env][key]

#This is for requireJS
  GMA.ENV

App.ENV(<key>)

```
I'd love to see how other people are doing ENV config on single page js apps.
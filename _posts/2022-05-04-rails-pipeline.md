---
layout: post
title: The Rails Asset Pipeline (Rails v5.2.2)
subtitle: The subtitlties of getting the pipeline to work in Rails 5 with Sprokets < 4.0
gh-repo: openstax/accounts
gh-badge: [star, fork, follow]
tags: [accounts,openstax,rails,assets]
comments: false
---

The Rails pipeline is throughly confusing to newcomers to Rails. My team has taken over the maintence of a rather complex beast that controls our authentication for multiple products. Some use OAuth, some use and SSO cookie we generate. We also use Omni-Auth for social login _and_ password identities.

## The pipeline problem

The asset pipeline automatically pulls in application.js and application.css from the assets directory. Anything else, needs to be manually included in `config/initializers/assets.js`

```ruby
Rails.application.config.assets.precompile += %w(
  admin.js
  admin.css
  profile.js
  profile.css
)
```

Then you'll need a couple settings in your `config/environments/production.rb` file:

{% highlight ruby linenos %}
# Disable Rails's static asset server (Apache or nginx will already do this)
config.serve_static_files = false

# Compress JavaScripts and CSS
config.assets.js_compressor = Uglifier.new(harmony: true)
# config.assets.css_compressor = :sass

# Don't fallback to assets pipeline if a precompiled asset is missed
config.assets.compile = false

# Generate digests for assets URLs
config.assets.digest = true
{% endhighlight %}

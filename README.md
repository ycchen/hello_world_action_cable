# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...

> rails new hello_world_action_cable

> rails g controller page index

* Add route to routes file config/routes.rb

```ruby
	get 'page/index'
	root 'page#index'
```

* Edit views/page/index.html.erb

```html
	<h1>Action Cable Demo</h1>
	<div id="messages"></div>
```

* Install redis uses brew on Mac
> brew install redis

* Start redis server
> brew services start redis

* Add jquery to gem or uses yarn
> brew install yarn
> bin/yarn add jquery

* Add jquery-rails, redis gems in Gemfile
gem 'jquery-rails'
gem 'redis'


* Edit assets/javascripts/application.js
//=require jquery

* Add channel called WebNotifications 
> rails g channel WebNotifications

* Edit app/channels/web_notifications_channel.rb

```ruby
class WebNotificationsChannel < ApplicationCable::Channel
	def subscribed
		stream_from "web_notifications_channel"
	end

	def unsubscribed
	end
end
```

* Edit coffeeScript assets/javascripts/page.coffee

```javascript
App.room = App.cable.subscriptions.create "WebNotificationsChannel", 
	received: (data) ->
		$('#messages').appedn data['message']
```

* we cannot push messages from a Rails console to a Rails server in a different terminal in the development default setup. We have to activate the Redis gem and have Redis running on your system

* Configurate redis yml file config/cable.yml

```yaml
development:
  adapter: redis
  url: redis://localhost:6379/1
  channel_prefix: hello_world_action_cable_production

test:
  adapter: async

production:
  adapter: redis
  url: redis://localhost:6379/1
  channel_prefix: hello_world_action_cable_production

# redis: &redis
#   adapter: redis
#   url: redis://localhost:6379/1

# production: *redis
# development: *redis
# test: *redis
```

* Start the rails server
> rails s

* Open http://localhost:3000/

* Start rails console
> rails c
> ActionCable.server.broadcost 'web_notifications_channel', message: '<p>Hello World</p>'

* You should see the Hello World shows up in your browser
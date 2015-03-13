# Sawyer

Sawyer is an experimental secret user agent built on top of
[Faraday][faraday].

[faraday]: https://github.com/lostisland/faraday

![](http://techno-weenie.net/sawyer/images/faraday.jpeg)

Think of Faraday as the nerdy scientist behind an HTTP API.  Sure, he
knows the technical details of how to communicate with an application.
But he also gets overly obsessive about alternate timelines to be of
much use.

![](http://techno-weenie.net/sawyer/images/sawyer.jpeg)

Sawyer knows what needs to be done.  He gets in there, assesses the
situation, and figures out the next action.

##Usage

One particularlly valuable feature of Sawyer is the support for Hypermedia link relations. The user don't have to care about the URL in the response and pass it to next request by hands. Instead, Sawyer's `Relation` objects support dictionary-like operations, HTTP verbs and even handle URI templates.

Here is an example from [example/client.rb](/example/client.rb).

```ruby
agent    = Sawyer::Agent.new(endpoint) do |http|
  http.headers['content-type'] = 'application/json'
end
puts agent.inspect
puts

root = agent.start
puts root.inspect

puts "LISTING USERS"
users_rel = root.data.rels[:users]

puts users_rel.inspect
puts

users_res = users_rel.get
puts users_res.inspect

users = users_res.data

users.each do |user|
  puts "#{user.login} favorites:"

  fav_res = user.rels[:favorites].get

  fav_res.data.each do |sushi|
    puts "- #{sushi.inspect})"
  end
  puts
end

puts "CREATING USER"
create_user_rel = root.data.rels[:users]

puts create_user_rel.inspect

created = create_user_rel.post(:login => 'booya')
puts created.inspect
puts

puts "ADD A FAVORITE"
created_user = created.data
create_fav_res = created_user.rels[:favorites].post nil, :query => {:id => 1}
puts create_fav_res.inspect

```

##TODO
* ~~Expand README.~~

##Contributing
Read [CONTRIBUTING.md](CONTRIBUTING.md) to learn how to contribute.


##License

See the [LICENSE](LICENSE.md) file for license rights and limitations.

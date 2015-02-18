---
title: JavaScript ORMS suck
tags: [JavaScript]
---

Consider this scenario: there's a strange bug in production's data that you need to inspect. In Rails, you can tell `rails console`: "show me all project IDs that are active and made by users that never logged in" like so:

```ruby
pry(main)> User.where(last_signed_in: null)
  .find_each.map { |u| u.projects.active }
  .flatten.compact.pluck :id
```

This may not be the most efficient way to do it, but some situations are urgent and will call for one-offs like these.

In Node.js, you'll probably do something like this:

```js
> User.where({ last_signed_in: null })
  .fetch().then(function (users) {
    return users.map(function(u){
      return u.projects.active.fetch(); })})
  .then(function(projects) {
    return projects.map(function(p) {
      return p.id; }); });
```

Ah, granted that promises and ES6 (fat arrows) would make this better, but still...
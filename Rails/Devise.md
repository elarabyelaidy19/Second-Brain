- add devise gem 
- run `rails g devise:install` 
- configure flash messages at application layout
```html 
<p class="notice"><%= notice %></p>
<p class="alert"><%= alert %></p> 
```
- create devise user `rails g devise user` and migrate 
- create devise views `rails g devise:views`
## Create rails app with bootstrap 
- `rails new linktre --database=postgresql --skip-git -j esbuild --css bootstrap`  
- `yarn add sass` 
- `npm set-script build:css "sass ./app/assets/stylesheets/application.sass.scss ./app/assets/builds/application.css --no-source-map"` 
- `yarn build:css` 
- add `bootstrap, sassc-rails` gems 

## Annotate 
- `annotate --models --exclude fixtures` 


## Devise  
- 


## Asset pipeline  
- configure the CSS style file at manifiest.js `//= link users.css` 


## Clear Cache 
- if there is a repeated error try to clear cache `rails tmp:cache:clear`   


## Rails Generators 

this generator used to generate service objects using rails generators 

- Template for the generator 
```ruby 
# lib/generators/templates/service.rb.erb

class <%= class_name %>Service
  def self.all_<%= plural_name %>
    <%= class_name %>.all
  end

  def self.find_<%= singular_name %>(id)
    <%= class_name %>.find(id)
  end 
end
```

- the actual genertator 
```ruby 
class ServiceGenerator < Rails::Generators::NamedBase
  source_root File.expand_path("templates", __dir__)

  def create_service_file
    template 'service.rb.erb', File.join('app/services', class_path, "#{file_name}_service.rb")
  end

end
```
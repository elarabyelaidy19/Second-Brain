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
- 
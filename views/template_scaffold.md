# Quick script to get the template files for scaffold new project

# Intro
Once you have your design down, it is great to use custom scaffold views so your scaffolds will get created with most of the layout you already need.

This is just a quick way to get the templates from the railties gem.

Depending on your bundler version, it might be `bundle show railties`

I like to disable the scaffold css as you won't be using that.

For even more custom setup, you can copy the controller scaffold.


## Code

Run this in your terminal
```bash
# get the path for the rails templates
bundle info railties --path
# I like to store it in a variable
TPATH=`bundle info railties --path`
# create the folder in your project
mkdir -p lib/templates/erb/scaffold
cp $TPATH/lib/rails/generators/erb/scaffold/templates/* lib/templates/erb/scaffold/

# add the scaffold controller if you want to customize that
mkdir -p lib/templates/rails/scaffold_controller/
cp $TPATH/lib/rails/generators/rails/scaffold_controller/templates/controller.rb.tt lib/templates/rails/scaffold_controller/
```

Disable the scaffold css from being generated.
```
# config/initializers/disable_scaffold_css.rb
Rails.application.config.generators do |g|
  g.scaffold_stylesheet false
end
```

Now add your custom view code and controller tweaks.

## Further Tips

I often include duplicate code in my templates if I am using different types of views for different models. This way I can delete the view I am not using and still keep the performance boost.

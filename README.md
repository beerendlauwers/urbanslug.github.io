HTML5 Muffin chocchip steps
============================

Run bundle install

	bundle update --system
It installs necessary gems into $GEM_HOME
If you wish to keep your gems in that same directory use `bundle install --path ./vendor/bundle` This did not go well for me.
You can also bundle install to a path such as `bundle install --path ~/.gem/ruby/2.1.0/`
Run jekyll

	jekyll
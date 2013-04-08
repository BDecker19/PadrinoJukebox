# Padrino Jukebox

We're going to write a little app that provides a jukebox service. We're going to draw on what we know about MVC and build on our Sinatra knowledge to create a fuller MVC app in padrino.

# INSTALL

* Sign up for SoundCloud (if you've not done so already)
* Create a new app with soundcloud: http://soundcloud.com/you/apps/new. Make a note of what ClientID you have.
* Fork this repo as normal, git clone your own copy.
* cd into your project folder
* Install the padrino gem: 

    gem install padrino
* Generate a skeleton MVC app with the following commands:

    padrino g project jukebox -d activerecord -e erb -t shoulda .
* Add the following gems to your gemfile:
  * soundcloud
  * json
  * run ```bundle```

* Generate a controller and model:

    padrino g controller Tracks get:play get:index post:search
    padrino g model Track title:string track_url:string soundcloud_id:integer permalink_url:string artwork_url:string description:string duration:integer genre:string bpm:integer comment_count:integer download_count:integer favouritings_count:integer

* Create a database suitable for the model:

    padrino rake ar:create:all
    padrino rake ar:migrate

* Run padrino server

    padrino s

* Run padrino tests

    padrino rake

# TODO

* Add a class method to the Track model, which performs a search for tracks with a given name and returns an array of Track objects. Use your own client ID. 
* Add 3 views to your application: app/views/tracks/index.erb, app/views/tracks/search.erb and app/views/tracks/play.erb.
* Rig up your views to the controller, so that the index action shows all tracks, the search action shows a form, which when submitted searches for all tracks, and rig up the play button so that a player is shown when you click on the tracks's title. Use code along these lines for the player:

    Controller:
    @track = Track.find(params[:id])
    client = Soundcloud.new(:client_id => 'YOUR_CLIENT_ID')
    @sound_cloud_widget = client.get('/oembed', :url => @track.player_url)['html']

    View:
    <%= @sound_cloud_widget.html_safe %>

* Update your application so that search results are cached to a local database.
* Update your application to display the tracks artwork in the list view. Pretty it up using CSS
* Update your application so that it uses postgresSQL in production and push to heroku
* Update your application so that you can search for titles within given genres.
* If you've got this far, see if you can get pagination to work, read the API docs and see how you might page through results
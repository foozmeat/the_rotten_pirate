# The Rotten Pirate?

 - Slurps rotten tomatoes new dvd releases feeds
 - Performs configurable filters on it 
   - e.g. Only certified fresh movies
   - Only movies whose tomatoemeter is greater than 90
   - Movies not already downloaded before (through sqlite database)
 - We then pass along the movies to download to the "Torrent API" gem.
   - The gem looks at comments left by users for ratings of audio and video
   - Performs an average of all ratings given
   - Selects the highest rating (with a reasonable amount of seeds, and at least 5 votes) # This is all subject to change.

## Setup

Clone the source and install proper gems

	git clone git@github.com:hjhart/the_rotten_pirate.git
	cd the_rotten_pirate
	
At this point you'll get prompted by RVM to trust this new .rvmrc file. If you don't have p290 you're welcome to remove and create your own .rvmrc file. Let me know if you get it tested in other rubies.

Now, after the proepr ruby has been picked and you've got a clean gemset

You may need to install bundler (I prefer bundler 1.1 right now)

	gem install bundler --pre
	bundle

Run this command to initialize your sqlite database

	rake init_db
	
## Configure stuff!

Important configurations in config.yml

	download\_directory: Set your download_directory to a directory you have monitored by your torrent application. 
	filter\_out\_less\_than\_percentage: Change this to whatever percent you can handle watching.

Most other stuff is already configured and has been attempted to be optimized.

Edit the\_rotten\_pirate/config.yml

	filter_out_less_than_percentage: 90 # This will reject all movies less than 90%
	filter_out_non_certified: 1 # This will reject movies that aren't certified 'fresh'
	filter_out_already_downloaded: 1 # This will reject movies that have already been downloaded. Don't recommend to turn this off.
	comments: 
	    analyze: true # Turn this off for faster querying.
	    quality: high # or low
	    results_to_analyze: 5 # Crank this up if you want to query more results comments. Maximum of 50 right now.
	download_directory: tmp/torrents # can be relative to the current directory (as seen), or it can be from the home directory (e.g. ~/Torrents)

## Run the process!

	rake execute
	
You should see a bunch of output. If you don't, run the smallish test suite and see if it's all passing.

## Run tests

	rspec spec

So far this has been tested on ruby-1.9.2-p290 on OSX 10.6
	
	
## Play with gem

Thanks to echoe, you can get a irb session loaded with the classes loaded inside of it.

	rake console
	TheRottenPirate.execute
	
## Process

Rotten Tomatoes -> The Rotten Pirate -> Download torrents to a directory -> Torrent program watching directory begins download.


## Potential TODOS

Don't bother taking the time requesting comments from the pirate bay if there is less than (some configurable amount) number of seeds
Make sure if there are no ratings that first seeded wins (pretty sure this is already happening)
Allow for import of top movies?
# Jekyll-Bootstrap

The quickest way to start and publish your Jekyll powered blog. 100% compatible with GitHub pages

## Usage

For all usage and documentation please see: <http://jekyllbootstrap.com>

## Installation on Windows

My setup here is Win 7 32-bit.
Requirement:
  * Make sure you have installed python and added it in your environmental path.
  * Install Ruby, here comes the link.([Download](http://rubyinstaller.org/downloads/))   
     **Note:** Ruby 1.9.3 is recommended (For example, I install it in `D:/ruby`)
  * Also download Development Kit from the link above.

1. First go into the directory where you installed the ruby. Mine is `E:\cloud\MLTeam`.
   And then run the following command:   
      `ruby dk.rb init`   
   Then you can see the `config.yml` file.

2. Open `config.yml` and modify `C:/Ruby193` to your ruby installation path. (e.g. mine is `D:/Ruby193`)

3. Then run this command to install to devkit.     
	`ruby dk.rb install`   
 	**Note:** if the system says you should install RubyGems and you realized that you have installed it. Then you can run the following command   
		`gem install rubygems-update`    
		`update_rubygems`   
	After that, you can run `ruby dk.rb install` again.

4. Then you need to install jekyll now.   
	`gem install jekyll --version "=1.4.2"`

5. Because I didn't install Pygments in this guide. I assume you also don't need to use it. But if you want to install it. Just follow Step 7 and 8 [here](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html).

6. Start Jekyll    
	`jekyll new myblog`   
	`cd myblog`    
	`jekyll serve`   
   Then check [http://localhost:4000](http://localhost:4000)

## Version

0.3.0 - stable and versioned using [semantic versioning](http://semver.org/).

**NOTE:** 0.3.0 introduces a new theme which is not backwards compatible in the sense it won't _look_ like the old version.
However, the actual API has not changed at all.
You might want to run 0.3.0 in a branch to make sure you are ok with the theme design changes.

## Contributing


To contribute to the framework please make sure to checkout your branch based on `jb-development`!!
This is very important as it allows me to accept your pull request without having to publish a public version release.

Small, atomic Features, bugs, etc.
Use the `jb-development` branch but note it will likely change fast as pull requests are accepted.
Please rebase as often as possible when working.
Work on small, atomic features/bugs to avoid upstream commits affecting/breaking your development work.

For Big Features or major API extensions/edits:
This is the one case where I'll accept pull-requests based off the master branch.
This allows you to work in isolation but it means I'll have to manually merge your work into the next public release.
Translation : it might take a bit longer so please be patient! (but sincerely thank you).

**Jekyll-Bootstrap Documentation Website.**

The documentation website at <http://jekyllbootstrap.com> is maintained at https://github.com/plusjade/jekyllbootstrap.com


## License

[MIT](http://opensource.org/licenses/MIT)

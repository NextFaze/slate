<p align="center" style="background-color: black;"><img src="http://xped-corporation.github.io/xerts-docs/images/logo.png" alt="Xped" width="340"></p>

# Xped Xerts API Docs

Xped’s Xerts is digital coupon transfer and trigger technology developed and patented by Xped.  

Leveraging the dependence on smartphones in consumer’s everyday lives Xerts provides a unique way for advertisers and retailers to promote to their customers.

It’s hard to walk through a shopping mall or retail space without being exposed to some form of advertising. 
Each advertisement is competing for attention and trying to trigger an action; which is most commonly to make a purchase now or in the future.


The key objective for the advertiser is to insert a memory into the consumer’s mind that will be triggered at a later point in time, prompting them into action.  Xerts does just that!


## Features

* *Xerts Technology Platform*

* *Web Management Interface*
  * Amazon Web Services
  * Simple user interface
  * On demand reporting services
  * Software as a Service  
  
* *Highly Configurable*
  * Many coupons can be configured
  * Customer supplied images
  * Assign coupons to 1 or many sites
  * Configurable duration
  * Assign number of coupons to be issued
  * Track success of coupons per offer, per site or per vendor

* *Triggers can be set by:*
  * Date & Time
  * Geographic Location
  * Any Combination of these

* *Coupons*
  * Assigned directly to web browser on mobile phone
  * Stored in Xped App



# Building API Docs

These API Docs are built with [Slate](https://github.com/lord/slate)

## Getting Started with Slate

### Prerequisites

You're going to need:

 - **Linux or OS X** — Windows may work, but is unsupported.
 - **Ruby, version 1.9.3 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

1. Fork this repository on Github.
2. Clone this repository with `git clone _URL_`
3. `cd slate`
4. Initialize and start Slate. 

You can either do this locally, 
```shell
# either run this to run locally
bundle install
bundle exec middleman server
```
or with Vagrant:
```shell
# OR run this to run with vagrant
vagrant up
```

You can now see the docs at http://localhost:4567. Whoa! That was fast!

Now that Slate is all set up your machine, you'll probably want to learn more about [editing Slate markdown](https://github.com/tripit/slate/wiki/Markdown-Syntax).

### Publishing Your Docs to Github Pages

Publishing your API documentation couldn't be more simple.

 1. Make sure your `origin` is a Slate fork in your own account, not our original repo.
 1. Commit your changes to the markdown source: `git commit -a -m "Update index.md"`
 2. Push the *markdown source* changes to Github: `git push`
 3. Run `./deploy.sh`

Done! Your changes should now be live.

More at "[how to publish your docs](https://github.com/tripit/slate/wiki/Deploying-Slate)".

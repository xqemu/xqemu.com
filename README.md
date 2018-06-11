[![](https://travis-ci.org/xqemu/xqemu.com.svg?branch=master)](https://travis-ci.org/xqemu/xqemu.com)

This repository contains the contents of the [XQEMU project](http://github.com/xqemu/xqemu) website, [xqemu.com](http://xqemu.com). You are welcome to improve
the site by submitting a pull request!

How to contribute
-----------------
Clone the repo, make/test your changes, then file a pull request! Once approved,
the changes will automatically be propagated to the website via the CI system.

Testing your changes
--------------------
The website is built using [mkdocs](https://www.mkdocs.org/). You can build the
entire website and host it locally for testing your changes before submitting a
pull request!

Start by using [Docker](https://www.docker.com/) to pull down the Material theme
Docker image:

	docker pull squidfunk/mkdocs-material

Then in the root of the repository run:

	docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material

This will start the mkdocs daemon to monitor for local changes to any of the files,
automatically rebuild the site when a change is detected, and host all of it 
locally at [http://127.0.0.1:8000](http://127.0.0.1:8000).

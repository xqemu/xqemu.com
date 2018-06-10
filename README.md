This repository contains the contents of xqemu.com. You are welcome to improve
the site by submitting a pull request!

How to contribute
-----------------
Clone the repo, make/test your changes, then file a pull request! Once approved,
the changes will automatically be propagated to the website via the CI system.

Testing your changes
--------------------
The website is built using mkdocs. You can build the website and host it locally
for testing your changes before submitting a pull request!

Use Docker to pull down the Material theme Docker image:

	docker pull squidfunk/mkdocs-material

Then in the root of the repository run:

	docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material

This will start the mkdocs daemon to monitor for local changes, automatically
rebuild the site, and host it at [http://127.0.0.1:8000](http://127.0.0.1:8000).
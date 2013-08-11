Heroku buildpack: Py+Geo
========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps, powered by [pip](http://www.pip-installer.org/).

It has been modified from the [Heroku original](https://github.com/heroku/heroku-buildpack-python) to add precompiled binaries for [GEOS](http://trac.osgeo.org/geos/), [Proj.4](http://trac.osgeo.org/proj/) and [GDAL](http://trac.osgeo.org/gdal/).
It will also fetch and convert a single zipped OGR datasource to `/app/datasource.shp` when provided with the environment variable `DATASOURCE_URL`.
Note that [user-env-compile](https://devcenter.heroku.com/articles/labs-user-env-compile) from Heroku Labs must be enabled for this to work:

    heroku apps:create -b https://github.com/migurski/heroku-buildpack-pygeo
    heroku config:set DATASOURCE_URL=https://data.sfgov.org/download/3vyz-qy9p/ZIP
    heroku labs:enable user-env-compile

Usage
-----

Example usage:

    $ ls
    Procfile  requirements.txt  web.py

    $ heroku create --stack cedar --buildpack git://github.com/migurski/heroku-buildpack-pygeo.git

    $ git push heroku master
    ...
    -----> Fetching custom git buildpack... done
    -----> Python app detected
    -----> No runtime.txt provided; assuming python-2.7.4.
    -----> Using Python runtime (python-2.7.4)
    -----> Fetching and installing Proj 4.8.0
           Proj cached in /app/tmp/repo.git/.cache/proj-4.8.0
    -----> Fetching and installing GDAL 1.10.1
           GDAL cached in /app/tmp/repo.git/.cache/gdal-1.10.1
    -----> Copying precompiled libraries from /app/tmp/repo.git/.cache to /app
    -----> Installing dependencies using Pip (1.3.1)
           Downloading/unpacking Flask==0.7.2 (from -r requirements.txt (line 1))
           Downloading/unpacking Werkzeug>=0.6.1 (from Flask==0.7.2->-r requirements.txt (line 1))
           Downloading/unpacking Jinja2>=2.4 (from Flask==0.7.2->-r requirements.txt (line 1))
           Installing collected packages: Flask, Werkzeug, Jinja2
           Successfully installed Flask Werkzeug Jinja2
           Cleaning up...

You can also add it to upcoming builds of an existing application:

    $ heroku config:add BUILDPACK_URL=git://github.com/migurski/heroku-buildpack-pygeo.git

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root. 

It will use Pip to install your dependencies, vendoring a copy of the Python runtime into your slug. 

Specify a Runtime
-----------------

You can also provide arbitrary releases Python with a `runtime.txt` file.

    $ cat runtime.txt
    python-3.3.0
    
Runtime options include:

- python-2.7.4
- python-3.3.1
- pypy-1.9 (experimental)

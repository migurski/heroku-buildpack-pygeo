GEOS notes:
    mkdir -pv $HOME/vendor/geos
    ./configure --prefix=$HOME/vendor/geos
    make && make install
    tar -C $HOME -czvf geos-3.3.8-heroku.tar.gz vendor/geos

    tar -xzf geos-3.3.8-heroku.tar.gz
    env CPPFLAGS=`vendor/geos/bin/geos-config --cflags` LIBRARY_PATH=$LIBRARY_PATH:/app/vendor/geos/lib pip install shapely

Proj notes:
    mkdir -pv $HOME/vendor/proj
    ./configure --prefix=$HOME/vendor/proj
    make && make install
    tar -C $HOME -czvf proj-4.8.0-heroku.tar.gz vendor/proj

GDAL notes:
    mkdir -pv $HOME/vendor/gdal
    env LIBRARY_PATH=/app/vendor/proj/lib:/app/vendor/geos/lib:$LIBRARY_PATH \
        PATH=/app/vendor/geos/bin:$PATH \
        ./configure --prefix=/app/vendor/gdal --with-pcraster=no --with-jasper=no --with-grib=no --with-vfk=no --with-python --with-hide-internal-symbols
    make && make install
    mkdir -pv /app/vendor/gdal/lib/python2.7/site-packages
    cp -rf /app/.heroku/python/lib/python2.7/site-packages/GDAL-1.10.0-py2.7-linux-x86_64.egg/*.* \
           /app/.heroku/python/lib/python2.7/site-packages/GDAL-1.10.0-py2.7-linux-x86_64.egg/osgeo \
           /app/vendor/gdal/lib/python2.7/site-packages/
    tar -C $HOME -czvf gdal-1.10.1-heroku.tar.gz vendor/gdal

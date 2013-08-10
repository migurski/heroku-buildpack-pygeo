Proj.4 and GDAL configuration:

    ./configure --prefix=/app/proj && make -j2 && make install
    ./configure --prefix=/app/gdal --with-static-proj4=/app/proj --with-pcraster=no --with-jasper=no --with-grib=no --with-vfk=no --with-python --with-hide-internal-symbols
    
After `make && make install`, move incorrectly-placed GDAL python libraries to correct */app/gdal/lib/python2.7/site-packages*.

GEOS notes (in progress):
    mkdir -pv $HOME/vendor/geos
    ./configure --prefix=$HOME/vendor/geos
    make && make install
    tar -C $HOME -czvf geos-3.3.8-heroku.tar.gz vendor/geos

    tar -xzf geos-3.3.8-heroku.tar.gz
    env CPPFLAGS=`vendor/geos/bin/geos-config --cflags` LIBRARY_PATH=$LIBRARY_PATH:/app/vendor/geos/lib pip install shapely
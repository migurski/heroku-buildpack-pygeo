Proj.4 and GDAL configuration:

    ./configure --prefix=/app/proj && make -j2 && make install
    ./configure --prefix=/app/gdal --with-static-proj4=/app/proj --with-pcraster=no --with-jasper=no --with-grib=no --with-vfk=no --with-python --with-hide-internal-symbols
    
After `make && make install`, move incorrectly-placed GDAL python libraries to correct */app/gdal/lib/python2.7/site-packages*.

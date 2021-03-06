# Utility to generate TopoJSON maps for Catalonia

This repository provides a utility to generate
[TopoJSON](https://github.com/mbostock/topojson) maps for Catalonia using the
source maps provided by [ICGC](//www.icc.cat/vissir3) (*Base municipal*).

You may find this [article](http://teslabs.com/articles/topojson-catalonia)
useful to better understand what this utility does.

<table>
<tr height="180">
<td>provincies<br><a href="https://cloud.githubusercontent.com/assets/11430385/11307750/f1ca3b1e-8f6e-11e5-9da6-a3d608213063.png"><img src="https://cloud.githubusercontent.com/assets/11430385/11307750/f1ca3b1e-8f6e-11e5-9da6-a3d608213063.png"></a></td>
<td>comarques<br><a href="https://cloud.githubusercontent.com/assets/11430385/11307744/ed041eb0-8f6e-11e5-9da9-9b18e511971f.png"><img src="https://cloud.githubusercontent.com/assets/11430385/11307744/ed041eb0-8f6e-11e5-9da9-9b18e511971f.png"></a></td>
<td>municipis<br><a href="https://cloud.githubusercontent.com/assets/11430385/11307753/f4e45ca8-8f6e-11e5-874d-d7b54c699696.png"><img src="https://cloud.githubusercontent.com/assets/11430385/11307753/f4e45ca8-8f6e-11e5-874d-d7b54c699696.png"></a></td>
</tr>
</table>

**NOTE:** Source maps are produced by ICGC and are subject to [terms of
use](//www.icc.cat/conditions).

## Preliminary requirements

Regarding software, the first requirement is the TopoJSON host tool,
``topojson``, which in turn requires [Node.js](http://nodejs.org/en/). If you
are on OS X, and assuming you use [brew](http://brew.sh/), you can easily get
both by typing:

    $ brew install node
    $ npm install -g topojson

In case of converting from one coordinate system to another you will also need
[GDAL](//www.gdal.org/). It can also be installed using ``brew``:

    $ brew install gdal

## Getting started

To get started, clone this repository:

    git clone https://github.com/teslabs/cat-topojson

Once done, you can generate the maps by simply typing:

    make

The following maps are generated in the `topo/` directory:

* `cat-caps.json`: Location of the capital cities for each municipality
* `cat-municipis.json`: Municipalities polygons
* `cat-comarques.json`: Counties polygons
* `cat-provincies.json`: Provinces polygons
* `cat.json`: Combination of all of the above

Using the default parameters will generate projected maps (see all available
parameters and their defaults below) using the *Base municipal* in 1:50 scale.
You can open any of the examples to verify the generated maps.

## Parameters

### Geographical coordinates

By default projected maps are generated, as ICGC source maps are already
projected. However, you can also generate maps in geographical coordinates by
setting the `GEO` parameter. By default it will generate maps in the WGS-84
coordinate system. You can change this by setting `GEO_COORD` to any of the
supported coordinate systems by ``ogr2ogr``.

### Dimensions

When generating projected maps, three parameters can be used to control the
dimensions of the generated map: `WIDTH` (default to 500 px), `HEIGHT` and
`MARGIN`.

### Simplification and quantization

Simplification can be set using the `SIMPLIFICATION` parameter. It defaults to 2
px<sup>2</sup> for projected maps and to 10<sup>-8</sup> sr for geographic
coordinates.

Quantization can be set using the `QUANTIZATION` parameter. It has no default,
which means that `topojson` default (10<sup>4</sup>) is used.

### Maps version

ICGC maps file names are in the format
`bm[eeee]mv33sh1f{xx}[m]_[aaaammdd]_[c].shp` where:

* `eeee` is the scale (i.e., 50, 250, 1000)
* `m` is the frame coordinate reference (only 1 is available, meaning
  EPSG:25831 - ETRS89 / UTM zone 31N)
* `aaaammdd` is the maps reference date (e.g. 20150501)
* `c` is the distribution revision (e.g. 0)
* `xx` identifies the map (e.g. `mp` is for municipalities polygons)

A couple of parameters can be set to change the maps used: `SCALE` (default to
50) and `DATE` (default to 20150501). Source maps are found inside the `sources`
folder and are subject to terms of use (see `LICENSE` in `sources`).

## Examples

You can verify that the maps have been generated properly by opening any of the
examples found in ``examples``. You will first need to link or copy the
generated `topo` folder containing the maps and launch them using an HTTP
server, e.g.

    cd examples/
    ln -s ../topo .
    python -m SimpleHTTPServer

`example-proj.html` requires projected maps to work properly. As already
said, projected maps are generated by default. Parameters may be set like this:

    make WIDTH=250 MARGIN=10 SIMPLIFICATION=10

`example-geo.html` requires maps in geographical coordinates to work properly.
The simplest way to generate them using defaults is to just set `GEO`:

    make GEO=1


Random3Dcity
============

A basic procedural modelling engine for generating random (synthetic) buildings and other features in CityGML in multiple levels of detail (LOD).

![Overview](https://3d.bk.tudelft.nl/biljecki/code/img/R3-interior.png)


Ready to use datasets
---------------------

If you are interested only in the datasets, without the need to run the code, please visit my webpage to download a prepared collection of datasets: [https://3d.bk.tudelft.nl/biljecki/Random3Dcity.html](http://3d.bk.tudelft.nl/biljecki/Random3Dcity.html)

Introduction
---------------------


CityGML data (semantically structured 3D city models) in multiple LODs are not available. This project presents an effort to bridge this gap, by developing a basic procedural modelling engine which generates random CityGML buildings to test software and carry out various experiments. This experimental software prototype was developed as a part of my [PhD research](http://filipbiljecki.com/research/phd.html).

The engine is composed of two modules. The first one is procedural: it randomly generates buildings and their elements according to a set of rules and constraints. The buildings are realised through parameters which are stored in an XML form. The second part of the engine reads this data and constructs CityGML data in multiple LODs.

What Random3Dcity is not
---------------------

Random3Dcity is not intended to compete with ESRI's CityEngine and similar tools, which are focused on taking real-world data and augmenting them for various purposes such as urban planning. This tool is far from that: it simply provides synthetic CityGML data in multiple levels of detail.

Be aware that the tool cannot be used for production purposes, it's merely for research purposes to address the lack of multi-LOD CityGML data sets.


Potential uses
---------------------
The data generated by the engine has been used for a variety of applications where real-world data is not of importance:

+ Source of CityGML data not burdened by topological errors.
+ Testing multi-LOD CityGML software implementations, e.g. database solutions to compress data.
+ Multi-LOD experiments, e.g. determining the optimal LOD for a spatial analysis.
+ Developing 3D use cases, and testing the complexity of each LOD.




Conditions for use and citation
---------------------

This software is free to use. You are kindly requested to acknowledge its use by citing it in a research paper you are writing, reports, and/or other applicable materials. Please cite the following paper ([PDF](http://doi.org/10.5194/isprs-annals-IV-4-W1-51-2016)):

    

    @article{random3dcity,
        Author = {Biljecki, Filip and Ledoux, Hugo and Stoter, Jantien},
        Doi = {10.5194/isprs-annals-IV-4-W1-51-2016},
        Journal = {ISPRS Ann. Photogramm. Remote Sens. Spatial Inf. Sci.},
        Pages = {51--59},
        Pdf = {http://www.isprs-ann-photogramm-remote-sens-spatial-inf-sci.net/IV-4-W1/51/2016/isprs-annals-IV-4-W1-51-2016.pdf},
        Title = {{Generation of multi-LOD 3D city models in CityGML with the procedural modelling engine Random3Dcity}},
        Vol = {IV-4/W1},
        Year = {2016}
    }



Furthermore, I will be very happy to hear if you find this tool useful for your workflow. If you find it useful and/or have suggestions for its improvement, please let me know.

System requirements
---------------------

Python packages:

+ [Numpy](http://docs.scipy.org/doc/numpy/user/install.html) (likely already on your system)
+ [lxml](http://lxml.de)
+ [Fish](https://pypi.python.org/pypi/fish/)

If not on your system please install them, it's easy with `pip`.

### OS and Python version
  
Both Python 2 and Python 3 are supported.

The software has been developed and tested on Mac OS X, and has not been tested with other configurations. Hence, it is possible that some of the functions will not work on Windows.

## Features and technical details

### Roof types and building parts

The engine supports five types of roofs: flat, gabled, hipped, pyramidal, and shed. Further, it supports also building parts such as garages and alcoves.

![Roofs](https://3d.bk.tudelft.nl/biljecki/code/img/R3-roofTypes.png)


### 16 Levels of detail

The engine supports generating data in 16 levels of detail. The following composite render shows an example of four LODs:

![LOD-composite](https://3d.bk.tudelft.nl/biljecki/code/img/R3-LOD-composite.png)


The following image shows the specification of our novel LOD specification ("Delft LODs") according to which the models are generated. This specification will be published in details.

![LOD-refined-specification](https://3d.bk.tudelft.nl/biljecki/code/img/R3-refinedLODs.png)

### Geometric references

Besides the LODs, the engine generates multiple representations according to geometric references within LODs, e.g. an LOD1 with varying heights of the top surface (height at the eaves, at the half of the roof, etc.)

![Geometric references](https://3d.bk.tudelft.nl/biljecki/code/img/R3-LOD1cb.png)


### Solids

Each representation is constructed its corresponding solid.

![Solids](https://3d.bk.tudelft.nl/biljecki/code/img/R3-assemblingSolid.png)


### Vegetation and streets

An experimental feature is the generation of vegetation and streets.

![Other-features](https://3d.bk.tudelft.nl/biljecki/code/img/R3-roads.png)


### Interior of buildings

A basic interior of buildings in three LODs may be generated: see the header in the attachment. This is based on another paper from my group that deals with the refinement of the level of detail concept for interior features. Besides the solids for each floor, a 2D representation per each storey, and a solid for the whole building (offset from the walls) may be generated.

Documented uses
---------------------

Besides my [PhD](http://filipbiljecki.com/research/phd.html) in which I did a lot of experiments and benchmarking with CityGML data, the engine has been used for testing validation and repair software, and other purposes such as error propagation. For the full showcase visit the [data page](http://3d.bk.tudelft.nl/biljecki/Random3Dcity.html).


Usage and options
---------------------

### Randomising the city

To generate buildings run

```
python randomiseCity.py -o /path/to/the/building/file.xml -n 4000
```

where `n` is the number of buildings to be generated. If you don't specify the number of buildings, by default the program will generate 1000 buildings. Please use absolute paths.

To realise these buildings as a 3D city model in CityGML in multiple levels of detail run:

```
python generateCityGML.py -i /path/to/the/building/file.xml -o /path/to/CityGML/directory/
```

Don't forget to put the `/` at the end of the directory. Again, please use absolute paths.

### Rotation

If you want to have the buildings rotated randomly, in both commands toggle the flag `-r 1`.

### Building parts

Building parts (garages, warehouses, alcoves) are generated with the flag `-p 1`.

### Vegetation and street network

Vegetation and street network are generated with the flags `-v 1` and `-s 1`, respectively.

### Solids and geometric references

`generateCityGML.py` generates solids with the option `-ov 1`, and all geometric references with `-gr 1`.

### gml:id according to UUID

It is possible to generate an UUID for each <gml:Polygon> with the option `-id 1`.

### Coordinate system

By default, buildings are placed in a local coordinate system. If you run the building randomiser with the option `-c 1`, the buildings will be placed in the Dutch coordinate system (RD new), somewhere in the Nordoostpolder in the Netherlands. You can easily customise this in the code. You don't have to toggle `-c 1` in the second script (`generateCityGML.py`).

### Reporting of the progress

When running `generateCityGML.py` it is possible to get the report of the progress of the script with `-rp 1`.
This option is turned on by default. However, there have been reports of bugs when using Python3, so the option disables itself automatically if the underlying dependency cannot be loaded. Should you run into problems, disable it with `-rp 0`.



Performance
---------------------

The speed mainly depends on the invoked options. With all the options the engine generates around 100 buildings per minute. The computational complexity is not strictly linear, and a high number of buildings (>20000) will likely eat all of your RAM making the process slower. If you need to generate more than tens of thousands of buildings, consider not generating all LODs and representations (e.g. solids).

Known issues and limitations
---------------------

### CityGML issues

+ The output files are stored in CityGML 2.0. Output in the legacy versions 0.4 and 1.0 is not available.

### Running issues

The `generateCityGML.py` program has been known to crash in two cases:

+ It runs out of memory if too many buildings are attempted to be generated in CityGML. Reduce the number of buildings and/or their variants (e.g. disable the generation of solids).
+ Rarely it crashes when it encounters a very peculiar building to be generated (despite the established rules and constraints), some weird-looking buildings can still occur). This does not happen often, and when it does just generate a new set of buildings with `randomiseCity.py`.

### General limitations

+ While this software prototype generates virtually unlimited CityGML data for free from scratch and in multiple LODs, which can be used for a multitude of purposes, this is not a commercial procedural modelling engine that can be considered as a complete standalone solution in many production workflows. 


Special datasets
---------------------

I have prepared a number of intentionally impaired datasets suited for different types of experiments, such as overlapping buildings and simulated geometric errors. For the full list visit the [data page](https://3d.bk.tudelft.nl/biljecki/Random3Dcity.html)

![Intentional errors](https://3d.bk.tudelft.nl/biljecki/code/img/R3-overlapping.png) 


Contact for questions and feedback
---------------------

Feel free to contact me should you have questions:

Filip Biljecki

[3D geoinformation research group](https://3d.bk.tudelft.nl/)

Delft University of Technology

email: fbiljecki at gmail dot com

[Personal webpage](http://filipbiljecki.com/)


Acknowledgments
---------------------

+ This research is supported by the Dutch Technology Foundation STW, which is part of the Netherlands Organisation for Scientific Research (NWO), and which is partly funded by the Ministry of Economic Affairs (project code: 11300).

+ People who gave suggestions and reported errors, especially Mickaël Brasebin (IGN France) for the suggestions to make the code compatible with Python3.

<!-- README.md is generated from README.Rmd. Please edit that file -->

# Hydrofabric Subsetter

## CLI Option

For those interested in using the NOAA NextGen fabric as is, we have
provided a Go-based CLI
[here](https://github.com/LynkerIntel/hfsubset/releases)

This utility has the following syntax:

``` bash
hfsubset - Hydrofabric Subsetter

Usage:
  hfsubset [OPTIONS] identifiers...
  hfsubset (-h | --help)

Examples:
  hfsubset -l divides,nexus        \
           -o ./divides_nexus.gpkg \
           -r "pre-release"        \
           -t hl                   \
           "Gages-06752260"

  hfsubset -o ./poudre.gpkg -t hl "Gages-06752260"

Options:
  -l string
        Comma-delimited list of layers to subset.
        Either "all" or "core", or one or more of:
            "divides", "nexus", "flowpaths", "flowpath_attributes",
            "network", "hydrolocations", "lakes", "reference_flowline",
            "reference_catchment", "reference_flowpaths", "reference_divides" (default "core")
  -o string
        Output file name (default "hydrofabric.gpkg")
  -quiet
        Disable progress bar
  -r string
        Hydrofabric version (default "pre-release")
  -t string
        One of: "hf", "hl", "comid", "loc", or "nldi" (default "hf")
```

## NextGen Needs GeoJSON

While GPKG support is the end goal for NextGen, it current requires
GeoJSON and CSV inputs.

Fortunately, `ogr2ogr` provides easy ways to extract these sub
layer/formats from the GPKG file.

Here is a full-stop example of extracting a subset for a hydrolocation,
using the CLI, and generating the needed files for NextGen

``` bash
mkdir poudre
cd poudre

hfsubset -l divides,nexus,flowpath_attributes -o ./poudre-subset.gpkg -r "pre-release" -t hl "Gages-06752260"

ogr2ogr -f GeoJSON catchments.geojson poudre-subset.gpkg -nln divides  
ogr2ogr -f GeoJSON nexus.geojson poudre-subset.gpkg -nln nexus
ogr2ogr -f CSV flowpath_attributes.csv poudre-subset.gpkg -nln flowpath_attributes

ls poudre
```

## License

`hfsubset` is distributed under [GNU General Public License v3.0 or later](LICENSE.md)

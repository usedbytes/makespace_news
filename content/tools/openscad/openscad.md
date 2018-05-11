# OpenSCAD

Author: Brian Starkey
Authorship Date: 11th May 2018
URL: TODO
Tags: CAD, 3D, printing, laser, software

OpenSCAD is a 3D CAD package, where designs are constructed with code, rather
than drawn. This will probably strike most people as a terrible idea (and in
some respects they'd be right), but at the same time it allows for some
extremely constructs which would be difficult to achieve in other ways.

TODO: There's similar packages. Rhino maybe?

OpenSCAD is Free (as in beer and speech), and available for Windows, Mac OS and
Linux, with various other ports (such as TODO: Javascript).

## The basics

Below is the most basic OpenSCAD program, which draws a single cube, 15 mm on a
side:

```
cube(15);
```

The transformation primitives can be used to manipulate the object:

```
translate([0, 0, 15]) rotate([0, -45, 0]) cube(15);
```

And boolean operations are available, to combine or subtract different objects:

```
difference() {
	translate([0, 0, 15]) rotate([0, -45, 0]) cube(15);
	cylinder(r=5, h=100);
}
```

There are comprehensive tutorials around on the 'net - and also a series which
I created for a course I ran in Makespace in TODO: XXX. If there's interest
I'd be happy to run through the content again.

## Libraries

Being source-code based, OpenSCAD is ideally suited to the "open source"
culture, with any number of libraries and modules available online. For example
the TODO: XXX module provides primitives for creating Lego(r)-compatible parts:

```
use <lego.scad>

brick(XXX);
```

Or perhaps you need a TODO: XXX gear??

## Two dimensions

As well as 3D, OpenSCAD has a 2D subsystem, and allows extrusion from 2D to 3D,
and projection or slicing from 3D to 2D.

TODO: Projection (from OpenSCAD-tut)

Combined with a capable 2D CAD package (I use LibreCAD), this allows you to
draw a 2D shape, import it in to OpenSCAD, and extrude it into a 3D shape (with
any number of variations, enhancements and adjustments). Not unique to OpenSCAD,
but certainly useful:

TODO: panel_bracket.dxf


## Getting procedural

Where OpenSCAD really shines, is when you want to create something which is
easy to describe mathematically, but devilishly difficult to create in a
"normal" CAD program.

For instance, fractals or other structures which are described recursively
are a natural fit for OpenSCAD:

```
// Written in 2015 by Torsten Paul <Torsten.Paul@gmx.de>
// From the OpenSCAD examples
identity = [ [ 1, 0, 0, 0 ], [ 0, 1, 0, 0 ], [ 0, 0, 1, 0 ], [ 0, 0, 0, 1 ] ];
function mt(x, y) = [ [ 1, 0, 0, x ], [ 0, 1, 0, y ], [ 0, 0, 1, 0 ], [ 0, 0, 0, 1 ] ];
function mr(a) = [ [ cos(a), -sin(a), 0, 0 ], [ sin(a), cos(a), 0, 0 ], [ 0, 0, 1, 0 ], [ 0, 0, 0, 1 ] ];

random = rands(0, 1, 1000, 18);
function rnd(s, e, r) = random[r % 1000] * (e - s) + s;

module tree(length = 100, thickness = 5, count = 10, m = identity, r = 1) {
    color([0, 1 - (0.8 / 10 * count), 0])
        multmatrix(m)
            square([thickness, length]);

    if (count > 0) {
        tree(rnd(0.6, 0.8, r) * length, 0.8 * thickness, count - 1, m * mt(0, length) * mr(rnd(20, 35, r + 1)), 8 * r);
        tree(rnd(0.6, 0.8, r + 1) * length, 0.8 * thickness, count - 1, m * mt(0, length) * mr(-rnd(20, 35, r + 3)), 8 * r + 4);
    }
}

tree();
```

Or perhaps you need an egg holder which arranges them in a perfect sinusoid:

```
difference() {
	cube([240, 240, 20]);
	for (i = [0:6]) {
		translate([30 + i * 30, 30 + 180 * sin(i * 30), 30]) sphere(20);   
	}
}
```

TODO: sweep.

## Summary

While not suited to everyone, and not always the easiest tool to work with,
OpenSCAD is a powerful and versatile application, which it's worth considering
adding to your toolbox.

Rays
####

A ray is a half-line: a starting point (its *origin*) and a *direction*. The point reached after travelling a distance ``t`` along the direction is ``origin + t * direction``.

.. figure:: ../../_static/svg/ray.drawio.svg

   A ray, defined by an origin and a direction.

Define a ``ray`` class with:

- Member data: a ``point3`` (its origin) and a ``vec3`` (its direction).
- Constructors: a default one and one taking a ``point3`` (origin) and a ``vec3`` (direction).
- Member functions: accessors for both, and a function to get the position along the ray:

.. code-block:: cpp

    point3 at(double t) const;

``at(0)`` should return the origin, ``at(1)`` a point one full "direction vector" away from the origin — the direction does not need to be a unit vector.

.. note:: ``ray.hpp`` will need ``#include "vec3.hpp"`` since it uses ``point3`` and ``vec3``. Remember to add ``#include "ray.hpp"`` to ``src/main.cpp``.

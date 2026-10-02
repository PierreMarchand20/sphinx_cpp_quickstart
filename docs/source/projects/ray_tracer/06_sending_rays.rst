Sending rays
############

Instead of a flat color, compute a color that depends on the ray's direction: blend white and sky-blue based on how much the ray points "up".

.. code-block:: cpp

    color ray_color(const ray& r);

Where the returned color blends white and sky-blue:

.. code-block:: cpp

    (1 - a) * color(1.0, 1.0, 1.0) + a * color(0.5, 0.7, 1.0)

where ``a = 0.5 * (unit_vector(r.direction()).y() + 1.0)``. Remark that ``a`` goes to 0 when looking straight down, to 1 when looking straight up.

Replace the loop body's flat color with a call to ``ray_color(r)``.

.. figure:: ../../_static/img/empty_scene.png

   ``aspect_ratio=16./9.``, ``image_width=400``, ``focal_length=1.``, ``viewport_height=2``

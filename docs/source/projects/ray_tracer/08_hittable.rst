What is an obstacle?
#####################

.. important:: Required reading: :ref:`object_oriented/polymorphism:polymorphism`.

So far, "hitting a sphere" was one hardcoded function. To support any number of different obstacles, define a common interface they all implement.

``hit_record`` carries everything a hit needs to report: where it happened, the surface normal there, and the ray parameter ``t``. Define a ``struct hit_record`` with:

- Data members: ``point3 p``, ``vec3 normal``, ``double t``, ``bool front_face``.
- A member function ``void set_face_normal(const ray& r, const vec3& outward_normal)`` that sets ``front_face`` to whether the ray hit from outside, and stores a normal always pointing *against* the ray (flipping ``outward_normal`` when the ray hit from inside).

``hittable`` is the interface every obstacle must implement — a :ref:`pure virtual member function <object_oriented/polymorphism:pure virtual member functions>`:

.. code-block:: cpp

    class hittable {
    public:
        virtual ~hittable() = default;
        virtual bool hit(const ray& r, double t_min, double t_max, hit_record& rec) const = 0;
    };

``sphere`` implements it, reworking the previous step's ``hit_sphere`` to fill in a ``hit_record`` instead of just returning a distance. Define a ``class sphere : public hittable`` with:

- A constructor taking a ``point3`` (center) and a ``double`` (radius).
- The overridden ``hit`` member function.

``t_min``/``t_max`` restrict which intersections count — useful once there are several obstacles and we only want the *closest* one in front of the camera. If the nearer of the two mathematical roots falls outside that range, try the farther root before giving up.

.. important:: Add a test exercising a ray tangent to the sphere (discriminant exactly 0) and a ray whose origin is *inside* the sphere, not just a ray through the center — these are the cases most likely to silently break the ``(t_min, t_max)`` logic above without causing a compile error.

Project setup
#############

.. important:: Required reading: :ref:`first_cpp_program/compilation:cmake`, :ref:`first_cpp_program/compilation:fetching external dependencies`.

Every step of this project builds on the same CMake project, using `SFML <https://www.sfml-dev.org>`__ to display the rendered image in a window. Create the following structure:

.. code-block:: bash
    :caption: Project structure

    ray_tracer
    ├── CMakeLists.txt
    ├── include
    └── src
        └── main.cpp

.. code-block:: cmake
    :caption: CMakeLists.txt
    :name: ray_tracer_cmakelists
    :linenos:

    cmake_minimum_required(VERSION 3.28)
    project(RayTracer LANGUAGES CXX)

    include(FetchContent)
    FetchContent_Declare(
      SFML
      GIT_REPOSITORY https://github.com/SFML/SFML.git
      GIT_TAG 3.0.1
      GIT_SHALLOW ON
      EXCLUDE_FROM_ALL SYSTEM)
    FetchContent_MakeAvailable(SFML)

    add_executable(ray_tracer src/main.cpp)
    target_compile_features(ray_tracer PRIVATE cxx_std_17)
    target_compile_options(ray_tracer PRIVATE -Wall -Wextra)
    target_include_directories(ray_tracer PRIVATE include)
    target_link_libraries(ray_tracer PRIVATE SFML::Graphics)

.. code-block:: cpp
    :caption: src/main.cpp
    :name: ray_tracer_main_setup

    #include <SFML/Graphics.hpp>
    #include <optional>

    int main() {
        sf::RenderWindow window(sf::VideoMode({400u, 225u}), "Ray Tracer");

        while (window.isOpen()) {
            while (const std::optional event = window.pollEvent()) {
                if (event->is<sf::Event::Closed>()) {
                    window.close();
                }
            }
            window.clear();
            window.display();
        }
    }

.. note::

    Linux users: building SFML 3 from source via ``FetchContent`` needs a few system development packages first (X11, Xrandr, Xcursor, Xi, udev, GL/EGL headers, …) — see `SFML's own CMake getting-started guide <https://www.sfml-dev.org/tutorials/3.0/getting-started/cmake/>`__ for the exact list for your distribution. Also, ``cmake_minimum_required(VERSION 3.28)`` above needs an actual CMake 3.28 or newer: Ubuntu 22.04 ships CMake 3.22, which is too old, while Ubuntu 24.04's packaged CMake (or a manually installed newer one) works fine.

Generate and build the project as in :ref:`first_cpp_program/compilation:cmake`:

.. code-block:: bash

    mkdir build
    cd build
    cmake ../
    make

Equivalently, ``cmake --build build`` run from the project root (not from inside ``build/``) does the same thing as ``cd build && make`` — later steps use this shorter form for brevity when rebuilding after a source change.

Run the resulting executable. You should get an empty black window that closes when you click its close button. This is the starting point for every step that follows: you will only ever *add* code to ``src/main.cpp`` (and later, new header files in ``include``) — ``CMakeLists.txt`` only changes again to add tests, starting with :doc:`03_vec3`.

.. note:: The very first ``cmake`` configuration downloads and compiles SFML, which can take a few minutes. It only needs to do this once.

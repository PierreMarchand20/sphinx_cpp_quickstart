Create an image
################

We first need to display an image from raw RGB data: fill a buffer with a color per pixel, then hand that buffer to SFML to display.

- The buffer is a ``std::vector<std::uint8_t>``, 4 bytes per pixel (red, green, blue, alpha), so its size is ``image_width * image_height * 4`` and the byte index for pixel ``(x, y)`` is ``4 * (y * image_width + x)``.
- Fill it with an arbitrary color first, then try to make something colorful — a simple gradient based on ``x``/``y`` is enough.

Once the buffer is filled, SFML needs a few calls to display it:

.. code-block:: cpp
    :caption: src/main.cpp
    :name: ray_tracer_create_image
    :linenos:

    #include <SFML/Graphics.hpp>
    #include <cstdint>
    #include <optional>
    #include <vector>

    constexpr unsigned int image_width = 400;
    constexpr unsigned int image_height = 225;

    int main() {
        std::vector<std::uint8_t> pixels(std::size_t{image_width} * image_height * 4);

        for (unsigned int y = 0; y < image_height; ++y) {
            for (unsigned int x = 0; x < image_width; ++x) {
                // fill pixels[4 * (y * image_width + x)] (red), + 1 (green),
                // + 2 (blue) and + 3 (alpha, use 255) here
            }
        }

        sf::Texture texture(sf::Vector2u{image_width, image_height});
        texture.update(pixels.data());
        sf::Sprite sprite(texture);

        sf::RenderWindow window(sf::VideoMode({image_width, image_height}), "Ray Tracer");
        while (window.isOpen()) {
            while (const std::optional event = window.pollEvent()) {
                if (event->is<sf::Event::Closed>()) {
                    window.close();
                }
            }
            window.clear();
            window.draw(sprite);
            window.display();
        }
    }

- ``texture.update(pixels.data())`` uploads the whole buffer to the graphics card once; the window keeps displaying it until closed, nothing is recomputed per frame.

Rebuild and run (``cmake --build build`` then run the executable). You should see your chosen gradient or color.

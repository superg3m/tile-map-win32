# tile-map-win32

The point of this repo is the reproduce eps 1 - 40 of Handmade Hero by Casey muratori
The reason i'm doing this is because its difficult to conceptialize offsets sometimes

what I hope to accomplish 

- resultion independent rendering (using meters instead of pixels)

struct Vector2 {
    double x;
    double y;
};

struct Vector3 { 
    union {
        struct {
            double x;
            double y;
            double z;
        };

        struct {
            double r;
            double g;
            double b;
        };
    };
};

struct Vector4 { 
    union {
        struct {
            double x;
            double y;
            double z;
            double w;
        };

        struct {
            double r;
            double g;
            double b;
            double a;
        };
    };
};

struct world_specifications {
    const u32 tile_width_in_pixels; // eventually these should be meters?
    const u32 tile_height_in_pixels;

    const double pixels_to_meters; // 1 pixel is how many meters? what fractional value?

    const u32 tile_map_width;
    const u32 tile_map_height;
}

struct world_position {
    Vector2 absolute_tile;

    Vector2 absolute_pixel_position; // eventually this wil be meters
}

// Using absolute_pixel_position you can derive absolute_tile
Vector2 calculate_absolute_tiles(Vector2 absolute_pixel_position) {
    Vector2 ret = {};
    ret.x = absolute_pixel_position.x / world_specification.tile_width_in_pixels; // integer division
    ret.y = absolute_pixel_position.y / world_specification.tile_width_in_pixels; // integer division
    return ret;
}
// absolute_tile = calculate_absolute_tiles(absolute_pixel_position);

then from absolute tiles you can derive the tile_map index
x_tile_map_index = absolute_tile.x / world_specification.tile_map_width;
y_tile_map_index = absolute_tile.y / world_specification.tile_map_height;

Vector2 calculate_relative_tiles(u32 absolute_tile_x, absolute_tile_y) {
    Vector2 ret = {};
    x_tile_map_index = absolute_tile.x / world_specification.tile_map_width; // integer division
    y_tile_map_index = absolute_tile.y / world_specification.tile_map_height; // integer division

    ret.x = absolute_tile.x - (x_tile_map_index * world_specification.tile_map_width)
    ret.y = absolute_tile.y - (y_tile_map_index * world_specification.tile_map_height)
    return ret;
}

- set up draw_rectangle(x, y, width, height, color); 
- set up u32 tile_value = query_tile_map(x, y); // returns 0 if not valid
- map tile_value to color

- set up a notion of camera
    - set_camera_world_position(world_position new_world_position);
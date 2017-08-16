# ZPL - Tree culler module
[![npm version](https://badge.fury.io/js/zpl_cull.c.svg)](https://badge.fury.io/js/zpl_cull.c)

Fast 2D/3D spatial tree culler. This module takes care of placing tree nodes into the culling zone and splitting branch into subtrees accordingly (based on simple set of conditions).
User can easily query for any list of nodes in a tree and easily retrieve a list of visible nodes user can operate on.

## Usage

```c
#define ZPL_IMPLEMENTATION
#define ZPLM_IMPLEMENTATION
#define ZPLC_IMPLEMENTATION
#include "zpl.h"
#include "zpl_math.h"
#include "zpl_cull.h"

int
main(void) {
    zplc_bounds_t b = {0};

    b.centre = zplm_vec3(0, 0, 0);
    b.half_size = zplm_vec3(100, 100, 100);

    zplc_t root = {0};
    zplc_init(&root, zpl_heap_allocator(), zplc_dim_2d_ev, b, 2);

    zplc_node_t e1 = {0};
    e1.position.x = 20;
    zplc_insert(&root, e1);

    zplc_node_t e2 = {0};
    e2.position.x = 30;
    zplc_insert(&root, e2);

    zplc_node_t e3 = {0};
    e3.position.x = 35;
    zplc_insert(&root, e3);

    zplc_node_t e4 = {0};
    e4.position.x = 12;
    zplc_insert(&root, e4);

    zplc_bounds_t search_bounds = {
        .centre = {0,0,0},
        .half_size = {20,20,20},
    };

    zpl_array_t(zplc_node_t) search_result;
    zpl_array_init(search_result, zpl_heap_allocator());

    zplc_query(&root, search_bounds, search_result);

    isize result = zpl_array_count(search_result);
    ZPL_ASSERT(result == 2);

    return 0;
}
```
# In-Memory Inventory Manipulation
**This mod's performance is untested and might not actually preduce significant improvements**

There are many common mods which perform intensive and repetitive inventory manipulation operations. Mant of these mods perform these operations directly on the inventory objects themselves.

In server environments this can cause sluggish behaviour and high bandwidth usage as every operation performed on an inventory object is sent to the client(s), and network latency can add more than 200ms of delay _per operation_ for regular users. A "craft all" button could theoretically be sending hundreds of inventory updates for the client every time you use it, yet only a handful of stacks are actually modified.

mem_inv resolves this by creating a simple Lua wrapper over the default inventory object which operates entirely server-side, until the operations are complete. For applications where bulk inventory modification is required and the resulting number of modified stacks is less than the number of operations performed on the inventory, this mod should improve the performance of your mod.

## Usage
Usage of this mod is simple and can easily replace standard inventory manipulation.

Before:
```lua
local meta = minetest.get_meta(pos)
local inv = meta:get_inventory()

-- Expensive stuff here
```

After:
```lua
local meta = minetest.get_meta(pos)
local inv = mem_inv.get_mem_inv(meta:get_inventory())

-- Expensive stuff here

-- Send changes to client(s)
inv:commit()
```

the mem_inv object features identical features to the regular inventory object, except the changes are stored in memory until `mem_inv:commit()` is run.

If you'd rather not add mem_inv as a dependancy then the mod can be packaged as a separate lua file and the `dofile()` function used to import it at load time.

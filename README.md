# AMLP Library

Standard LPC library for the AMLP Driver, providing the foundation for MUD development with a complete inheritance hierarchy, simulated efuns, and domain structure.

## Features

- **Master Object** - Central control object with security and initialization
- **Standard Inheritance** - Base object, room, item, living, and player classes
- **Simulated Efuns** - Extended LPC functions beyond driver efuns
- **Domain Structure** - Organized world-building framework
- **Security System** - Permission management and access control

## Quick Start

```bash
# Build (run from amlp-library directory)
make all

# Test with driver
cd ../amlp-driver
./build/driver
```

## Project Structure

```
amlp-library/
├── master.lpc              # Master object (required)
├── std/                    # Standard inheritance
│   ├── object.lpc          # Base object for all items
│   ├── room.lpc            # Base room object
│   ├── item.lpc            # Portable items
│   ├── living.lpc          # Living entities
│   └── player.lpc          # Player object
├── secure/                 # Security and system files
│   ├── simul_efun.lpc      # Simulated efuns
│   └── security.lpc        # Access control
├── domains/                # MUD world domains
│   ├── start/              # Starting area
│   └── test/               # Test domain
└── tests/                  # Library tests
```

## Standard Objects

### `/std/object.lpc`
Base object inherited by all in-game items.

**Features:**
- Basic properties (id, short, long)
- Inventory management
- Event handling
- Command processing

### `/std/room.lpc`
Base for all rooms and locations.

**Features:**
- Exits and directions
- Room descriptions
- Inventory tracking
- Entry/exit events

### `/std/item.lpc`
Base for portable objects.

**Features:**
- Weight and value
- Get/drop handling
- Item manipulation

### `/std/living.lpc`
Base for all living creatures.

**Features:**
- Health/stats management
- Combat system hooks
- Movement handling
- Communication

### `/std/player.lpc`
Player character object.

**Features:**
- Login/logout handling
- Save/restore
- Command processing
- Player-specific features

## Simulated Efuns

Located in `/secure/simul_efun.lpc`. Extends driver efuns with additional functionality:

**String Functions:**
- `string capitalize(string str)` - Capitalize first letter
- `string replace(string str, string old, string new)` - String replacement
- `int member(mixed item, mixed* array)` - Array membership test

**Object Functions:**
- `object* all_inventory(object ob)` - Get all objects in inventory
- `object* deep_inventory(object ob)` - Recursive inventory search
- `object present(string id, object where)` - Find object by id

**Path Functions:**
- `string base_name(object ob)` - Get object's base file path
- `string file_name(object ob)` - Get object's full file path

**Utility Functions:**
- `void log_file(string file, string message)` - Server logging
- `string query_ip_name(object player)` - Get player's IP address

## Domain Structure

Domains organize the MUD world into logical areas. Each domain contains:

```
domains/example/
├── rooms/              # Room files
│   ├── entry.lpc
│   └── main.lpc
├── npc/               # Non-player characters
│   ├── guard.lpc
│   └── merchant.lpc
├── obj/               # Domain-specific objects
│   ├── sword.lpc
│   └── key.lpc
└── data/              # Domain data files
```

## Master Object

`/master.lpc` - Central control object loaded at driver startup.

**Responsibilities:**
- Driver initialization
- Security checks
- User authentication
- Error handling
- Resource management

**Key Functions:**
- `void create()` - Initialize driver
- `object connect()` - Handle new connections
- `int valid_read(string path, object caller)` - File read permission
- `int valid_write(string path, object caller)` - File write permission
- `void log_error(string file, string error)` - Error logging

## Security System

Located in `/secure/security.lpc`.

**Features:**
- Role-based access control
- File system permissions
- Command restrictions
- Wizard/admin privileges

**Access Levels:**
- Player - Basic access
- Builder - Object creation
- Wizard - Advanced features
- Admin - Full system access

## Usage Examples

### Creating a Simple Room

```lpc
// domains/start/rooms/void.lpc
inherit "/std/room";

void create() {
    ::create();
    set_short("The Void");
    set_long("You are floating in an endless void. "
             "There is nothing here but darkness.");
    add_exit("east", "/domains/start/rooms/entry");
}
```

### Creating an NPC

```lpc
// domains/start/npc/guard.lpc
inherit "/std/living";

void create() {
    ::create();
    set_name("guard");
    set_short("a burly guard");
    set_long("This guard looks very strong and alert.");
    set_race("human");
    set_level(5);
}
```

### Creating an Item

```lpc
// domains/start/obj/sword.lpc
inherit "/std/item";

void create() {
    ::create();
    set_name("sword");
    set_short("a rusty sword");
    set_long("An old, rusty but still functional sword.");
    set_weight(30);
    set_value(10);
}
```

## Development

### Adding New Standard Objects

1. Create new file in `/std/`
2. Inherit appropriate base class
3. Override standard functions
4. Document public API

### Creating a New Domain

1. Create domain directory in `/domains/`
2. Set up subdirectories (rooms, npc, obj)
3. Create entry room
4. Link from existing rooms

### Testing

```bash
# Run library tests
make test

# Test individual files
cd tests
../amlp-driver/build/driver test_room.lpc
```

## Integration with AMLP Driver

The library is designed to work seamlessly with the AMLP Driver:

- **Automatic Loading** - Master object loaded at startup
- **Simul Efuns** - Available to all objects
- **Security Integration** - Driver calls master object for permissions
- **Error Handling** - Errors logged through master object

### Driver Configuration

In `amlp-driver/config/runtime.conf`:

```conf
mudlib_dir = "../amlp-library"
master_file = "/master.lpc"
simul_efun_file = "/secure/simul_efun.lpc"
```

## Contributing

Contributions welcome for:

- Additional standard objects
- New simulated efuns
- Example domains
- Documentation improvements

## Requirements

- AMLP Driver (compatible version)
- LPC syntax support

## License

MIT License - See LICENSE for details

---

**Status:** Active Development | **Version:** 0.1.0 | **Driver:** AMLP Driver v0.6.0

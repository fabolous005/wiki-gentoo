<!-- source: https://wiki.gentoo.org/wiki/Lua/Porting_notes | group: Gentoo Wiki (Main) | wiki-title: Lua/Porting notes -->
---
title: Lua/Porting notes
url: https://wiki.gentoo.org/wiki/Lua/Porting_notes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-11"
fingerprint: "2fe1fe8981611a8e"
license: CC BY-SA 4.0
---

# Lua/Porting notes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page shows some migration notes between major versions of Lua.

Functions and structures declared as deprecated can be used with USE-flag "deprecated" on specific Lua major release (5.1, 5.2 etc), but next release usually completely remove deprecated code. Avoid using the "deprecated" flag and try to migrate to the new API.

## Lua 5.1

### luaL\_getn / luaL\_setn

Compatibility define in luaconf.h: LUA\_COMPAT\_GETN

Replace `luaL_getn` with `lua_objlen`:

`luaL_setn` completely dropped, is safe to remove it from code.

### lua\_open

Replace `lua_open` with `luaL_newstate`:

### luaL\_openlib

Compatibility define in luaconf.h: LUA\_COMPAT\_OPENLIB

Lua manual says that this function should be replaced with `luaL_register`, which is deprecated in Lua 5.2 too. For universal approach it's better "backport" `luaL_setfuncs` and use it as substitution for `luaL_openlib` and `luaL_register`:

Calls to `luaL_openlib` and `luaL_register` should be changed according to its second argument. If second argument is NULL, migration is simple:

Calls such as `luaL_openlib(L, name, lreg, x)` and `luaL_register(L, name, lreg)` should be carefully rewritten because a global table with the given name will be searched and possibly created. When possible, it should be rewritten to `luaL_setfuncs(L, lreg, 0)`.

## Lua 5.2

### lua\_strlen / lua\_objlen

Replace them with `lua_rawlen`:

### lua\_equal / lua\_lessthan

Replace them with appropriate `lua_compare` calling:

### luaL\_register

Compatibility define in luaconf.h: LUA\_COMPAT\_MODULE

See [#luaL\_openlib](https://wiki.gentoo.org#luaL_openlib)

<!-- source: https://wiki.gentoo.org/wiki/Mpv/autoloadtxt.lua | group: Gentoo Wiki (Main) | wiki-title: Mpv/autoloadtxt.lua -->
---
title: Mpv/autoloadtxt.lua
url: https://wiki.gentoo.org/wiki/Mpv/autoloadtxt.lua
hostname: gentoo.org
sitename: Mpv/autoloadtxt.lua
date: "2017-05-30"
fingerprint: aa0d82da72e9c96a
license: CC BY-SA 4.0
---

# Mpv/autoloadtxt.lua

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

FILE **`autoloadtxt.lua`**

```
-- Copied from https://gist.github.com/wiiaboo/a8f355905aaa2da5f11f5202d9a0d099
-- See https://github.com/mpv-player/mpv/issues/4144 for more info.
-- autoloadtxt.lua
-- autoload .txt subtitles respecting --sub-auto setting
-- hacked from autoload.lua
local msg = require 'mp.msg'
local options = require 'mp.options'
local utils = require 'mp.utils'
o = { disabled = false }
options.read_options(o)
function split_ext(path)
    return string.match(path, "^(.+)%.([^%.]+)$")
end
table.filter = function(t, iter)
    for i = #t, 1, -1 do
        if not iter(t[i]) then
            table.remove(t, i)
        end
    end
end
function find_and_add_entries()
    local path = mp.get_property("path", "")
    local dir, filename = utils.split_path(path)
    local name, ext = string.match(filename, "(.+)%.([^%.]+)$")
    local fuzz = mp.get_property("sub-auto", "no")
    if o.disabled or #dir == 0 or fuzz == "no" then
        return
    end
    local files = utils.readdir(dir, "files")
    if not files then
        return
    end
    local logdir, logfile = utils.split_path(mp.get_property("log-file", ""))
    table.filter(files, function (v, k)
        local sname, sext = split_ext(v:lower())
        local name = name:lower()
        if fuzz=="all" and logfile and
            logdir:lower()==dir:lower() and
            logfile:lower()==v then
            return false
        end
        return (sext:lower() == "txt") and
        (
            (fuzz=="exact" and sname==name) or
            (fuzz=="fuzzy" and sname:find(name)==1) or
            fuzz=="all"
        )
    end)
    table.sort(files, function (a, b)
        local len = string.len(a) - string.len(b)
        if len ~= 0 then -- case for ordering filename ending with such as X.Y.Z
            local _, ext = split_ext(a)
            local ext = string.len(ext) + 1
            return string.sub(a, 1, -ext) < string.sub(b, 1, -ext)
        end
        return string.lower(a) < string.lower(b)
    end)
    msg.verbose("candidates:", utils.to_string(files))
    if dir == "." then
        dir = ""
    end
    for i = 1,#files do
        local subpath = files[i]
        local subname = split_ext(subpath)
        
        if subname:lower() == name:lower() then
            mp.commandv('sub-add', utils.join_path(dir,subpath), 'select')
        elseif fuzz == "all" or
            (fuzz == "fuzzy" and subname:lower():find(name:lower()) == 1) then
            mp.commandv('sub-add', utils.join_path(dir,subpath), 'auto')
        end
    end
end
mp.register_event("start-file", find_and_add_entries)
```

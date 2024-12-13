# Sera
This is a Low-level schematized serialization library for Roblox for handling limited size
dictionaries with primitive types. This library aims to do as few operations
(table lookups, comparisons, etc.) as possible to achieve it's result while
staying a good-enough serialization library.

Sera's intended purpose is to help with serialization shemas when handling high-efficiency game entity replication to clients. Note that this is not a module for beginners and you should at least know the basics of using the `buffer` class.

## Limitations:
- Schemas can only hold up to 255 fields in Sera and it's going to stay that way - you may fork the project and easily change this for yourself.
- `Sera.Serialize` and `Sera.DeltaSerialize` will fail if resulting buffer size is larger than  the constant `BIG_BUFFER_SIZE` inside the `Sera` module - This value should be moderate, but within reason since that's the memory `Sera` will take and never release during runtime.

[Source Code (Single ModuleScript)](https://github.com/MadStudioRoblox/Sera/blob/main/Sera.luau)

[Longer Example Code](https://github.com/MadStudioRoblox/Sera/blob/main/Test.luau)

***This is an experimental project - there will be no guarantees for backwards-compatibility on further updates***

## Short Example:

```lua

local Sera = require(game.ReplicatedStorage.Sera)

local Schema = Sera.Schema({
	Health = Sera.Int16,
	Position = Sera.Vector3,
	Name = Sera.String8,
})

local serialized, error_message = Sera.Serialize(Schema, {
    Health = 150,
    Position = Vector3.new(1001, 69, 0.1),
    Name = "loleris",
})

if error_message ~= nil then
    error(error_message)
end

local deserialized = Sera.Deserialize(Schema, serialized :: buffer)

print(`Serialized buffer size: {buffer.len(serialized :: buffer)} bytes`)
print(`Deserialized:`, deserialized)

```
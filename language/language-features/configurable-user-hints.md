# Configurable user hints

As a mod developer, you can help the users of your mod fix common errors by adding hint messages. These messages are displayed by the REDscript compiler when the user gets an error while launching the game. The hints are configured using TOML files that are stored in the `r6/config/redsUserHints/` folder.

### Example

Suppose a user runs into compilation errors because they don’t have TweakXL installed. TweakXL is a mod that many other mods need, so this is a common problem. The compiler will generate errors like this:

```log
 [ERROR] [UNRESOLVED_TYPE] At ./r6/scripts/CraftingQualityOfLife/RandomQualityModsTweakDB.reds:41:1:
public class AddFixedQualityModRecords extends ScriptableTweak {
^^^
class ScriptableTweak not found

 [ERROR] [UNRESOLVED_REF] At ./r6/scripts/CraftingQualityOfLife/RecraftIconicsTweakDB.reds:160:13:
            TweakDBManager.CloneRecord(newCraftingRecipeElement, weaponIngredientRecipeElement.GetID());
            ^^^^^^^^^^^^^^
unresolved reference TweakDBManager
```

The text in brackets contains the error codes (`UNRESOLVED_TYPE` and `UNRESOLVED_REF`). We can match on these codes to tell the user to install TweakXL. To do that, we need to create a TOML file in `r6/config/redsUserHints/{unique-name}.toml`. The file name can be anything, as long as it doesn’t conflict with other mods. Here is what the file could look like:

```toml
[[UNRESOLVED_REF]]
id = "MISSING_TWEAKXL"
span_starts_with = "TweakDBManager"
message = "one of your mods depends on TweakXL, try installing it"

[[UNRESOLVED_TYPE]]
id = "MISSING_TWEAKXL"
file = "CraftingQualityOfLife/RandomQualityModsTweakDB.reds"
line_contains = "extends ScriptableTweak"
message = "CraftingQualityOfLife depends on TweakXL, try installing it"
```

Each hint configuration consists of up to five parts:

* `[[ERROR_CODE]]` - the code of the error you want to match (you can find it in the compiler output)
* `id` - a name for the hint, which is used to avoid showing the same hint more than once (required)
* `span_starts_with` or `line_contains` - a pattern used to match the code that caused the error (required)
  * `span_starts_with` looks for a specific piece of code at the exact line and column where the error happened, which allows for a very precise match
  * `line_contains` looks for a match anywhere in the line where the error happened, this is more flexible, but can also match things you don’t want
* `file` - the path to the file where the error came from, by default these hints apply to all files, so you can use this to limit them to a specific file, if you think your hint might match something else in another file or mod (optional)
* `message` - the message that you want to show to the user when the error matches (required)

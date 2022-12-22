# Configurable user action messages

REDscript supports custom user action messages through TOML configuration files located at `r6/config/userActions/[unique-name].toml`. Mod authors can ship these with their mods and they will be picked up by the compiler and shown to users in a popup automatically when an error occurs.

### How-to

Given a scenario where a user has a broken installation due to a missing TweakXL dependency, they'll receive compilation error logs looking like this:

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

Every compilation error has a custom code (`UNRESOLVED_TYPE` and `UNRESOLVED_REF`in this case). These codes can be used to identify specific errors and provide nice hints to the user about the underlying cause (here they're missing a TweakXL dependency). This can be done by creating a TOML file at `r6/config/userActions/[unique-name].toml`(the file name can be anything as long as it's unique, it's not tied to the mod):

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

The TOML file contains a list of action definitions, each one containing:

* `[[ERROR_CODE]]` - the error code to match on (can be found in the compiler output)
* `id` - identifier of the user action, which is used to deduplicate them when several matches are made (required)
* `span_starts_with` or `line_contains`- string used for matching on source code that caused the error (required)
  * `span_starts_with` matches on a prefix of the error span, the specific piece of code that caused the error, this is the more specific and preferred option
  * `line_contains` searches the entire line where the error occured for a match, it's less specific and more powerful, but can more easily lead to false positives
* `file` - relative path to the file where the error originates from, by default these matchers are global, so this can be used to restrict them to a specific file, if you're worried your match can lead to false positives in other files/mods (optional)
* `message` - message to be shown to the user when the error is matched successfully (required)

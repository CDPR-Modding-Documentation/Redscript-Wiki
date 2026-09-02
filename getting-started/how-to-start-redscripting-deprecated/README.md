---
description: Downloaded and prepared my IDE to do some scripting, what now?
---

# How to start REDscripting (deprecated)

### Summary <a href="#summary" id="summary"></a>

Created: Apr 02 2024 by [mana vortex](https://app.gitbook.com/u/NfZBoxGegfUqB33J9HXuCs6PVaC3 "mention")\
Last documented edit: Jul 21 2026 by [Zhincore](https://app.gitbook.com/u/OsI9JXgCSSbt40hb327iBDif7Xv1 "mention")

## How to start?

Simply open your IDE and create an empty project.

It is **discouraged** to develop directly in the game's folder, you should create your projects outside of the game, as usual for any language. ~~(But no one’s stopping you for experiments)~~

Now simply create a text file with the .reds extension, for example `MyMod.reds`. Then you're ready to code!

In REDscript we are usually _injecting_ code using [hook annotations](../../language/hook-annotations-deprecated.md). There is no "main" function, however, you can use [_scriptable classes_](../../references-and-examples/scriptable-systems-deprecated/) to run your code, too.

{% code title="Example of basic hooking:" overflow="wrap" %}
```swift
@wrapMethod(DamageSystem)
private func ProcessLocalizedDamage(hitEvent: ref<gameHitEvent>) {
    Log("Yay, my custom code!");
    wrappedMethod(hitEvent);
}
```
{% endcode %}

This code wraps a method in the game's `DamageSystem` class, which allows us to run our code! The original method still runs thanks to the special `wrappedMethod` call, so we didn't break the game (yet).

If you wanna see more examples of REDscript see [redscript-in-2-minutes-deprecated.md](redscript-in-2-minutes-deprecated.md "mention"):

{% content-ref url="redscript-in-2-minutes-deprecated.md" %}
[redscript-in-2-minutes-deprecated.md](redscript-in-2-minutes-deprecated.md)
{% endcontent-ref %}

## Developing a mod

For starters it should be enough to copy-paste your script(s) into the `Cyberpunk 2077\r6\scripts` folder. You can hot-reload all scripts without restarting the game using RedHotTools:

{% content-ref url="https://app.gitbook.com/s/4gzcGtLrr90pVjAWVdTc/for-mod-creators-theory/modding-tools/redhottools/rht-hot-reload" %}
[RHT: Hot Reload](https://app.gitbook.com/s/4gzcGtLrr90pVjAWVdTc/for-mod-creators-theory/modding-tools/redhottools/rht-hot-reload)
{% endcontent-ref %}

When your mod grows, you probably want to give it more structure, have proper folders in your project, maybe add a [Red4Ext plugin](https://app.gitbook.com/o/-MP5ijqI11FeeX7c8-N8/s/-MjhIjZ0BBsL6SCohtnf/) or a [CET integration](https://app.gitbook.com/o/-MP5ijqI11FeeX7c8-N8/s/-MP5jWcLZLbbbzO-_ua1-887967055/). \
Before it all falls down on your head, consider using the `red-cli` tool which can not only bundle all your scripts together but also install them for you while you develop:

{% embed url="https://github.com/rayshader/cp2077-red-cli" %}

## Where to go next?

Depends on what you want to do!

You probably want to explore more of what REDscript does:

{% content-ref url="https://app.gitbook.com/s/-McniwB8YOK2HnJ7SYg_/language" %}
[Language Reference](https://app.gitbook.com/s/-McniwB8YOK2HnJ7SYg_/language)
{% endcontent-ref %}

If you want to interact with the game, you mostly want to hook up:

{% content-ref url="../how-to-create-a-hook-deprecated/" %}
[how-to-create-a-hook-deprecated](../how-to-create-a-hook-deprecated/)
{% endcontent-ref %}

Also checkout notable libraries, maybe they can help you towards your goal:

{% content-ref url="../../references-and-examples/libraries-deprecated.md" %}
[libraries-deprecated.md](../../references-and-examples/libraries-deprecated.md)
{% endcontent-ref %}

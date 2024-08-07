---
description: What is ScriptableSystem vs. ScriptableService?
---

# Scriptables comparison

## RedScript's `ScriptableSystem`&#x20;

Class in base RedScript.

* Is bound to the game session (save)&#x20;
  * Is created (attached) when loading a save
  * Is destroyed (detached) when unloading a save
* Can store things in a save
  * `persistent` properties will be saved in game's save file and value can differ between saves

Example and documentation: [scriptable-systems-singletons.md](../common-patterns/scriptable-systems-singletons.md "mention")

## Codeware's `ScriptableService`

Class added by [Codeware](https://www.nexusmods.com/cyberpunk2077/mods/7780) (Cannot be used without Codeware).

* Independent of saves
  * Is created when starting the game
  * Is destroyed when quitting the game
* Always runs with the game
* Can store things globally for all saves
  * `persistent` properties will be saved globally and will be same between saves
* Usually used for patching/modifying resources

Example and documentation: [https://github.com/psiberx/cp2077-codeware/wiki#lifecycle](https://github.com/psiberx/cp2077-codeware/wiki#lifecycle)

{% hint style="info" %}
Previously was named `ScriptableEnv`. Kept for backwards compatibility, do not use that name in new code.
{% endhint %}

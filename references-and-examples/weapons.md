---
description: How to do stuff with weapons, the Redscript way
---

# Weapons

### Weapon damage debugging

{% hint style="info" %}
You can find a list of weapon damage effects next door on the [yellow wiki.](https://app.gitbook.com/s/4gzcGtLrr90pVjAWVdTc/for-mod-creators/references-lists-and-overviews/cheat-sheet-tweak-ids/weapons/cheat-sheet-weapon-damage-effects)
{% endhint %}

To see damage types and damage proccs, you can useAngeVil's script ([gist](https://gist.github.com/Berdagon/6ae42e4ff6b0964808a91d32951c52e4)) and copy it to e.g. [`Cyberpunk 2077`](https://app.gitbook.com/s/4gzcGtLrr90pVjAWVdTc/for-mod-users/users-modding-cyberpunk-2077/the-cyberpunk-2077-game-directory)`/r6/scripts/debug_damage_types.reds`:

{% hint style="warning" %}
You have to include the [logging.md](../language/logging.md "mention") functions for this to work!
{% endhint %}

{% embed url="https://gist.github.com/Berdagon/6ae42e4ff6b0964808a91d32951c52e4" %}

---
description: Creating a hook (with code snippets)
---

# How to create a hook

Created by [HJHughJanus](https://github.com/HJHughJanus) on github, copied here for easier maintainability

Injecting your code without altering anything existing works via wrapping.\
Once you‘re sure what you want to do, you need to find out which system and which function within that system handles something close to your goal and then use this function as a wrapper function for your code.

## **Example**

_**Goal:**_ Ragdoll any NPC hit by a bullet.\
_**System:**_ I need to find out how Cyberpunk 2077 handles bullets and damage → a look through the decompiled game scripts shows there is a class called „DamageSystem“. This class looked up on Cyberdoc shows there is a function wihtin that class called „ProcessLocalizedDamage“. Back in the decompiled game scripts the code of this function seems to handle a „hit event“ and checking if the bullet hit the head or any weak spots. That is very close to my goal, so I will use this function for the hook.\
_**Functions:**_ I want to ragdoll an NPC, so I will need a function to do that. I search on Cyberdoc for „ragdoll“ and find something called „ApplyRagdollImpulseEvent“. I search this function in the decompiled game scripts to find out how it is used.

\
First, I need to specify which system I want to use for injection, then specify the function.

```swift
@wrapMethod(DamageSystem)
private func ProcessLocalizedDamage(hitEvent: ref<gameHitEvent>) {
}
```

In this function I must now call the vanilla function first, so I don‘t overwrite any of the vanilla action – you can do this via the function `wrappedMethod(params)` (this one automatically calls the vanilla function).

```swift
@wrapMethod(DamageSystem)
private func ProcessLocalizedDamage(hitEvent: ref<gameHitEvent>) {
wrappedMethod(hitEvent);
}
```

Then I copy some code from the vanilla function to handle some errors and set up my variables.

```swift
@wrapMethod(DamageSystem)
private func ProcessLocalizedDamage(hitEvent: ref<gameHitEvent>) {
    wrappedMethod(hitEvent);
    
    //variables
    let hitUserData: ref<HitShapeUserDataBase>;
    let hitShapes: array<HitShapeData> = hitEvent.hitRepresentationResult.hitShapes;
    
    //error handling and setting up hitUserData (copy & paste from original)
    if !hitEvent.attackData.GetInstigator().IsPlayer() {
      return;
    };
    
    if AttackData.IsAreaOfEffect(hitEvent.attackData.GetAttackType()) {
      return;
    };
    
    if ArraySize(hitShapes) > 0 {
      hitUserData = DamageSystemHelper.GetHitShapeUserDataBase(hitShapes[0]);
    };
    
    if !IsDefined(hitUserData) {
      return;
    };
}

```

Now my actual code can take effect. Since I found the „ApplyRagdollImpulseEvent“ and looked up how it is used, I know this will not be done in one line. So, to keep the code in the wrapper function simple, I will write a separate function below which sets up the ragdolling for me.

```swift
@wrapMethod(DamageSystem)
private func RagdollNPC(target: ref<NPCPuppet>, pushForce: Float) -> Void {
  if IsDefined(target) {
    target.QueueEvent(CreateForceRagdollEvent(n"Debug Command"));
    if pushForce > 0.0 {
      GameInstance.GetDelaySystem(target.GetGame()).DelayEvent(target, CreateRagdollApplyImpulseEvent(target.GetWorldPosition(), Vector4.Normalize(target.GetWorldPosition()) * pushForce, 5.00), 0.10, false);
    };
  };
}
```

Now my actual code can take effect. Since I found the „ApplyRagdollImpulseEvent“ and looked up how it is used, I know this will not be done in one line. So, to keep the code in the wrapper function simple, I will write a separate function below which sets up the ragdolling for me.

<pre class="language-swift"><code class="lang-swift"><strong>@wrapMethod(DamageSystem)
</strong>private func ProcessLocalizedDamage(hitEvent: ref&#x3C;gameHitEvent>) {
    //start vanilla function, so everything at least runs normally (even if the mod doesnt work)
    wrappedMethod(hitEvent);
    
    //variables
    let hitUserData: ref&#x3C;HitShapeUserDataBase>;
    let hitShapes: array&#x3C;HitShapeData> = hitEvent.hitRepresentationResult.hitShapes;
    
    //error handling and setting up hitUserData (copy &#x26; paste from original)
    if !hitEvent.attackData.GetInstigator().IsPlayer() {
      return;
    };
    
    if AttackData.IsAreaOfEffect(hitEvent.attackData.GetAttackType()) {
      return;
    };
    
    if ArraySize(hitShapes) > 0 {
      hitUserData = DamageSystemHelper.GetHitShapeUserDataBase(hitShapes[0]);
    };
    
    if !IsDefined(hitUserData) {
      return;
    };

    //actual mod code
    let npc: ref&#x3C;NPCPuppet> = hitEvent.target as NPCPuppet;
    let npcID: StatsObjectID = Cast(npc.GetEntityID());
    if IsDefined(npc) {
      RagdollNPC(npc, 10.0);
    };
}
</code></pre>

The whole file now looks like this:

<figure><img src="../../.gitbook/assets/create_a_hook_final_file" alt=""><figcaption></figcaption></figure>

This file I save and then put into `D:\PATH\TO\CYBERPUNK2077\r6\scripts` so the game will load it.&#x20;

When I start the game, every NPC hit by a bullet should be ragdolled with a push.

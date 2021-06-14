# shared-plugin


### Required Script Spaces

```
sbt_proof             Shared:sys:build-tool:proof
shared                Shared:sys:Core
shared_clothing       Shared:sys:CoreClothing
shared_consumable     Shared:sys:CoreConsumables
shared_drink          Shared:sys:CoreDrinks
shared_food           Shared:sys:CoreFoods
shared_object         Shared:sys:CoreObject
```

### Cron

```
Weekly

      <Core:Property property="merry:inherit:lib:cron_expand">
         \<Shared:sys:Core\>
      </Core:Property>
````

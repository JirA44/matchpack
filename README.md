# matchpack

The goal of this module is to find the best schematic layout for a set of `SchematicChip`s and `SchematicGroups` containing
`SchematicPins` connected to each other with `SchematicTraces`

To do this, we use a series of solvers that are run in a pipeline. Each solver is responsible for a specific piece of
input preprocessing or calculation.

This is roughly the hierarchy of solvers:

```
LayoutPipelineSolver: Runs pipeline
↳ ChipPartitionsSolver: Creates partitions (small subset groups) surrounding complex chips
  ↳ SingleChipPartitionSolver: Creates a single partition for a single chip
↳ PartitionPackingSolver: Packs the laid out chip partitions into a single layout
```

## Présentation en français

matchpack est un module qui cherche une disposition de schéma pour des composants, groupes, broches et connexions. Il exécute plusieurs solveurs en pipeline pour partitionner, calculer puis assembler la disposition.

### À quoi ça sert, et pourquoi ?

Il automatise une partie de l’agencement d’un schéma électronique et permet de traiter séparément des sous-problèmes de placement.

### Exemple

Fournir un ensemble de composants et de connexions, lancer le pipeline de disposition, puis examiner les positions proposées par les solveurs et les contraintes satisfaites.

## Implementation Notes

- There is the concept of a "weak" and "strong" connection between pins. A "strong" connection is one where a pin is directly assigned to another pin. A "weak" connection is generally a pin assigned to a net like "GND" or "VCC". Strong connections are important for layout but weak connections often determine orientation of passives (e.g. a capacitor is "facing up" to VCC but "facing down" to GND)
- Often there are pre-laid-out designs that are passed in. This is represented by a `SchematicGroup`. We don't lay out anything inside of these groups but the inner pins are still used to compute a good packing

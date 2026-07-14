# Stateware naming and compatibility

The public naming system separates the category, engineering method,
specification, platform, and applications:

| Name | Meaning |
|---|---|
| **Stateware** | Software whose durable, inspectable state and governed lifecycle contracts form the application boundary while agents and models remain replaceable processors. |
| **State-Centric Engineering** | The practice of building systems around canonical state, authority boundaries, reproducible execution, and evidence-backed closure. |
| **StateSpec** | The portable files, schemas, ownership rules, lifecycle contracts, context compilation, validation, and evidence requirements. |
| **StatePort** | The runtime and lifecycle platform. |
| **StateBench** | The evaluation system. |
| **StudyState** | The learning application/template. |
| **ClassState** | The classroom application/template. |

## Why old names still appear

Public prose now uses the names above. Existing repository names, file paths,
package/import names, environment variables, schema identifiers, persisted
fields, Git history, and URLs remain compatibility contracts until a separately
versioned migration changes them safely. For example, the StudyState repository
and this site's stable guide URLs may still contain `StudyDD` or `StateDD`.

Do not rename those identifiers by hand. A physical migration needs aliases,
upgrade tests, rollback, and an explicit removal schedule.

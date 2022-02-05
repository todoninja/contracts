# Task Schema

The Task schema uses semantic versioning in order to allow for schema changes to
be made without breaking existing tasks.

This means, that **adding** a new field to the schema results in the **minor**
version number being incremented. **Removing** a field from the schema results
in the **major** version number being incremented. **Changing** the type of a
field is **not** recommended, but can be done anyway. This will likely result in
the **major** version number being incremented, if not it has to be carefully
checked in advance if the changes will be backwards compatible.

## Version handling

An app always has one _supported_ version. Supported version is this context
means the version, that is actually expected in the code of the app. The
versions that are _compatible_ with an app are the versions the app can migrate
to the supported versions. Migrating is always a one-way action, it should not
be reversed. You migrate data from a lower to a higher version.

```yaml
version difference:
    lower than supported:
        patch: migrate
        minor: migrate
        major: migrate if user explicitly wants to, otherwise discard or go in compatibility-mode
    higher than supported:
        patch: ignore
        minor: ignore
        major: discard or compatibility-mode
```

### Compatibility mode

In compatibility mode, the app will try to read the data from the task and
display it as best as possible. Changes to the data are not allowed.

### Ignoring

Just ignore the difference in versions. This is a backwards-compatible change.
**Important**: Do not discard unknown properties on the resource, since this
will lead to data from the higher version being lost.

### Migrating

If there is a valid migration path, execute the steps in order to migrate the
resource to the new schema. If not, discard or go in compatibility mode.

### Discarding

Don't use this resource. Act like it was never received from the backend.

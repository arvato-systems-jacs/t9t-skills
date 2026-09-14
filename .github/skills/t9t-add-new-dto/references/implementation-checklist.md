# t9t DTO implementation checklist

This checklist is based on `docs/Adding-a-new-DTO.md` from the main `t9t` repository.

1. Create or confirm the BON DTO definitions in the `src/main/bon/dto` folder of the `-api` module:
   - `ExampleRef`
   - `ExampleKey`
   - `ExampleDTO`
2. Define BON requests in `src/main/bon/request`:
   - `ExampleCrudRequest`
   - `ExampleSearchRequest`
   - optional `ExampleLeanSearchRequest` for single-field natural keys
3. Register the `CrudViewModel` in the module's `IViewModelContainer` implementation and add the `register()` entry.
4. Add English and German header translations in `src/main/resources/translations/headers_en.properties` and `headers_de.properties`.
5. Add the grid config JSON in `src/main/resources/gridconfig/<viewModel>.json`.
6. Define the entity in the JPA module's `entity` BDDL file and include the natural-key uniqueness index.
7. Add the resolver method to the `*Resolvers.xtend` class annotated with `@AutoResolver42`.
8. Create the mapper Xtend class and the JPA request handlers:
   - search handler
   - CRUD handler
   - optional lean-search handler
9. Run `mvn clean compile`, inspect `src/generated/sql`, and create the versioned SQL migration file in `src/main/sql/POSTGRES/Migration`.
10. When the test area uses extension helpers, add an Xtend merge helper in a suitable `t9t-tests-*` module.
11. When UI work is required, add the screen ZUL file.
12. When UI work is required, add a placeholder menu icon.
13. When UI work is required, register the new screen in the ZK configuration properties file.

Do not skip validation:

- compile the changed modules
- run targeted tests for the touched area when they already exist
- confirm generated-SQL-based migration content matches the new entity

# Best Practices

This document outlines the guidelines that we, as Under the Oaks, have set for ourselves to follow when working on the project.
This includes coding, documentation, and working with Git.

## Story duration - Actual

After completing each issue, it is mandatory to note the time spent on it. Use the "Actual" field of the issue
itself. The time you spend working on the issue is rounded up to the next full hour; for example, 1 hour and 20 minutes is recorded as 2 hours.

![example image actual field in issue](image.png)

## Working on an issue

### New branch

If an issue requires changes to the repository, you must create a new branch for it. Such a branch is called a feature
or bug branch, depending on the type of issue.

This can be done from the issue (see image below), which links the new branch to the issue automatically. Alternatively,
this can be done manually by clicking on the cog icon next to the "Development" Section of the issue.

<warning> ALWAYS LINK YOUR BRANCH TO THE CORRESPONDING ISSUE </warning>

![example image where you can create a branch linked from an issue](image-2.png)

#### Naming convention for feature branch

The feature branch should be named as follows: "NR-issue-name-in-kebap-case".  
Bsp.: 9-update-feature-branch-naming-convention

### Commenting your Implementation

Annotate your classes and methods with JavaDoc.

#### Classes

Short description of the class, what it is supposed to do and what dependencies it has.

Bsp.:

```java
/**
 * Class for generating maps from text files.
 * <p>
 * This class is a singleton, and can be accessed using the {@link #getInstance()} method.
 * It provides methods for serializing and deserializing maps to and from JSON.
 * The JSON format used is the one provided by the {@link Json} class provided by libGDX.
 */
public class MapGenerator {}
```

#### Methods

Use the `@param`, `@throws` and the `@returns` JavaDoc annotations.

Bsp.:

```java
/**
* Continuously updates the map until no further updates are possible.
*
* <p>Keeps trying to update the map with the given {@code InteractionChain} until no more changes occur.</p>
*
* @param chain the {@code InteractionChain} used to manage interactions and snapshots during updates
* @throws IllegalStateException if the maximum iteration limit is exceeded, suggesting a potential cyclic dependencyn {@code TileContent}.
* @implNote This method has a limit on the number of iterations to prevent endless loops. If one {@code TileContent}
* triggers another in a cyclic manner, the loop may otherwise never terminate.
*/
public void updateUntilStable(InteractionChain chain) throws GameStateUpdateException {...}
```

### Testing your Implementation

Before creating your PR, you should always write new tests or update existing ones for your code changes. Code Coverage has to
be above 80% for files you changed!

To create an overview of the different logical paths in your implementation, you should always document the edge cases to test in the JavaDoc of the method.

```java
/**
* @implNote case x should result in method call y
*           case z should result in method call a
*           The code has <this unique feature> you need to consider when testing
*/
```

---

### Coding best practices

- If you work with IntelliJ IDEA, regularly press `Strg + Alt + L` to reformat your file.

---

### Commit Messages

The commit message should follow this format: "NR short summary of your changes."
Example: "9 changed naming convention to kebab case for issue branches."


---

### Pushing to development

After completing the issue:

1. Create and resolve a pull request from the development branch to your feature branch (this prevents merge conflicts on development).
2. Create a pull-request from development branch to main branch
3. 2 or more coworkers have to approve the pull-request
4. Merge the pull-request

### Pushing to main

1. Create a pull-request from development branch to main branch
2. 2 or more coworkers have to approve the pull-request
3. Merge the pull-request


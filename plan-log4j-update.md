# Log4j Migration Plan: parent-javalin-pom

**Layer**: 1 — update after `parent-pom`.

## Before Starting

Prompt the user for the following version numbers before making any changes:

| Variable | Question |
|----------|----------|
| `NEW_PARENT_POM_VERSION` | What is the new `parent-pom` version? |
| `OWN_NEW_VERSION` | What version should `parent-javalin-pom` be bumped to? (currently check `<version>` in pom.xml) |

## Context

Part of a migration from Log4j 1.x to Log4j 2.25.3 across all libraries. This POM only adds Javalin + Jetty on top of `parent-pom`. It has no logging config of its own — just needs the parent version bump to propagate the new logging backend.

## Current State

- **Artifact**: `info.unterrainer.commons:parent-javalin-pom`
- **Parent**: `parent-pom` (check current version in line 8)
- **Own dependencies**: javalin, jetty-http, jetty-server
- **log4j.properties**: none
- **@Slf4j usage**: none

## Steps

### 1. Update parent version in `pom.xml`

Change the parent version (line 8) to the new parent-pom version:

```xml
<parent>
    <groupId>info.unterrainer.commons</groupId>
    <artifactId>parent-pom</artifactId>
    <version>NEW_PARENT_POM_VERSION</version>
</parent>
```

### 2. Bump own version

Increment `<version>` (line 14).

### 3. Build and install locally

```bash
mvn clean install
```

### 4. Verify logging deps

```bash
mvn dependency:tree -Dincludes="*log4j*,*slf4j*"
```

Must show Log4j 2 artifacts inherited from parent-pom, not `slf4j-log4j12` / `slf4j-simple`.

## Files Changed

| File | Action |
|------|--------|
| `pom.xml` | Update parent version, bump own version |

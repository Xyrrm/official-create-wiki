---
minecraft_version: 1.21.1
create_version: 6.0.0-4
ponder_version: 1.0.39
flywheel_version: 1.0.0-9
registrate_version: MC1.21-1.3.0+62

next: false
---

<!--@include: ./template.md-->

::: code-group

```groovy [build.gradle]
repositories {
    maven { url = "https://maven.createmod.net" } // Create, Ponder, Flywheel
    maven { url = "https://mvn.devos.one/snapshots" } // Registrate
    maven { url = "https://raw.githubusercontent.com/Fuzss/modresources/main/maven/" } // ForgeConfigAPIPort
}

dependencies {
    implementation("com.simibubi.create:create-${minecraft_version}:${create_version}") { transitive = false }
    implementation("net.createmod.ponder:Ponder-NeoForge-${minecraft_version}:${ponder_version}")
    compileOnly("dev.engine-room.flywheel:flywheel-neoforge-api-${flywheel_minecraft_version}:${flywheel_version}")
    runtimeOnly("dev.engine-room.flywheel:flywheel-neoforge-${flywheel_minecraft_version}:${flywheel_version}")
    implementation("com.tterrag.registrate:Registrate:${registrate_version}"))
}
```

```kotlin [build.gradle.kts]
repositories {
    maven("https://maven.createmod.net") // Create, Ponder, Flywheel
    maven("https://mvn.devos.one/snapshots") // Registrate
}

dependencies {
    implementation("com.simibubi.create:create-${property("minecraft_version")}:${property("create_version")}") { isTransitive = false }
    implementation("net.createmod.ponder:Ponder-Forge-${property("minecraft_version")}:${property("ponder_version")}")
    compileOnly("dev.engine-room.flywheel:flywheel-forge-api-${property("flywheel_minecraft_version")}:${property("flywheel_version")}")
    runtimeOnly("dev.engine-room.flywheel:flywheel-forge-${property("flywheel_minecraft_version")}:${property("flywheel_version")}")
    implementation("com.tterrag.registrate:Registrate:${property("registrate_version")}")
}
```

```properties-vue [gradle.properties]
minecraft_version = {{ $frontmatter.minecraft_version }}
create_version = {{ $frontmatter.create_version }}
ponder_version = {{ $frontmatter.ponder_version }}
flywheel_version = {{ $frontmatter.flywheel_version }}
registrate_version = {{ $frontmatter.registrate_version }}
```

:::

### Production Environment Dependency

This type of dependency is added to the `neoforge.mods.toml` file so that NeoForge knows your mod will not work without
a certain
version of Create. This type of dependency is only useful if a development environment dependency was also added. This
type of dependency should not be added if your mod is able to work in a production environment without Create.

#### Configuration

Add the following dependency definition to your `neoforge.mods.toml` file,
replacing `${modid}` with your mod's ID. This tells Forge that your mod depends on Create.
If Create is not present or is outdated, NeoForge will display an error screen explaining this to the user.

```toml-vue   
[[dependencies.${mod_id}]]
    modId="create"
    type="required"
    versionRange="[{{ create_version_no_build_number }},)"
    ordering="NONE"
    side="BOTH"
```
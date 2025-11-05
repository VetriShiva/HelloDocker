# ðŸ§© Kotlin Not Configured â€” Quick Fix Guide (IntelliJ IDEA)

If Kotlin **was working before** and suddenly shows  
`âš ï¸ Kotlin not configured` â€” it usually means IntelliJ lost its Kotlin module linkage after an update, cache corruption, or Maven/Gradle reimport.

---

## ðŸ§± 1ï¸âƒ£ Re-sync or Re-import Project

1. Open **Maven / Gradle** tool window in IntelliJ.  
2. Click the ðŸ”„ **Reload All Maven Projects** button.  
3. Wait until indexing finishes.

âž¡ï¸ This re-attaches the Kotlin runtime and plugin to each module.

---

## âš™ï¸ 2ï¸âƒ£ Check Module SDK and Kotlin Facet

1. Go to **File â†’ Project Structure â†’ Modules**.  
2. Select your Kotlin module (e.g., `dvp-server`).  
3. Under **Dependencies tab**:
   - Verify **Module SDK** = `JDK 21` (or your version).
   - Verify **Language level** is correct.  
4. Under **Kotlin tab / Kotlin facet** (if visible):
   - Ensure â€œUse project settingsâ€ or correct compiler path selected.  

If Kotlin facet is missing:
> **Add â†’ Kotlin â†’ JVM**

---

## ðŸ§° 3ï¸âƒ£ Ensure Kotlin Plugin Is Active

- Go to **Settings â†’ Plugins â†’ Installed â†’ Kotlin**.
- If disabled â†’ **Enable** and restart IDE.
- If already enabled â†’ click âš™ï¸ â†’ **Uninstall â†’ Reinstall**.

---

## ðŸ” 4ï¸âƒ£ Rebuild Cache

In IntelliJ:
> **File â†’ Invalidate Caches / Restart â†’ Invalidate and Restart**

This clears stale metadata that causes Kotlin detection loss.

---

## ðŸ§© 5ï¸âƒ£ Clean & Rebuild from Terminal

**For Maven:**
```bash
mvn clean install -U
```

**For Gradle:**
```bash
./gradlew clean build --refresh-dependencies
```

---

## ðŸ§  6ï¸âƒ£ (If Still Persists) â€“ Add Kotlin Facet Manually

1. Right-click your module â†’ **Add Framework Supportâ€¦**  
2. Select **Kotlin (JVM)** â†’ click **OK**  
3. IntelliJ re-adds Kotlin configuration automatically.

---

## ðŸª„ 7ï¸âƒ£ Optional Sanity Check

1. Open any Kotlin file.  
2. Go to **Preferences â†’ Build, Execution, Deployment â†’ Compiler â†’ Kotlin Compiler**.  
3. Ensure **Target JVM = 21** (or same as your project JDK).

---

## âœ… After All Steps

The yellow â€œKotlin not configuredâ€ banner should disappear.

If not, delete these files manually and reopen project:

```
.idea/kotlinc.xml
.idea/misc.xml
```

IntelliJ will regenerate them automatically.

---

## ðŸ’¡ Tip

If your project uses multiple modules (like `dvp-server`, `controller-utils`, etc.),  
ensure **each submodule** has the same:
- Kotlin version  
- Kotlin plugin configuration  
- Project SDK  

---

## ðŸ§¾ Example of Kotlin Plugin for Maven Projects

If IntelliJ still doesn't detect Kotlin, manually verify the plugin in your `pom.xml`:

```xml
<properties>
  <kotlin.version>2.0.21</kotlin.version>
</properties>

<dependencies>
  <dependency>
    <groupId>org.jetbrains.kotlin</groupId>
    <artifactId>kotlin-stdlib</artifactId>
    <version>${kotlin.version}</version>
  </dependency>
  <dependency>
    <groupId>org.jetbrains.kotlin</groupId>
    <artifactId>kotlin-reflect</artifactId>
    <version>${kotlin.version}</version>
  </dependency>
</dependencies>

<build>
  <plugins>
    <plugin>
      <groupId>org.jetbrains.kotlin</groupId>
      <artifactId>kotlin-maven-plugin</artifactId>
      <version>${kotlin.version}</version>
      <executions>
        <execution>
          <id>compile</id>
          <goals><goal>compile</goal></goals>
        </execution>
        <execution>
          <id>test-compile</id>
          <goals><goal>test-compile</goal></goals>
        </execution>
      </executions>
    </plugin>
  </plugins>
</build>
```

After saving, reimport Maven in IntelliJ.

---

## ðŸ§  Summary

| Step | Action | Purpose |
|------|---------|----------|
| 1 | Reload Maven/Gradle | Re-link Kotlin build |
| 2 | Verify SDK | Ensure JDK + Kotlin facet active |
| 3 | Check Plugin | Confirm Kotlin plugin installed |
| 4 | Invalidate Cache | Clear corrupted IDE cache |
| 5 | Clean Build | Regenerate Kotlin classes |
| 6 | Add Kotlin Facet | Force reconfiguration |
| 7 | Verify Compiler | Match JVM target with JDK |

---

âœ… **Result:**  
Your Kotlin environment is re-linked with IntelliJ,  
the warning disappears, and `.kt` files compile normally again.
```
Subject: Action Required – Incorrect CardDetails Returned from ESB (AURA Prepaid)
Hi ,
We have identified that the MAS error in the AURA Prepaid flow is caused by ESB returning incorrect card details.
Specifically, when MAS fetches CardDetails from ESB for customer 11346024, ESB returns the data of another customer (113145296). This data inconsistency is the root cause of the failure.
Requesting your assistance to follow up with the concerned ESB/MAS team to validate and correct the mapping.
Happy to share more details if required.
```


```
Subject: AURA Prepaid – CardDetails API Returning Wrong Customer Data
Hi ,
Good afternoon. I wanted to bring a small observation to your attention regarding the AURA Prepaid CardDetails API behavior.
When calling GetCardDetails for customer 11346024 (MAZOUN HASAN ALFASHTAKI), MAS is returning the data of a different customer (113145296 – DAWOOD ALI). This mismatch is causing our AURA prepaid flow to fail during validation.
I have captured the complete request/response, and I’ve highlighted the incorrect customer fields in the attached screenshot.
From the initial review, this issue seems to be at the MAS layer.
If you’d like, I can:
Coordinate with MAS/ESB team for RCA
Open an incident for tracking
Provide a short summary for business impact
Just let me know how you prefer to proceed.
```





# OpenAPI Generator - README
This document explains how to configure, run, and maintain the OpenAPI Generator Maven Plugin used in this module.

------------------------------------------------------------

## 1. Overview

This module uses the OpenAPI Generator Maven Plugin to generate Java model classes from an OpenAPI/Swagger specification.

Benefits:
- Consistent domain models
- No manual DTO coding
- Automatic updates when API spec changes
- Clean and maintainable code

------------------------------------------------------------

## 2. Plugin Location

The plugin lives in:

pom.xml  ->  build  -> plugins  -> openapi-generator-maven-plugin

Each generation task is added as a separate execution block.

------------------------------------------------------------

## 3. File Structure

module/
  src/
    main/
      java/
      resources/
        CivilIdDetails.json
  pom.xml
  target/
    generated-sources/openapi/
      src/main/java/...

------------------------------------------------------------

## 4. Running Code Generation

### 4.1 Run entire plugin

```
mvn openapi-generator:generate
```

### 4.2 Run a specific execution (recommended for multi-module)

```
mvn -pl <module> openapi-generator:generate -DexecutionId=<id>
```

Example:

```
mvn -pl domain/customer openapi-generator:generate -DexecutionId=generate-civil-id-details
```

------------------------------------------------------------

## 5. Key Configuration Fields

| Field | Description |
|-------|-------------|
| inputSpec | Path to OpenAPI JSON/YAML |
| generatorName | java / kotlin / spring / typescript |
| output | Generated output folder |
| modelPackage | Package for DTOs |
| typeMappings | Override default types |
| configOptions | Extra settings |
| generateApis | Enables/disables REST clients |
| generateModels | Enables/disables model generation |

------------------------------------------------------------

## 6. Java Model Generation Only (No REST Client)

```
<generateApis>false</generateApis>
<generateApiTests>false</generateApiTests>
<generateApiDocumentation>false</generateApiDocumentation>

<generateModels>true</generateModels>
<generateModelTests>false</generateModelTests>
<generateModelDocumentation>false</generateModelDocumentation>
```

------------------------------------------------------------

## 7. Common Java Config Options

```
<configOptions>
  <dateLibrary>java8</dateLibrary>
  <serializableModel>true</serializableModel>
  <useBeanValidation>false</useBeanValidation>
  <hideGenerationTimestamp>true</hideGenerationTimestamp>
  <enumPropertyNaming>original</enumPropertyNaming>
</configOptions>
```

------------------------------------------------------------

## 8. Type Mappings

```
<typeMappings>
  <typeMapping>
    ResponseStatus=com.nbk.dvp.utils.client.model.ResponseStatus
  </typeMapping>
  <typeMapping>
    OffsetDateTime=String
  </typeMapping>
</typeMappings>
```

------------------------------------------------------------

## 9. Example Plugin Execution

```
<execution>
    <id>generate-civil-id-details</id>
    <goals>
        <goal>generate</goal>
    </goals>

    <configuration>
        <inputSpec>${project.basedir}/src/main/resources/CivilIdDetails.json</inputSpec>
        <generatorName>java</generatorName>

        <generateApis>false</generateApis>
        <generateModels>true</generateModels>

        <output>${project.build.directory}/generated-sources/openapi</output>
        <sourceFolder>src/main/java</sourceFolder>
        <modelPackage>com.nbk.dvp.domain.customer.civilid.model</modelPackage>

        <typeMappings>
            <typeMapping>
                ResponseStatus=com.nbk.dvp.utils.client.model.ResponseStatus
            </typeMapping>
            <typeMapping>
                OffsetDateTime=String
            </typeMapping>
        </typeMappings>

        <configOptions>
            <dateLibrary>java8</dateLibrary>
            <serializableModel>true</serializableModel>
            <hideGenerationTimestamp>true</hideGenerationTimestamp>
            <useBeanValidation>false</useBeanValidation>
        </configOptions>
    </configuration>
</execution>
```

------------------------------------------------------------

## 10. Cleaning and Regenerating

```
mvn clean
mvn -pl domain/customer openapi-generator:generate -DexecutionId=generate-civil-id-details
```

------------------------------------------------------------

## 11. Ignore Generated Code (Optional)

Add to .gitignore:

```
/target/generated-sources/openapi/
```

------------------------------------------------------------

## 12. Troubleshooting

| Issue | Fix |
|-------|------|
| Models not updated | Re-run execution |
| IntelliJ not showing files | Mark folder as Generated Sources Root |
| Wrong type | Update typeMappings |
| Missing default values | Check required fields in OpenAPI spec |

------------------------------------------------------------

## 13. Revision History

| Version | Date | Description |
|---------|-------|-------------|
| 1.0 | dd-MMM-yyyy | Initial README |


```
<execution>
    <id>generate-civil-id-details</id>
    <goals>
        <goal>generate</goal>
    </goals>

    <configuration>
        <!-- Input OpenAPI Spec -->
        <inputSpec>${project.basedir}/src/main/resources/CivilIdDetails.json</inputSpec>

        <!-- Java Generator -->
        <generatorName>java</generatorName>

        <!-- Only generate models -->
        <generateApis>false</generateApis>
        <generateApiTests>false</generateApiTests>
        <generateApiDocumentation>false</generateApiDocumentation>

        <generateModels>true</generateModels>
        <generateModelTests>false</generateModelTests>
        <generateModelDocumentation>false</generateModelDocumentation>

        <!-- Output directory -->
        <output>${project.build.directory}/generated-sources/openapi</output>
        <sourceFolder>src/main/java</sourceFolder>

        <!-- Packages -->
        <modelPackage>com.nbk.dvp.domain.customer.civilid.model</modelPackage>

        <!-- Custom Type Mappings -->
        <typeMappings>
            <typeMapping>
                ResponseStatus=com.nbk.dvp.utils.client.model.ResponseStatus
            </typeMapping>
            <typeMapping>
                OffsetDateTime=String
            </typeMapping>
        </typeMappings>

        <configOptions>
            <!-- Clean Java POJO configuration -->
            <dateLibrary>java8</dateLibrary>
            <useBeanValidation>false</useBeanValidation>
            <serializableModel>true</serializableModel>
            <hideGenerationTimestamp>true</hideGenerationTimestamp>
            <enumPropertyNaming>original</enumPropertyNaming>

            <!-- No client code -->
            <library>java8</library> <!-- required but ignored when generateApis=false -->
        </configOptions>
    </configuration>
</execution>
```


```
Sure — here is the formal version including environment & build details placeholders (replace as needed):
Hi Team,
The reported issue has been fixed, and the latest build has been deployed for validation in the <Environment Name – e.g., UAT / SIT / T4> environment.
Build / Deployment Details:
Build Version: <enter build version or tag>
Deployment Time: <enter timestamp>
Reference Ticket / Bug ID: <enter ID>
Requesting QA to proceed with re-testing and perform a full end-to-end regression across all prepaid card journeys to ensure there is no functional or behavioural impact.
Scenarios to validate:
Existing user with AURA card
Existing user without AURA card
VDK service available and unavailable cases
Duplicate journey prevention and resume flow
Error handling, fallback behavior & localization (EN/AR)
Additionally, kindly share the final E2E test cases and coverage for alignment and to maintain consistency for future regression cycles.
Please confirm once testing is completed and provide the status (Pass/Fail with evidence).
Thanks & Regards,
Vetri
If you want, I can also provide a Teams message, WhatsApp-style short note, or Jira comment version.
```



```
Here is a complete Spring Boot configuration that sets up a RestTemplate bean with custom key store and trust store, and injects them using properties from application.properties.
This setup works for calling HTTPS services with mutual TLS (mTLS) if required.
✅ 1. RestTemplate Configuration with Custom SSL (Java Config)
Copy code
Java
package com.example.config;

import org.apache.http.conn.ssl.NoopHostnameVerifier;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.conn.ssl.TrustSelfSignedStrategy;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.ssl.SSLContexts;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.client.HttpComponentsClientHttpRequestFactory;
import org.springframework.web.client.RestTemplate;

import javax.net.ssl.SSLContext;
import java.io.File;
import java.io.FileInputStream;
import java.security.KeyStore;

@Configuration
public class RestTemplateConfig {

    @Value("${ssl.key-store}")
    private String keyStorePath;

    @Value("${ssl.key-store-password}")
    private String keyStorePassword;

    @Value("${ssl.trust-store}")
    private String trustStorePath;

    @Value("${ssl.trust-store-password}")
    private String trustStorePassword;

    @Bean
    public RestTemplate restTemplate(RestTemplateBuilder builder) throws Exception {
        // Load KeyStore
        KeyStore keyStore = KeyStore.getInstance("PKCS12");
        keyStore.load(new FileInputStream(getFileFromPath(keyStorePath)), keyStorePassword.toCharArray());

        // Load TrustStore
        KeyStore trustStore = KeyStore.getInstance("JKS");
        trustStore.load(new FileInputStream(getFileFromPath(trustStorePath)), trustStorePassword.toCharArray());

        // Build SSLContext
        SSLContext sslContext = SSLContexts.custom()
                .loadKeyMaterial(keyStore, keyStorePassword.toCharArray())
                .loadTrustMaterial(trustStore, new TrustSelfSignedStrategy())
                .build();

        // Set up HTTP client with SSL
        SSLConnectionSocketFactory socketFactory =
                new SSLConnectionSocketFactory(sslContext, NoopHostnameVerifier.INSTANCE);

        CloseableHttpClient httpClient = HttpClients.custom()
                .setSSLSocketFactory(socketFactory)
                .build();

        HttpComponentsClientHttpRequestFactory requestFactory =
                new HttpComponentsClientHttpRequestFactory(httpClient);

        return builder
                .requestFactory(() -> requestFactory)
                .build();
    }

    private File getFileFromPath(String path) {
        if (path.startsWith("classpath:")) {
            String cleanPath = path.replace("classpath:", "");
            return new File(getClass().getClassLoader().getResource(cleanPath).getFile());
        }
        return new File(path);
    }
}
📄 2. application.properties
Copy code
Properties
ssl.key-store=classpath:keystore.p12
ssl.key-store-password=changeit
ssl.trust-store=classpath:truststore.jks
ssl.trust-store-password=changeit
✅ 3. Usage in Your Service Class
Copy code
Java
@Service
public class MyApiService {

    private final RestTemplate restTemplate;

    public MyApiService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public String callSecureEndpoint() {
        String url = "https://your-secure-api.com/data";
        return restTemplate.getForObject(url, String.class);
    }
}
🧪 Notes
Your key store should be .p12 (PKCS#12 format), and trust store usually .jks.
Place both files in src/main/resources if using classpath: in properties.
You can verify SSL is working correctly by enabling debug logs:
-Djavax.net.debug=ssl
Would you also like the WebClient version for reactive applications, or are you working only with RestTemplate?
```


```
To integrate mutual TLS (mTLS) HTTP client support into your Spring Boot mock server project, we’ll:
✅ Goal
Add an HTTP client with mTLS (mutual TLS authentication)
Use it to make outgoing secure calls (e.g., to another API)
Allow mTLS config via application.yml
🔧 Step-by-Step Integration
1. ✅ Add HTTP Client Config
In pom.xml, add this if not already present:
Copy code
Xml
<!-- HTTP Client with TLS support -->
<dependency>
    <groupId>org.apache.httpcomponents.client5</groupId>
    <artifactId>httpclient5</artifactId>
    <version>5.2</version>
</dependency>
2. 📁 Add mTLS Config to application.yml
Copy code
Yaml
mtls:
  enabled: true
  key-store: classpath:client-keystore.p12
  key-store-password: changeit
  trust-store: classpath:client-truststore.p12
  trust-store-password: changeit
You can also move this to the external application.yml you're already using.
3. 🧠 Create MtlsHttpClientConfig.java
Copy code
Java
package com.example.mockserver.config;

import org.apache.hc.client5.http.classic.HttpClient;
import org.apache.hc.client5.http.impl.classic.HttpClients;
import org.apache.hc.core5.ssl.SSLContextBuilder;
import org.apache.hc.core5.ssl.SSLContexts;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.net.ssl.SSLContext;
import java.io.File;
import java.security.KeyStore;

@Configuration
public class MtlsHttpClientConfig {

    @Value("${mtls.enabled}")
    private boolean mtlsEnabled;

    @Value("${mtls.key-store}")
    private String keyStorePath;

    @Value("${mtls.key-store-password}")
    private String keyStorePassword;

    @Value("${mtls.trust-store}")
    private String trustStorePath;

    @Value("${mtls.trust-store-password}")
    private String trustStorePassword;

    @Bean
    public HttpClient httpClient() throws Exception {
        if (!mtlsEnabled) {
            return HttpClients.createDefault();
        }

        SSLContext sslContext = SSLContexts.custom()
                .loadKeyMaterial(
                        getFileFromPath(keyStorePath), keyStorePassword.toCharArray(), keyStorePassword.toCharArray()
                )
                .loadTrustMaterial(
                        getFileFromPath(trustStorePath), trustStorePassword.toCharArray()
                )
                .build();

        return HttpClients.custom()
                .setSSLContext(sslContext)
                .build();
    }

    private File getFileFromPath(String path) throws Exception {
        if (path.startsWith("classpath:")) {
            String cleanPath = path.replace("classpath:", "");
            return new File(getClass().getClassLoader().getResource(cleanPath).toURI());
        }
        return new File(path);
    }
}
4. 🌐 Example Service: Use mTLS HTTP Client
Copy code
Java
package com.example.mockserver.service;

import org.apache.hc.client5.http.classic.HttpClient;
import org.apache.hc.client5.http.classic.methods.HttpGet;
import org.apache.hc.core5.http.HttpResponse;
import org.apache.hc.core5.http.io.entity.EntityUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ExternalMtlsClient {

    @Autowired
    private HttpClient httpClient;

    public String callSecureService(String url) throws Exception {
        HttpGet request = new HttpGet(url);
        HttpResponse response = httpClient.execute(request);
        return EntityUtils.toString(response.getEntity());
    }
}
5. 🌍 Endpoint for Testing Secure Call
Copy code
Java
@GetMapping("/mtls-test")
public ResponseEntity<String> mtlsTest() throws Exception {
    String response = externalMtlsClient.callSecureService("https://secure-api.example.com");
    return ResponseEntity.ok(response);
}
🔐 Required Files for mTLS
Place the following in src/main/resources or your config/ folder:
client-keystore.p12 — with your client certificate + private key
client-truststore.p12 — with CA cert of server you're calling
Generate with:
Copy code
Bash
# Example to generate keystore
keytool -genkeypair -alias client -keyalg RSA -storetype PKCS12 \
  -keystore client-keystore.p12 -storepass changeit

# Example truststore
keytool -importcert -file server-cert.pem -keystore client-truststore.p12 \
  -storepass changeit -alias server
✅ Summary
Feature
Status
mTLS support
✅
Outgoing HTTPS
✅
Configurable
✅ via YAML
Runtime ready
✅
Would you like:
🔐 Incoming mTLS support (server-side validation)?
🧪 Postman collection or test endpoint for mTLS?
🐳 Dockerfile with cert mounting?
Let me know and I’ll add that.
```

Hey Naji, noticed the 403 counts are still on the higher side — maybe some repeated MAS calls (like firm2D reminders). Let’s review when you get a chance 😊
```
data class LoanScheduledPayment(
    val autoRepayDay: Int?,
    val nextDate: LocalDate
)

fun LoanData.toScheduledPayment(today: LocalDate = LocalDate.now()): LoanScheduledPayment {
    val autoDay = autoRepayDay
    val baseDate = upcomingInstallmentDate

    val nextDate = when {
        // Case 1: Auto repay not configured
        autoDay == null -> baseDate

        // Case 2: Fixed date in same or next month
        else -> {
            // Determine the target month
            val thisMonthTarget = getSafeDate(today.year, today.monthValue, autoDay)
            val nextMonthTarget = getSafeDate(today.year, today.monthValue + 1, autoDay)

            // If today is before or equal to auto repay day, use current month
            if (today.isBefore(thisMonthTarget) || today.isEqual(thisMonthTarget))
                thisMonthTarget
            else
                nextMonthTarget
        }
    }

    return LoanScheduledPayment(autoDay, nextDate)
}

/**
 * Returns a valid date even if the requested day (e.g. 30 or 31)
 * doesn't exist in the given month (like February).
 */
fun getSafeDate(year: Int, month: Int, day: Int): LocalDate {
    val ym = YearMonth.of(year, month.coerceIn(1, 12))
    val validDay = minOf(day, ym.lengthOfMonth()) // handles 28/30/31 gracefully
    return ym.atDay(validDay)
}


private val KUWAIT_ZONE_ID = ZoneId.of("Asia/Kuwait")

/**
 * Safely computes a valid date within the given month.
 * Example: if day=31 but the month has 30 days, adjusts to 30.
 */
fun getValidDate(year: Int, month: Int, day: Int): ZonedDateTime {
    val safeMonth = month.coerceIn(1, 12)
    val yearMonth = YearMonth.of(year, safeMonth)
    val safeDay = day.coerceIn(1, yearMonth.lengthOfMonth())

    return yearMonth
        .atDay(safeDay)
        .atStartOfDay(KUWAIT_ZONE_ID)
}

/**
 * Computes the next scheduled payment date for a loan, respecting autopay day
 * and month rollovers.
 */
fun LoanData.toScheduledPayment(): LoanScheduledPayment? {
    val repayDay = autoRepayDay?.toIntOrNull() ?: return null
    if (repayDay <= 0) return null

    val baseDate = upcomingInstallmentDate ?: return null
    val nextDate = getValidDate(
        year = baseDate.year,
        month = baseDate.monthValue + 1, // move to next month
        day = repayDay
    )

    return LoanScheduledPayment(
        scheduledDay = repayDay,
        scheduledDate = nextDate
    )
}


fun changeDaySafely(baseDateTime: ZonedDateTime, newDay: Int): ZonedDateTime {
    val ym = YearMonth.from(baseDateTime)
    val validDay = minOf(newDay, ym.lengthOfMonth())
    return ym.atDay(validDay).atStartOfDay(baseDateTime.zone)
}
```

```
### Subject: Request to Update | Synthetic Monitoring 2.5.x

**Dear IT Service Management Team,**

Hope you're doing well. 😊  

Kindly find below the updated JAR for Synthetic Monitoring:  

📦 [performance-monitoring-2.5.13-jar-with-dependencies.jar](\\link\svom\n\Userdata\Common_Share\ETD_Stage\DVP\Release\PROD\performance-monitoring\performance-monitoring-2.5\performance-monitoring-2.5.13-jar-with-dependencies.jar)

Please help update this on the server when convenient.  
Let me know if you need any additional details or support.  

Thanks a lot for your help! 🙏  

**Best regards,**  
Vetri
```


# Loan EMI Schedule – Next Date Calculation Matrix

> **Rule:**  
> If the selected schedule day is already past in the current month,  
> → Show **due date (1st of next month)**  
> → From next month onward, follow user-selected schedule day.  
> Includes 1-month prepayment & 3-month grace period logic.

| # | Login Date | Schedule Day | Grace (months) | Prepaid (1 mo) | **Next Schedule Date** | **Month+1** | **Month+2** | Reason |
|---|-------------|---------------|----------------|----------------|-------------------------|--------------|--------------|--------|
| 1 | 2025-11-19 | 20 | 0 | ❌ | 2025-11-20 | 2025-12-20 | 2026-01-20 | In future same month |
| 2 | 2025-11-21 | 20 | 0 | ❌ | **2025-12-01 (Due)** | 2025-12-20 | 2026-01-20 | Past-date → due first |
| 3 | 2025-11-30 | 31 | 0 | ❌ | **2025-12-01 (Due)** | 2025-12-31 | 2026-01-31 | Month end fallback |
| 4 | 2025-12-02 | 31 | 0 | ❌ | 2025-12-31 | 2026-01-31 | 2026-02-28 | 31 → valid month |
| 5 | 2025-12-30 | 25 | 0 | ❌ | **2026-01-01 (Due)** | 2026-01-25 | 2026-02-25 | Past-date → due first |
| 6 | 2025-12-30 | 25 | 0 | ✅ | **2026-01-25** | 2026-02-25 | 2026-03-25 | Prepaid skips due |
| 7 | 2025-12-10 | 31 | 3 | ❌ | **2026-03-31** | 2026-04-30 | 2026-05-31 | 3-month grace applied |
| 8 | 2026-01-30 | 31 | 0 | ❌ | 2026-01-31 | 2026-02-28 | 2026-03-31 | Valid same month |
| 9 | 2026-01-30 | 31 | 0 | ✅ | **2026-02-28** | 2026-03-31 | 2026-04-30 | Prepaid skips Jan |
| 10 | 2026-02-27 | 31 | 0 | ❌ | 2026-02-28 | 2026-03-31 | 2026-04-30 | Feb fallback |
| 11 | 2026-02-28 | 20 | 0 | ❌ | **2026-03-01 (Due)** | 2026-03-20 | 2026-04-20 | Past-date → due |
| 12 | 2026-02-01 | 20 | 0 | ❌ | 2026-02-20 | 2026-03-20 | 2026-04-20 | Future same month |

---

✅ **Key behaviors:**
- Past schedule → use **due date (1st next month)**.
- Grace → defer full months (resume after grace).
- Prepaid → skip immediate next schedule.
- 31st day → fallback to last day of month (e.g., Feb 28).
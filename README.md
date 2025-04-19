# Custom-Splunk-Dashboard: Auth & Connection Logs

## Objective
This project was designed and built independently to deepen my skills with Splunk. I ingested two log types — Linux `auth.log` and Zeek-style `conn.log` — then used SPL queries and field extraction to create dashboards. The project demonstrates SIEM use cases such as brute-force login detection, network traffic monitoring, and protocol analysis.

### Skills Learned

- Manual log ingestion via Splunk Web interface
- Field extraction using regular expressions and `rex`
- Creating time-based dashboards and visual panels
- Using SPL to filter, group, and display results
- Basic threat hunting with real log examples

### Tools Used

- Splunk Enterprise (installed locally)
- Sample logs: `auth.log` (SSH login activity), `conn.log` (network flows)
- SPL (Search Processing Language)
- Custom dashboards and panels in Splunk UI

## Steps
### 1. Log Ingestion
Uploaded two different log types into Splunk using the **Upload** feature in the Web UI:

- `auth.log`: Simulated Linux authentication logs
- `conn.log`: Zeek-style network traffic logs


### 2. Search & Exploration (auth.log)

Used SPL to search through authentication events.
Example: 
- To look for failed passwords:
  `index=main sourcetype=linux_auth_logs "Failed password"`
 ![image](https://github.com/user-attachments/assets/072a2e62-b254-4700-8286-09a72a69686a)

- To look for successful logins: `index=main sourcetype=linux_auth_logs "Accepted password"`
  ![image](https://github.com/user-attachments/assets/0af80ae1-f677-4da7-90e0-85f41c803e72)


### 3. Field Extraction (conn.log) 

Extracted fields like source IP, destiantion IP, and protocol using rex: `index=main sourcetype=zeek_conn
| rex field=_raw "^(?<timestamp>\d+\.\d+)\t(?<src_ip>\d{1,3}(?:\.\d{1,3}){3})\t(?<src_port>\d+)\t(?<dest_ip>\d{1,3}(?:\.\d{1,3}){3})\t(?<dest_port>\d+)\t(?<protocol>\w+)"
| table _time src_ip src_port dest_ip dest_port protocol`
![image](https://github.com/user-attachments/assets/2232f282-edb0-4663-8033-143ab57bb6df)


### 4. Dashboard Creation

Created a Splunk dashboard from scratch using both log types. Panels include:

Auth log:
- Failed logins by IP
- Successful login count
- Login activity over time
![image](https://github.com/user-attachments/assets/528588da-249b-43f0-b67c-72fae8e46301)


Conn log
- Top source IPs & destination IPs 
- Traffic by protocol
![image](https://github.com/user-attachments/assets/d2e3e61b-ac5d-42a2-9582-49ebdaf424e3)


### Summary and Findings
- Auth.log was used to simulate SOC login monitoring and detect brute-force behavior. 
- Conn.log provided insight into network activity and potential reconnaissance.

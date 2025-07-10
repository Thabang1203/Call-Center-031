**Call Center 031 Real-Time Monitoring System**  
*Based in Durban, South Africa*  

---

**PROJECT OVERVIEW**  
Simulates real-time performance monitoring for a Durban-based call center using Python, SQL Server, and Power BI. Generates synthetic call data with 75% resolved calls and visualizes performance metrics in a live dashboard.  

---

**PREREQUISITES**  
*Software Requirements*  
- Microsoft SQL Server  
- SQL Server Management Studio (SSMS)  
- Power BI Desktop  
- Jupyter Notebook  

*Python Libraries*  
datetime, time, random, pyodbc

---

**DATABASE SETUP**  
*Database Name*  
Call_Center_031  

*Table Structure*  
Run these SQL commands in SSMS:  

CREATE TABLE Locations (LocationID INT PRIMARY KEY, LocationName NVARCHAR(50), City NVARCHAR(20) DEFAULT 'Durban');
CREATE TABLE Clients (ClientID INT PRIMARY KEY, ClientName NVARCHAR(50), Phone NVARCHAR(15));
CREATE TABLE Issues (IssueID INT PRIMARY KEY, IssueName NVARCHAR(100));
CREATE TABLE CSR (CSRID INT PRIMARY KEY, CSRName NVARCHAR(50));
CREATE TABLE Status (StatusID INT PRIMARY KEY, Title NVARCHAR(20));
CREATE TABLE Calls (
    CallID INT IDENTITY(1,1) PRIMARY KEY,
    CallDateTime DATETIME DEFAULT GETDATE(),
    LocationID INT FOREIGN KEY REFERENCES Locations(LocationID),
    ClientID INT FOREIGN KEY REFERENCES Clients(ClientID),
    IssueID INT FOREIGN KEY REFERENCES Issues(IssueID),
    CSRID INT FOREIGN KEY REFERENCES CSR(CSRID),
    ResponseTime INT,
    CallTime INT,
    StatusID INT FOREIGN KEY REFERENCES Status(StatusID),
    Rating INT
);


*Static Data Population*  
Import reference data (Locations, Clients, Issues, CSR, Status) from included Excel templates using SSMS Import Wizard. 
[Excel File](https://www.youtube.com/redirect?event=comments&redir_token=QUFFLUhqa212VUVEeUYzRHRhS1M4WEh0bEdzQ2ZMODFfUXxBQ3Jtc0tsMll5d2QxWTNQdVVycXhfVVpKSXBRbXRMZklvRkdXYjEwbDgzd3RhdkNzaG1pYm16Q1VDTE5pNU5QMEVaZ0FuMmQwX3ltN3Z6dC15YlVfTFgxV3lQREl3Ukc0dnQxVUdtLXNPN0JPT3U4Q2oxcWVnZw&q=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1tTRfVz-g99um2MUN31U-QQuUCj4dwL4v%2Fedit%3Fusp%3Dshare_link%26ouid%3D110287700425339553017%26rtpof%3Dtrue%26sd%3Dtrue)

---

**PYTHON SIMULATION**  
*Script Name*  
call_simulator.py  

*Key Features*  
- Generates 1 call/second  
- 75% bias toward "Resolved" status  
- Auto-populates SQL Server database  

*Connection Configuration*  
   python
conn = pyodbc.connect(
    'DRIVER={ODBC Driver 17 for SQL Server};'
    'SERVER=DESKTOP-BSIRHG7;'
    'DATABASE=Call_Center_031;'
    'Trusted_Connection=yes;'
)


*Status Distribution Logic*  
   python
status_id = random.choices(
    [1, 2, 3, 4],           # 1=Resolved, 2=Referred, 3=Declined, 4=Unresolved
    weights=[0.75, 0.08, 0.08, 0.09], 
    k=1
)[0]


---

**POWER BI DASHBOARD**  
*Connection Settings*  
- Data Source: SQL Server  
- Server: DESKTOP-BSIRHG7  
- Database: Call_Center_031  
- Connectivity Mode: DirectQuery  

*Data Model Relationships*  
- Calls.LocationID → Locations.LocationID  
- Calls.ClientID → Clients.ClientID  
- Calls.IssueID → Issues.IssueID  
- Calls.CSRID → CSR.CSRID  
- Calls.StatusID → Status.StatusID  

*Dashboard Components*  
1. Time Analysis: Line/column chart showing last 30 calls (call time, response time, rating)  
2. Geographic View: Map showing call locations across the globe 
3. Performance Gauges: Average rating, response time, and call duration  
4. Issue Distribution: Donut chart of complaints by issue type  
5. CSR Performance: Donut chart of calls handled per agent  
6. Status Breakdown: Column chart of call resolution status  

*Real-Time Configuration*  
- Set page refresh rate to 1 second in "Page Settings"  

---

**EXECUTION INSTRUCTIONS**  
1. Database Setup  
   - Create tables in SSMS  
   - Import static data from Excel templates  

2. Start Simulation  
   
   python call_simulator.py
   

3. Power BI Dashboard  
   - Open CallCenter_Dashboard.pbix  
   - Observe live data updates  

---

**TROUBLESHOOTING**  
*Common Issues*  
- Connection Errors: Verify SQL Server instance name and authentication  
- Data Not Updating: Ensure Power BI uses DirectQuery mode  
- Biased Data: Adjust weights in Python's random.choices()  
- Delayed Refresh: Check network latency to SQL Server  

---

**KEY METRICS MONITORED**  
- Average Call Rating (1-10 scale)  
- Response Time (seconds)  
- Call Duration (seconds)  
- Resolution Rate (%)  
- Issue Type Distribution  
- Agent Performance  

---

**NOTES**  
- Replace "YOUR_SERVER_NAME" with actual SQL Server instance name  
- Static data templates provided in /data_templates folder  
- Dashboard designed for 1920x1080 resolution

---

## Dashboard
[Call Center 031]
<img width="1366" height="768" alt="Screenshot (9)" src="https://github.com/user-attachments/assets/cde3ec9c-df2f-4cab-85e6-2840ebf58e68" />



# dp--203-lab-19

# **Realtime Data Analytics with Azure Stream Analytics and Power BI**

## **📌 Project Overview**
This project demonstrates how to create a **real-time data pipeline** using:
- **Azure Event Hubs** (data ingestion)
- **Azure Stream Analytics** (stream processing)
- **Power BI** (realtime visualization)

**Scenario:** Simulate an e-commerce platform processing live sales orders with a dashboard updating every 5 seconds.

---

## **🛠️ Technologies Used**
| Service | Purpose |
|---------|---------|
| **Azure Event Hubs** | Ingests streaming order data |
| **Azure Stream Analytics** | Processes and aggregates data in real-time |
| **Power BI** | Visualizes live order metrics |

---

## **🚀 Key Features**
- **Real-time aggregation** of sales data (5-second windows)
- **Power BI dashboard** with auto-refreshing line chart
- **End-to-end serverless architecture**

---

## **⚡ Setup Steps**
### **1. Resource Provisioning**
```powershell
git clone https://github.com/MicrosoftLearning/dp-203-azure-data-engineer
cd dp-203/Allfiles/labs/19
./setup.ps1
```
- Creates:
  - Azure Synapse workspace
  - Event Hubs namespace
  - Required storage accounts

### **2. Power BI Workspace**
1. Create a new workspace in [Power BI Service](https://app.powerbi.com/)
2. Note the workspace GUID (from URL: `https://app.powerbi.com/groups/<GUID>/list`)

### **3. Stream Analytics Job**
1. Create a Stream Analytics job (`stream-orders`)
2. Configure inputs/outputs:
   - **Input:** Event Hub (`orders`)
   - **Output:** Power BI (`realtime-data` dataset)

### **4. Streaming Query**
```sql
SELECT
    DateAdd(second,-5,System.TimeStamp) AS StartTime,
    System.TimeStamp AS EndTime,
    ProductID,
    SUM(Quantity) AS Orders
INTO [powerbi-dataset]
FROM [orders] TIMESTAMP BY EventEnqueuedUtcTime
GROUP BY ProductID, TumblingWindow(second, 5)
HAVING COUNT(*) > 1
```

---

## **📊 Power BI Dashboard**
1. Create dashboard: **Order Tracking**
2. Add tile: **Custom Streaming Data**
3. Configuration:
   - Visualization: `Line chart`
   - Axis: `EndTime`
   - Values: `Orders`
   - Time window: `1 Minute`

---

## **🔌 Data Simulation**
Run the order simulator (sends 100 test orders):
```bash
node ~/dp-203/Allfiles/labs/19/orderclient
```
**Expected Output:**  
Dashboard updates automatically every 5 seconds.

---

## **🧹 Cleanup**
1. Delete Power BI workspace
2. Stop Stream Analytics job
3. Delete resource group:
```powershell
az group delete --name dp203-xxxxxxx
```

---

## **💡 Key Learnings**
✅ **Real-time pipeline design** with Event Hubs → Stream Analytics → Power BI  
✅ **Tumbling windows** for time-based aggregations  
✅ **Power BI streaming datasets** configuration  

---
![LAB-19](https://github.com/user-attachments/assets/b1f73f8e-2064-48e1-8176-e86aa62fe321)
[LAB-19.pdf](https://github.com/user-attachments/files/19835716/LAB-19.pdf)



## **📚 Resources**
- [Stream Analytics Documentation](https://docs.microsoft.com/en-us/azure/stream-analytics/)
- [Power BI Real-time Dashboards](https://learn.microsoft.com/en-us/power-bi/connect-data/service-real-time-streaming)

---

**✨ Pro Tip:**  
For production scenarios, consider:
- Adding alerts on anomalous orders
- Using **Reference Data** in Stream Analytics for product lookups
- Implementing **Azure Functions** for complex event processing

#Azure #StreamAnalytics #PowerBI #RealtimeAnalytics #DataEngineering

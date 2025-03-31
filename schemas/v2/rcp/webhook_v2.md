# Modexpress MX Connect Receipt Orders v2 Webhook configuration

**Webhook "Receipt Orders"**

We are introducing a **new webhook variant** alongside the current version. The new webhook uses the `rcp.phase.update` action instead of `rcp.status.update`. We **strongly advise using the new version**, as the old webhook may not provide sufficient detail about the receipt status. For example, an order may be reported as closed while customs activities are still pending. In such cases, stock might not yet be available for ordering, which could result in rejected orders due to unavailable inventory.

---

**Webhook Payload**

The Modexpress Receipt Orders webhook allows you to stay informed about order status changes. When the `rcp.phase.update` event is triggered and you have subscribed to this event, we’ll send a `HTTP POST` payload to your configured webhook URL. The webhook payload for receipt orders contains the following properties:

| **Key**                       | **Type** | **Description**                                                                      |
| ----------------------------- | -------- | ------------------------------------------------------------------------------------ |
| action                        | string   | The specific activity that triggered the event. New version uses `rcp.phase.update`  |
| payload                       | object   | Container for the payload                                                            |
| payload.Id                    | string   | The id of the order for which the status was updated                                 |
| payload.Type                  | string   | Designation of the order type: either `PURCHASE` or `RETURN`                         |
| payload.ProcessingStatus      | object   | Container for the receipt order status detail information                            |
| payload.ProcessingStatus.Id   | number   | The numeric status Id (see table below)                                              |
| payload.ProcessingStatus.Name | string   | The receipt order status description                                                 |

---

**Processing status Id field values**

| **Processing status Id** | **Name**                           | **Description**                                                                 |
|--------------------------|------------------------------------|---------------------------------------------------------------------------------|
| 10                       | Concept order                      | Initial draft of the order                                                      |
| 12                       | Concept, shipment assigned         | Shipment has been assigned to the concept order                                |
| 20                       | New order                          | Order has been created and registered                                          |
| 22                       | New order, shipment assigned       | Shipment has been assigned to the new order                                    |
| 30                       | Unloading requested                | Request for unloading has been sent                                            |
| 32                       | Unloading in progress              | Goods are currently being unloaded                                             |
| 34                       | Unloading report sent to customs   | Report of unloading has been sent to customs authorities                       |
| 36                       | Unloading completed                | Unloading has finished                                                         |
| 40                       | Awaiting receiving                 | Waiting to begin receiving process                                             |
| 42                       | Receiving in progress              | Items are currently being received                                             |
| 44                       | Receiving completed                | All goods have been successfully received                                      |
| 50                       | Processing declaration             | Customs declaration is being processed                                         |
| 52                       | Declaration accepted               | Customs declaration has been accepted                                          |
| 54                       | Awaiting customs clearance         | Waiting for customs to release the goods                                       |
| 60                       | Goods released                     | Goods have been cleared by customs                                             |
| 62                       | Goods released, customer notified  | Goods released and customer has been informed                                  |

---

**Webhook payload example (new version)**

```json
{
  "action": "rcp.phase.update",
  "payload": {
    "Id": "123456",
    "Type": "PURCHASE",
    "ProcessingStatus": {
      "Id": 42,
      "Name": "Receiving in progress"
    }
  }
}
```

---

**Configuration**

You can subscribe to inbound order phase update events. These webhooks can be registered on both the test and production environments. Please contact your Modexpress representative to register your webhook URL and authentication token (Bearer token).

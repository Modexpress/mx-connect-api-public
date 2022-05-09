# Modexpress MX Connect Receipt Orders v2 Webhook configuration

**Webhook "Receipt Orders"**

The Modexpress Receipt Orders webhook allows you to stay informed about order status changes. When the `rcp.status.update` event is triggered and you have subscribed to this event update, we&#39;ll send a `HTTP POST` payload to the webhook&#39;s configured URL. The webhook payload for receipt orders contains the following properties:

| **Key**                       | **Type** | **Description**                                                                      |
| ----------------------------- | -------- | ------------------------------------------------------------------------------------ |
| action                        | string   | The specific activity that triggered the event which defaults to order.status.update |
| payload                       | object   | Container for the payload                                                            |
| payload.Id                    | string   | The id of the order for which the status was updated                                 |
| payload.Type                  | string   | Designation of the order type: either `PURCHASE` or `RETURN`                         |
| payload.ProcessingStatus      | object   | Container for the receipt order status detail information                            |
| payload.ProcessingStatus.Id   | string   | The status Id (see table below)                                                      |
| payload.ProcessingStatus.Name | string   | The receipt order status description                                                 |

---

**Processing status Id field values**

| **Processing status Id**  | **Name**    | **Description** |
| --------------------------| ----------- |------------------
| N                         | New         | New, not yet started
| P                         | In Progress | In progress, receipt has started
| C                         | Closed   | Closed, receipt has been closed

---

**Webhook payload example**

```json
{
  "action": "rcp.status.update",
  "payload": {
    "Id": "123456",
    "Type": "PURCHASE",
    "ProcessingStatus": {
      "Id": "I",
      "Name": "In progress"
    }
  }
}
```

---

**Configuration**

You can subscribe to outbound order status update events. This webhooks can be registered on the test and production server. Please contact your Modexpress representative to register your webhook URL and secret (Bearer token).

# Modexpress MX Connect Outbound Orders Webhook configuration

**Webhook "Outbound Orders"**

The Modexpress Outbound Orders webhook allows you to stay informed about order status changes. When the `shp.status.update` event is triggered and you have subscribed to this event update, we&#39;ll send a `HTTP POST` payload to the webhook&#39;s configured URL. The webhook payload for outbound orders contains the following properties:

| **Key**                       | **Type** | **Description**                                                                      |
| ----------------------------- | -------- | ------------------------------------------------------------------------------------ |
| action                        | string   | The specific activity that triggered the event which defaults to order.status.update |
| payload                       | object   | Container for the payload                                                            |
| payload.Id                    | string   | The id of the order for which the status was updated                                 |
| payload.ProcessingStatus      | object   | Container for the order status detail information                                    |
| payload.ProcessingStatus.Id   | string   | The status Id (see table below)                                                      |
| payload.ProcessingStatus.Name | string   | The order status description                                                         |

---

**Status field values**

| **Processing status Id**  | **Name**    |
| --------------------------| ----------- |
| N                         | New         |
| I                         | In progress |
| R                         | Released    |
| K                         | Picked      |
| P                         | Packed      |
| S                         | Shipped     |
| C                         | Cancelled   |

---

**Webhook payload example**

```json
{
  "action": "shp.status.update",
  "payload": {
    "Id": "123456",
    "ProcessingStatus": {
      "Id": "I",
      "Name": "In progress"
    }
  }
}
```

**Configuration**

You can subscribe to outbound order status update events. This webhooks can be registered on the test and production server. Please contact your Modexpress representative to register your webhook URL and secret (Bearer token).

**Testing**

To verify if the implementation of your webhook is set up correctly, you should place a new order via a Modexpress SalesKit API shp `POST` request. Upon receipt, the order status will be initialized with `New`. The status will automatically be set to `In progress` once the request is successfully processed. This update triggers a webhook event.

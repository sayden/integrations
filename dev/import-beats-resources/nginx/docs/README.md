# Nginx Integration

This integration periodically fetches metrics from [https://nginx.org/](Nginx) servers. It can parse access and error
logs created by the HTTP server. 

## Compatibility

The Nginx `stubstatus` metrics was tested with Nginx 1.9 and are expected to work with all version >= 1.9.
The logs were tested with version 1.10.
On Windows, the module was tested with Nginx installed from the Chocolatey repository.

## Logs

**Timezone support**

This datasource parses logs that don’t contain timezone information. For these logs, the Elastic Agent reads the local
timezone and uses it when parsing to convert the timestamp to UTC. The timezone to be used for parsing is included
in the event in the `event.timezone` field.

To disable this conversion, the event.timezone field can be removed with the drop_fields processor.

If logs are originated from systems or applications with a different timezone to the local one, the `event.timezone`
field can be overwritten with the original timezone using the add_fields processor.

### Access Logs

Access logs collects the nginx access logs.

{{fields "access"}}

### Error Logs

Error logs collects the nginx error logs.

{{fields "error"}}

### Ingress Controller Logs

Error logs collects the ingress controller logs.

{{fields "ingress_controller"}}

## Metrics

### Stub Status Metrics

The Nginx stubstatus stream collects data from the Nginx `ngx_http_stub_status` module. It scrapes the server status
data from the web page generated by ngx_http_stub_status.

This is a default stream. If the host datasource is unconfigured, this stream is enabled by default.

An example event for nginx looks as following:

```$json
{
   "@timestamp":"2020-04-28T11:07:58.223Z",
   "service":{
      "type":"nginx",
      "address":"127.0.0.1:8081"
   },
   "nginx":{
      "stubstatus":{
         "waiting":0,
         "hostname":"127.0.0.1:8081",
         "dropped":0,
         "writing":1,
         "handled":7339,
         "requests":7411,
         "reading":0,
         "accepts":7339,
         "current":10,
         "active":1
      }
   },
   "stream":{
      "namespace":"default",
      "type":"metrics",
      "dataset":"nginx.stubstatus"
   },
   "ecs":{
      "version":"1.5.0"
   },
   "agent":{
      "type":"metricbeat",
      "ephemeral_id":"8eb07b4f-df58-4794-8e00-60f1443f33b6",
      "hostname":"MacBook-Elastic.local",
      "id":"e47f6e4d-5277-46f3-801d-221c7584c604",
      "version":"8.0.0"
   },
   "event":{
      "module":"nginx",
      "duration":1112095,
      "dataset":"nginx.stubstatus"
   },
   "metricset":{
      "period":10000,
      "name":"stubstatus"
   }
}
```

{{fields "stubstatus"}}
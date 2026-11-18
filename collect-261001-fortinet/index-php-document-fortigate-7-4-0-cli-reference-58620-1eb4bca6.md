# config system session-ttl

Configure global session TTL timers for this FortiGate.

```
config system session-ttl
    Description: Configure global session TTL timers for this FortiGate.
    set default {user}
    config port
        Description: Session TTL port.
        edit <id>
            set protocol {integer}
            set start-port {integer}
            set end-port {integer}
            set timeout {user}
        next
    end
end
```
                                            
                                            | Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| default | Default timeout. | user | Not Specified |  | 

| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| id | Table entry ID. | integer | Minimum value: 0 Maximum value: 65535 | 0 | 
| protocol | Protocol. | integer | Minimum value: 0 Maximum value: 255 | 0 | 
| start-port | Start port number. | integer | Minimum value: 0 Maximum value: 65535 | 0 | 
| end-port | End port number. | integer | Minimum value: 0 Maximum value: 65535 | 0 | 
| timeout | Session timeout (TTL). | user | Not Specified |  |
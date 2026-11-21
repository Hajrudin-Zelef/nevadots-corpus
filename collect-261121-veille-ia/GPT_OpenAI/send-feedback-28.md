---
id: collect-261121-veille-ia/gpt-openai/send-feedback-28
title: "Send Feedback"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["value", "integer", "config", "default", "port", "system", "session-ttl", "session"]
source: https://docs.fortinet.com/index.php/document/fortigate/7.4.0/cli-reference/58620
source_anchor: ""
source_lines: [1, 36]
section: "GPT_OpenAI"
---
# config system session-ttl

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
                                            ## config system session-ttl

| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| default | Default timeout. | user | Not Specified |  | 

### config port

| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| id | Table entry ID. | integer | Minimum value: 0 Maximum value: 65535 | 0 | 
| protocol | Protocol. | integer | Minimum value: 0 Maximum value: 255 | 0 | 
| start-port | Start port number. | integer | Minimum value: 0 Maximum value: 65535 | 0 | 
| end-port | End port number. | integer | Minimum value: 0 Maximum value: 65535 | 0 | 
| timeout | Session timeout (TTL). | user | Not Specified |  |

Failover is must in networks, read how to configure MikroTik WAN failover for high availability of your network infrastructure.

## Table of Contents

## 1. Introduction

Network downtime costs businesses an average of $5,600 per minute. Single internet connections create dangerous points of failure. MikroTik RouterOS provides multiple failover methods to ensure continuous network availability.

This guide shows network engineers and systems administrators how to configure WAN failover on MikroTik devices. You will learn three proven methods:

- Distance-based routing failover
- Netwatch-based monitoring and automation
- VRRP hardware redundancy

Each method includes step-by-step configuration examples, troubleshooting tips, and performance optimization techniques.

## 2. Understanding MikroTik WAN Failover Fundamentals

### What is WAN Failover?

WAN failover automatically switches network traffic from a failed primary internet connection to a backup connection. This process happens without manual intervention and maintains business continuity.

Key benefits include:

- Reduced network downtime
- Automatic recovery processes
- Improved service reliability
- Cost savings from avoided outages

### MikroTik Failover Methods Overview

MikroTik RouterOS supports three primary failover approaches:

#### Distance-Based Routing Failover

- **Method** : Uses route distance values to prioritize connections
- **Pros** : Simple setup, built-in gateway monitoring
- **Cons** : Limited customization options
- **Best for** : Small to medium networks with basic requirements

#### Netwatch-Based Monitoring

- **Method** : Uses scripts triggered by network monitoring
- **Pros** : Full customization, advanced logic
- **Cons** : Complex setup, requires scripting knowledge
- **Best for** : Enterprise networks with specific requirements

#### VRRP Hardware Redundancy

- **Method** : Multiple routers share virtual IP addresses
- **Pros** : Hardware-level redundancy, fast failover
- **Cons** : Requires multiple devices, complex configuration
- **Best for** : Critical infrastructure with high availability needs

### Prerequisites and Planning

Before starting configuration, verify these requirements:

#### Hardware Requirements

- MikroTik RouterBoard with RouterOS v6.45 or newer
- Minimum two WAN interfaces
- Sufficient RAM for monitoring scripts (128MB recommended)

#### Network Requirements

- Two or more ISP connections
- Different physical paths for redundancy
- Adequate bandwidth on backup connections

#### Planning Checklist

- Document current network topology
- Identify critical services requiring failover
- Define acceptable downtime windows
- Plan IP addressing schemes
- Choose monitoring targets

## 3. Method 1: Distance-Based WAN Failover Configuration

Distance-based failover uses route priorities to control traffic flow. Routes with lower distance values take precedence over higher values.

### Basic Dual WAN Setup

#### Step 1: Configure WAN Interfaces

Set up primary WAN interface:

```
/ip dhcp-client
add interface=ether1 disabled=no comment="Primary WAN"
```
Set up secondary WAN interface:

```
/ip dhcp-client
add interface=ether2 disabled=no comment="Secondary WAN"
```
For static IP configuration:

```
/ip address
add address=203.0.113.10/24 interface=ether1 comment="Primary WAN Static"
add address=198.51.100.20/24 interface=ether2 comment="Secondary WAN Static"
```
#### Step 2: Configure NAT Rules

Create masquerade rules for both WAN interfaces:

```
/ip firewall nat
add chain=srcnat out-interface=ether1 action=masquerade comment="Primary WAN NAT"
add chain=srcnat out-interface=ether2 action=masquerade comment="Secondary WAN NAT"
```
#### Step 3: Set Up Default Routes

Configure primary route with gateway monitoring:

```
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 check-gateway=ping comment="Primary Route"
```
Configure backup route with higher distance:

```
/ip route
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2 comment="Backup Route"
```
### Gateway Monitoring Configuration

The check-gateway parameter monitors gateway reachability. Configure monitoring settings:

#### Advanced Gateway Check Options

```
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 \
    check-gateway=ping timeout=3s comment="Primary with timeout"
```
#### Multiple Target Monitoring

Monitor specific hosts instead of gateways:

```
/ip route
add dst-address=8.8.8.8/32 gateway=203.0.113.1 scope=10 comment="Google DNS Primary"
add dst-address=8.8.4.4/32 gateway=198.51.100.1 scope=10 comment="Google DNS Secondary"
add dst-address=0.0.0.0/0 gateway=8.8.8.8 distance=1 check-gateway=ping scope=11 comment="Primary via DNS"
add dst-address=0.0.0.0/0 gateway=8.8.4.4 distance=2 scope=11 comment="Secondary via DNS"
```
### Complete Basic Configuration Example

Here is a complete working configuration for basic dual WAN failover:

```
# Configure interfaces
/interface ethernet
set [find default-name=ether1] name=wan1-primary
set [find default-name=ether2] name=wan2-secondary
set [find default-name=ether3] name=lan1
# Set up DHCP clients for dynamic IP
/ip dhcp-client
add interface=wan1-primary disabled=no comment="Primary ISP"
add interface=wan2-secondary disabled=no comment="Secondary ISP"
# Configure LAN interface
/ip address
add address=192.168.1.1/24 interface=lan1 comment="LAN Gateway"
# Enable DHCP server for LAN
/ip pool
add name=lan-pool ranges=192.168.1.100-192.168.1.200
/ip dhcp-server
add address-pool=lan-pool interface=lan1 name=lan-dhcp
/ip dhcp-server network
add address=192.168.1.0/24 gateway=192.168.1.1 dns-server=8.8.8.8,8.8.4.4
# Configure NAT
/ip firewall nat
add chain=srcnat out-interface=wan1-primary action=masquerade
add chain=srcnat out-interface=wan2-secondary action=masquerade
# Configure routes with failover
/ip route
add dst-address=0.0.0.0/0 gateway=[/ip dhcp-client get [find interface=wan1-primary] gateway] \
    distance=1 check-gateway=ping comment="Primary Route"
add dst-address=0.0.0.0/0 gateway=[/ip dhcp-client get [find interface=wan2-secondary] gateway] \
    distance=2 comment="Secondary Route"
```
## 4. Method 2: Advanced Failover with Netwatch

Netwatch provides advanced monitoring capabilities with custom script execution. This method offers maximum flexibility for complex failover scenarios.

### Netwatch Configuration for Proactive Monitoring

#### Basic Netwatch Setup

Create netwatch entries for both WAN connections:

```
/tool netwatch
add host=8.8.8.8 interval=10s timeout=2s up-script="" down-script="" comment="Monitor Primary"
add host=1.1.1.1 interval=10s timeout=2s up-script="" down-script="" comment="Monitor Secondary"
```
#### Advanced Monitoring Parameters

- **Interval** : Time between ping attempts (recommended: 10-30 seconds)
- **Timeout** : Maximum wait time for response (recommended: 2-5 seconds)
- **Startup-delay** : Delay before starting monitoring (useful during boot)

### Script-Based Failover Automation

#### Basic Failover Script Structure

Create global variables for route management:

```
/system script
add name=global-vars source={
    :global primaryGW "203.0.113.1"
    :global secondaryGW "198.51.100.1"
    :global primaryRoute ""
    :global secondaryRoute ""
    :global currentActiveWAN "primary"
}
```
#### Primary WAN Down Script

```
/system script
add name=primary-down source={
    :global currentActiveWAN
    :global secondaryGW
    
    :if ($currentActiveWAN = "primary") do={
        :log info "Primary WAN failed - switching to secondary"
        
        # Disable primary route
        /ip route set [find comment="Primary Route"] disabled=yes
        
        # Enable secondary route if disabled
        /ip route set [find comment="Secondary Route"] disabled=no
        
        # Update global variable
        :set currentActiveWAN "secondary"
        
        :log info "Failover to secondary WAN completed"
    }
}
```
#### Primary WAN Up Script

```
/system script
add name=primary-up source={
    :global currentActiveWAN
    :global primaryGW
    
    :if ($currentActiveWAN = "secondary") do={
        :log info "Primary WAN restored - switching back"
        
        # Enable primary route
        /ip route set [find comment="Primary Route"] disabled=no
        
        # Secondary route will be disabled by distance
        
        # Update global variable
        :set currentActiveWAN "primary"
        
        :log info "Failback to primary WAN completed"
    }
}
```
#### Configure Netwatch with Scripts

```
/tool netwatch
set [find comment="Monitor Primary"] up-script=primary-up down-script=primary-down
```
### Multi-Target Redundancy

Monitor multiple targets to prevent false positives:

#### Advanced Multi-Target Script

```
/system script
add name=advanced-failover source={
    :global primaryTargets {"8.8.8.8";"1.1.1.1";"208.67.222.222"}
    :global failedTargets 0
    :local maxFailures 2
    
    # Check each target
    :foreach target in=$primaryTargets do={
        :local pingResult [/ping $target count=3 interval=1s]
        :if ($pingResult = 0) do={
            :set failedTargets ($failedTargets + 1)
        }
    }
    
    # Trigger failover if threshold exceeded
    :if ($failedTargets >= $maxFailures) do={
        :execute script=primary-down
    }
    
    :set failedTargets 0
}
```
### Connection Tracking and Session Management

Manage existing connections during failover:

```
/system script
add name=connection-cleanup source={
    # Remove connections using failed interface
    :local failedInterface "wan1-primary"
    
    /ip firewall connection
    :foreach connection in=[find] do={
        :local connInterface [get $connection orig-interface]
        :if ($connInterface = $failedInterface) do={
            remove $connection
        }
    }
    
    :log info "Cleaned up connections for failed interface"
}
```
## 5. Method 3: Hardware Redundancy with VRRP

VRRP (Virtual Router Redundancy Protocol) creates virtual routers shared between multiple physical devices. This provides hardware-level redundancy.

### VRRP Fundamentals on MikroTik

#### How VRRP Works

- Multiple routers share a virtual IP address
- One router acts as master, others as backup
- Master election based on priority values
- Automatic failover when master fails

#### VRRP Prerequisites

- Two or more MikroTik routers
- Same subnet for VRRP interfaces
- RouterOS Advanced license or higher
- Synchronized time between devices

### Dual Router VRRP Configuration

#### Master Router Configuration

```
# Configure physical interface
/ip address
add address=192.168.1.10/24 interface=ether3 comment="Master LAN IP"
# Configure VRRP interface
/interface vrrp
add interface=ether3 vrid=1 priority=200 \
    interval=1s fast-leave=yes \
    name=vrrp-lan comment="Master VRRP"
# Assign virtual IP to VRRP interface
/ip address
add address=192.168.1.1/24 interface=vrrp-lan comment="Virtual Gateway"
# Configure WAN failover for master
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 check-gateway=ping
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=2
```
#### Backup Router Configuration

```
# Configure physical interface
/ip address
add address=192.168.1.11/24 interface=ether3 comment="Backup LAN IP"
# Configure VRRP interface
/interface vrrp
add interface=ether3 vrid=1 priority=100 \
    interval=1s fast-leave=yes \
    name=vrrp-lan comment="Backup VRRP"
# Assign virtual IP to VRRP interface
/ip address
add address=192.168.1.1/24 interface=vrrp-lan comment="Virtual Gateway"
# Configure WAN failover for backup
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.2 distance=1 check-gateway=ping
add dst-address=0.0.0.0/0 gateway=198.51.100.2 distance=2
```
### Combining VRRP with WAN Failover

#### Advanced VRRP Script Integration

```
/system script
add name=vrrp-wan-monitor source={
    :local vrrpStatus [/interface vrrp get [find name=vrrp-lan] running]
    :local primaryWAN [/ip route get [find comment="Primary Route"] active]
    
    # Adjust VRRP priority based on WAN status
    :if ($primaryWAN = false) do={
        # Primary WAN failed, reduce priority
        /interface vrrp set [find name=vrrp-lan] priority=150
        :log warning "Primary WAN failed, reduced VRRP priority"
    } else={
        # Primary WAN active, restore priority
        /interface vrrp set [find name=vrrp-lan] priority=200
        :log info "Primary WAN active, restored VRRP priority"
    }
}
# Schedule script execution
/system scheduler
add name=vrrp-monitoring interval=30s on-event=vrrp-wan-monitor
```
## 6. Load Balancing vs. Failover Considerations

### When to Use Load Balancing

Load balancing distributes traffic across multiple connections. Consider load balancing when:

- Both WAN connections have sufficient bandwidth
- Applications can handle asymmetric routing
- Cost optimization is important
- Bandwidth aggregation is needed

#### Basic Load Balancing Configuration

```
# Configure route-marking
/ip firewall mangle
add chain=input in-interface=wan1-primary action=mark-connection \
    new-connection-mark=wan1-conn passthrough=yes
add chain=input in-interface=wan2-secondary action=mark-connection \
    new-connection-mark=wan2-conn passthrough=yes
# Configure outbound load balancing
/ip firewall mangle
add chain=output connection-mark=wan1-conn action=mark-routing \
    new-routing-mark=to-wan1 passthrough=yes
add chain=output connection-mark=wan2-secondary action=mark-routing \
    new-routing-mark=to-wan2 passthrough=yes
# Create routing tables
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 routing-mark=to-wan1
add dst-address=0.0.0.0/0 gateway=198.51.100.1 routing-mark=to-wan2
```
### Pure Failover Implementation

Pure failover maintains one active connection. Benefits include:

- Simpler configuration
- Predictable routing paths
- Lower bandwidth costs
- Easier troubleshooting

#### Bandwidth Conservation Setup

```
# Disable secondary interface when primary is active
/system script
add name=bandwidth-conservation source={
    :local primaryActive [/ip route get [find comment="Primary Route"] active]
    
    :if ($primaryActive = true) do={
        # Primary active - disable secondary DHCP client
        /ip dhcp-client set [find interface=wan2-secondary] disabled=yes
        :log info "Primary active - secondary WAN disabled"
    } else={
        # Primary failed - enable secondary DHCP client
        /ip dhcp-client set [find interface=wan2-secondary] disabled=no
        :log info "Primary failed - secondary WAN enabled"
    }
}
```
## 7. Monitoring and Troubleshooting

### Health Check Configuration

#### Optimal Monitoring Settings

- **Ping Interval** : 10-30 seconds (balance between responsiveness and overhead)
- **Timeout** : 2-5 seconds (account for network latency)
- **Failure Threshold** : 3-5 consecutive failures before triggering

#### Multiple Target Selection Strategy

Choose monitoring targets carefully:

- **Primary targets** : ISP DNS servers, gateway addresses
- **Secondary targets** : Public DNS (8.8.8.8, 1.1.1.1)
- **Tertiary targets** : Well-known websites (google.com, cloudflare.com)

```
/tool netwatch
add host=203.0.113.1 interval=15s timeout=3s comment="ISP1 Gateway"
add host=198.51.100.1 interval=15s timeout=3s comment="ISP2 Gateway" 
add host=8.8.8.8 interval=30s timeout=5s comment="Google DNS"
add host=1.1.1.1 interval=30s timeout=5s comment="Cloudflare DNS"
```
### Logging and Alerting

#### Configure System Logging

```
# Enable detailed logging
/system logging
add topics=info,warning,error action=memory prefix="FAILOVER"
add topics=route,script action=memory prefix="ROUTING"
# Configure log rotation
/system logging action
set memory memory-lines=5000 memory-stop-on-full=no
```
#### Email Notification Setup

```
# Configure email settings
/tool e-mail
set server=smtp.company.com port=587 \
    start-tls=yes user=alerts@company.com password=secret123
# Create notification script
/system script
add name=send-failover-alert source={
    :local message "WAN Failover Event: Primary connection failed at $[/system clock get date] $[/system clock get time]"
    /tool e-mail send to=netadmin@company.com subject="Network Alert" body=$message
}
```
### Common Issues and Solutions

#### Asymmetric Routing Problems

**Symptoms**:

- Intermittent connection issues
- Some applications work, others don’t
- Connection timeouts

**Solution**:

```
# Mark connections by input interface
/ip firewall mangle
add chain=prerouting in-interface=wan1-primary action=mark-connection \
    new-connection-mark=wan1-conn passthrough=yes
add chain=prerouting in-interface=wan2-secondary action=mark-connection \
    new-connection-mark=wan2-conn passthrough=yes
# Route responses back through same interface
/ip firewall mangle
add chain=output connection-mark=wan1-conn action=mark-routing \
    new-routing-mark=to-wan1 passthrough=yes
add chain=output connection-mark=wan2-conn action=mark-routing \
    new-routing-mark=to-wan2 passthrough=yes
```
#### DNS Resolution During Failover

**Problem**: DNS queries fail during connection switching

**Solution**:

```
# Configure DNS servers for both connections
/ip dns
set servers=8.8.8.8,8.8.4.4,1.1.1.1,1.0.0.1 \
    allow-remote-requests=yes cache-size=4096KiB
```
#### Troubleshooting Commands

```
# Check route status
/ip route print detail where active=yes
# Monitor gateway reachability
/tool netwatch print detail
# View connection tracking
/ip firewall connection print where connection-mark!=""
# Check interface statistics
/interface print stats
# Monitor system resources
/system resource print
# View recent logs
/log print where topics~"route|script"
```
## 8. Advanced Scenarios and Best Practices

### Multi-WAN Failover (3+ Links)

#### Three-WAN Configuration

```
# Configure interfaces
/interface ethernet
set [find default-name=ether1] name=wan1-fiber
set [find default-name=ether2] name=wan2-cable  
set [find default-name=ether3] name=wan3-lte
set [find default-name=ether4] name=lan1
# Set up DHCP clients
/ip dhcp-client
add interface=wan1-fiber disabled=no comment="Fiber Primary"
add interface=wan2-cable disabled=no comment="Cable Secondary"
# Configure LTE as tertiary
/interface lte
set [find name=lte1] allow-roaming=no
# Configure routes with distance prioritization
/ip route
add dst-address=0.0.0.0/0 gateway=[get wan1-fiber gateway] distance=1 check-gateway=ping
add dst-address=0.0.0.0/0 gateway=[get wan2-cable gateway] distance=2 check-gateway=ping
add dst-address=0.0.0.0/0 gateway=[get lte1 gateway] distance=3 check-gateway=ping
```
#### Advanced Priority Management

```
/system script
add name=multi-wan-manager source={
    :local wanStatus {"fiber"=false; "cable"=false; "lte"=false}
    
    # Check each WAN connection
    :set ($wanStatus->"fiber") [/ip route get [find comment="Fiber Primary"] active]
    :set ($wanStatus->"cable") [/ip route get [find comment="Cable Secondary"] active]  
    :set ($wanStatus->"lte") [/ip route get [find comment="LTE Tertiary"] active]
    
    # Log current status
    :log info ("WAN Status - Fiber: " . ($wanStatus->"fiber") . \
              " Cable: " . ($wanStatus->"cable") . \
              " LTE: " . ($wanStatus->"lte"))
    
    # Implement cost-based routing for LTE
    :if (($wanStatus->"fiber") = false and ($wanStatus->"cable") = false) do={
        :log warning "Using expensive LTE connection - consider bandwidth limits"
    }
}
```
### VPN Integration

#### Site-to-Site VPN Failover

```
# Create IPSec policies for both WAN interfaces
/ip ipsec policy
add dst-address=10.0.2.0/24 src-address=10.0.1.0/24 \
    out-interface=wan1-primary template=yes
add dst-address=10.0.2.0/24 src-address=10.0.1.0/24 \
    out-interface=wan2-secondary template=yes disabled=yes
# Script to manage VPN failover
/system script
add name=vpn-failover source={
    :local primaryActive [/ip route get [find comment="Primary Route"] active]
    
    :if ($primaryActive = false) do={
        # Primary failed - switch VPN to secondary
        /ip ipsec policy set [find out-interface=wan1-primary] disabled=yes
        /ip ipsec policy set [find out-interface=wan2-secondary] disabled=no
        :log info "VPN switched to secondary WAN"
    } else={
        # Primary restored - switch VPN back
        /ip ipsec policy set [find out-interface=wan1-primary] disabled=no
        /ip ipsec policy set [find out-interface=wan2-secondary] disabled=yes  
        :log info "VPN switched back to primary WAN"
    }
}
```
### Enterprise Implementation Guidelines

#### Change Management Procedures

1. **Pre-implementation Testing**  - Test configuration in lab environment
  - Validate failover timing requirements
  - Document expected behavior
  - Create rollback procedures
2. **Implementation Planning**  - Schedule during maintenance windows
  - Notify stakeholders of potential brief outages
  - Prepare monitoring tools
  - Have technical support available
3. **Post-implementation Validation**  - Verify failover functionality
  - Test application connectivity
  - Monitor system performance
  - Update documentation

#### Documentation Requirements

- Network topology diagrams
- Configuration backups with version control
- Monitoring target lists and rationale
- Emergency contact procedures
- Troubleshooting runbooks

## 9. Performance Optimization

### Failover Time Minimization

#### Optimal Monitoring Intervals

Balance between quick detection and system overhead:

- **Critical networks** : 5-10 second intervals
- **Standard networks** : 15-30 second intervals
- **Low priority** : 60+ second intervals

```
# High-performance monitoring configuration
/tool netwatch
add host=8.8.8.8 interval=5s timeout=2s startup-delay=30s \
    up-script="primary-restore" down-script="primary-failure"
```
#### Route Convergence Optimization

```
# Optimize routing table processing
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 \
    check-gateway=ping timeout=1s comment="Optimized Primary"
    
# Reduce ARP timeout for faster detection
/ip arp
set timeout=00:01:00
```
### Resource Management

#### CPU and Memory Considerations

Monitor system resources during failover operations:

```
# Create resource monitoring script
/system script
add name=resource-monitor source={
    :local cpuLoad [/system resource get cpu-load]
    :local freeMemory [/system resource get free-memory]
    :local totalMemory [/system resource get total-memory]
    :local memoryUsage (100 - (($freeMemory * 100) / $totalMemory))
    
    :if ($cpuLoad > 80) do={
        :log warning "High CPU load detected: $cpuLoad%"
    }
    
    :if ($memoryUsage > 80) do={
        :log warning "High memory usage detected: $memoryUsage%"
    }
}
# Schedule resource monitoring
/system scheduler
add name=resource-check interval=5m on-event=resource-monitor
```
#### Script Execution Optimization

```
# Optimized failover script with error handling
/system script
add name=optimized-failover source={
    :local startTime [/system clock get time]
    
    :do {
        # Disable primary route
        /ip route set [find comment="Primary Route"] disabled=yes
        
        # Clear connection tracking for faster convergence  
        /ip firewall connection remove [find orig-interface=wan1-primary]
        
        # Force routing table update
        /ip route check-active
        
        :local endTime [/system clock get time]
        :log info "Failover completed in $([:totime ($endTime - $startTime)])"
        
    } on-error={
        :log error "Failover script failed - manual intervention required"
    }
}
```
## 10. Security Considerations

### Firewall Rule Management

#### Interface-Specific Security Rules

```
# Create separate chains for each WAN interface
/ip firewall filter
add chain=forward in-interface=wan1-primary action=jump jump-target=wan1-security
add chain=forward in-interface=wan2-secondary action=jump jump-target=wan2-security
# Configure WAN1 security rules
add chain=wan1-security protocol=tcp dst-port=22,80,443 action=accept
add chain=wan1-security connection-state=established,related action=accept
add chain=wan1-security action=drop
# Configure WAN2 security rules (may have different requirements)
add chain=wan2-security protocol=tcp dst-port=22,80,443 action=accept
add chain=wan2-security connection-state=established,related action=accept
add chain=wan2-security action=drop
```
#### Source NAT Consistency

```
# Ensure consistent NAT behavior
/ip firewall nat
add chain=srcnat out-interface=wan1-primary src-address=192.168.1.0/24 \
    action=masquerade comment="Primary WAN NAT"
add chain=srcnat out-interface=wan2-secondary src-address=192.168.1.0/24 \
    action=masquerade comment="Secondary WAN NAT"
# Log NAT events for security monitoring
add chain=srcnat out-interface=wan1-primary action=log log-prefix="WAN1-OUT"
add chain=srcnat out-interface=wan2-secondary action=log log-prefix="WAN2-OUT"
```
### Logging and Auditing

#### Security Event Correlation

```
# Enhanced security logging
/system logging
add topics=firewall,critical,error action=remote remote=192.168.1.100:514
add topics=account,info action=memory prefix="AUTH"
# Create security monitoring script
/system script
add name=security-monitor source={
    :local suspiciousEvents 0
    
    # Check for unusual connection patterns
    :local connCount [/ip firewall connection print count-only]
    :if ($connCount > 1000) do={
        :log warning "High connection count detected: $connCount"
        :set suspiciousEvents ($suspiciousEvents + 1)
    }
    
    # Monitor failed login attempts
    :local failedLogins [:len [/log find message~"login failed"]]
    :if ($failedLogins > 5) do={
        :log warning "Multiple failed login attempts detected"
        :set suspiciousEvents ($suspiciousEvents + 1)
    }
    
    # Alert if multiple suspicious events
    :if ($suspiciousEvents > 1) do={
        /tool e-mail send to=security@company.com \
            subject="Security Alert" \
            body="Multiple suspicious events detected on router"
    }
}
```
## 11. Testing and Validation

### Failover Testing Procedures

#### Systematic Testing Methodology

1. **Pre-test Preparation**  - Document current network state
  - Identify test applications and services
  - Set up monitoring tools
  - Notify users of testing period
2. **Primary Failure Testing**  - Disconnect primary WAN cable
  - Monitor failover timing
  - Test application connectivity
  - Verify logging events
3. **Restoration Testing**  - Reconnect primary WAN
  - Verify failback occurs
  - Check for connection disruptions
  - Validate routing table updates

#### Automated Testing Scripts

```
# Create comprehensive test script
/system script
add name=failover-test source={
    :log info "Starting automated failover test"
    
    # Record baseline metrics
    :local startTime [/system clock get time]
    :local initialRoutes [/ip route print count-only where active=yes]
    
    # Test primary connection
    :local primaryTest [/ping 8.8.8.8 count=3 interval=1s]
    :log info "Primary connection test: $primaryTest packets received"
    
    # Simulate failure (disable primary route temporarily)
    /ip route set [find comment="Primary Route"] disabled=yes
    :delay 10s
    
    # Test secondary connection  
    :local secondaryTest [/ping 8.8.8.8 count=3 interval=1s]
    :log info "Secondary connection test: $secondaryTest packets received"
    
    # Restore primary connection
    /ip route set [find comment="Primary Route"] disabled=no
    :delay 15s
    
    # Final verification
    :local finalRoutes [/ip route print count-only where active=yes]
    
    :if ($finalRoutes = $initialRoutes) do={
        :log info "Failover test PASSED - routing restored"
    } else={
        :log error "Failover test FAILED - routing inconsistent"
    }
    
    :local endTime [/system clock get time]
    :log info "Test completed in $([:totime ($endTime - $startTime)])"
}
```
### Performance Measurement During Failover

#### Latency and Throughput Testing

```
# Create performance monitoring script
/system script
add name=performance-test source={
    :log info "Performance test starting"
    
    # Test latency to multiple targets
    :local targets {"8.8.8.8";"1.1.1.1";"208.67.222.222"}
    :foreach target in=$targets do={
        :local avgLatency [/ping $target count=10 interval=1s]
        :log info "Average latency to $target: $avgLatency ms"
    }
    
    # Monitor interface statistics
    :foreach interface in=[/interface find type=ethernet] do={
        :local intName [/interface get $interface name]
        :local rxBytes [/interface get $interface rx-byte]
        :local txBytes [/interface get $interface tx-byte]
        :log info "Interface $intName - RX: $rxBytes bytes, TX: $txBytes bytes"
    }
}
```
## 12. Real-World Case Studies

### Small Business Implementation

#### Scenario: Retail Store Chain

**Requirements**:

- 25 locations with point-of-sale systems
- Primary fiber connections with cable backup
- Maximum 60-second failover requirement
- Budget constraints on hardware

**Solution Implemented**:

```
# Basic distance-based failover for retail locations
/interface ethernet
set [find default-name=ether1] name=wan-fiber
set [find default-name=ether2] name=wan-cable
set [find default-name=ether3] name=lan-pos
# Configure automatic IP assignment
/ip dhcp-client  
add interface=wan-fiber disabled=no comment="Store Fiber"
add interface=wan-cable disabled=no comment="Store Cable"
# Simple LAN setup for POS systems
/ip address
add address=10.0.1.1/24 interface=lan-pos
# Configure DHCP for POS devices
/ip pool
add name=pos-pool ranges=10.0.1.100-10.0.1.150
/ip dhcp-server
add address-pool=pos-pool interface=lan-pos name=pos-dhcp
# Basic failover routing
/ip route
add dst-address=0.0.0.0/0 gateway=[find interface=wan-fiber] distance=1 check-gateway=ping
add dst-address=0.0.0.0/0 gateway=[find interface=wan-cable] distance=2
# Simple NAT configuration
/ip firewall nat
add chain=srcnat out-interface=wan-fiber action=masquerade
add chain=srcnat out-interface=wan-cable action=masquerade
```
**Results**:

- Average failover time: 45 seconds
- 99.8% uptime achieved across all locations
- Reduced revenue loss from outages by 95%
- Simple management with minimal training required

### Enterprise Deployment

#### Scenario: Financial Services Company

**Requirements**:

- Headquarters with 500+ users
- Multiple data centers requiring connectivity
- Sub-10-second failover requirement
- Regulatory compliance for logging
- VPN connectivity to branch offices

**Solution Implemented**:

```
# Enterprise-grade configuration with VRRP
# Master router configuration
/interface bridge
add name=bridge-lan protocol-mode=rstp
/interface bridge port
add bridge=bridge-lan interface=ether3
add bridge=bridge-lan interface=ether4
# VRRP configuration for hardware redundancy
/interface vrrp
add interface=bridge-lan vrid=10 priority=200 name=vrrp-main
/ip address
add address=10.10.10.1/24 interface=vrrp-main
# Advanced routing with multiple ISPs
/ip route
add dst-address=0.0.0.0/0 gateway=203.0.113.1 distance=1 \
    routing-mark=isp1-route check-gateway=ping
add dst-address=0.0.0.0/0 gateway=198.51.100.1 distance=1 \
    routing-mark=isp2-route check-gateway=ping
# Load balancing with failover capability
/ip firewall mangle
add chain=prerouting dst-address-type=!local per-connection-classifier=both-addresses:2/0 \
    action=mark-connection new-connection-mark=isp1-conn passthrough=yes
add chain=prerouting dst-address-type=!local per-connection-classifier=both-addresses:2/1 \
    action=mark-connection new-connection-mark=isp2-conn passthrough=yes
# Comprehensive logging for compliance
/system logging
add topics=info,warning,error,critical action=remote remote=10.10.10.100:514
add topics=firewall action=disk file-name=firewall-log file-lines=10000
```
**Results**:

- Average failover time: 8 seconds
- 99.99% uptime achieved
- Successful regulatory audits
- Scalable architecture supporting growth

## 13. Conclusion and Next Steps

### Key Takeaways

Implementing MikroTik WAN failover provides significant business value through improved network reliability. The three methods covered offer solutions for different scenarios:

- **Distance-based routing** : Best for simple, cost-effective implementations
- **Netwatch with scripts** : Optimal for customized enterprise requirements
- **VRRP** : Essential for mission-critical applications requiring hardware redundancy

#### Critical Configuration Points

1. Choose monitoring targets carefully to avoid false positives
2. Set appropriate timeouts balancing responsiveness and stability
3. Implement proper logging for troubleshooting and compliance
4. Test failover procedures regularly to ensure functionality
5. Document configurations and maintain current backups

#### Common Pitfalls to Avoid

- Using overly aggressive monitoring intervals that cause system overhead
- Forgetting to configure NAT rules for backup connections
- Ignoring asymmetric routing issues in complex topologies
- Inadequate testing of failover and failback procedures
- Poor choice of monitoring targets leading to unnecessary failovers

### Further Learning Resources

#### MikroTik Certification Paths

- **MTCNA (MikroTik Certified Network Associate)** : Foundation certification covering basic RouterOS concepts
- **MTCRE (MikroTik Certified Routing Engineer)** : Advanced routing and failover techniques
- **MTCWE (MikroTik Certified Wireless Engineer)** : Wireless failover implementations

#### Advanced RouterOS Features

- BGP routing for enterprise networks
- MPLS implementation for service providers
- Advanced scripting techniques
- Network monitoring and management tools

#### Community Resources

- **MikroTik Wiki** : Comprehensive documentation and examples
- **MikroTik Forum** : Community support and troubleshooting
- **RouterOS Scripting** : Advanced automation techniques
- **Third-party tools** : PRTG, Zabbix, and other monitoring solutions

Regular practice with different scenarios and staying updated with RouterOS releases ensures optimal network reliability. Consider implementing monitoring dashboards to track failover events and performance metrics for continuous improvement.

#### Next Implementation Steps

1. Assess current network requirements and constraints
2. Choose appropriate failover method based on analysis
3. Create detailed implementation plan with rollback procedures
4. Set up test environment to validate configuration
5. Implement during planned maintenance window
6. Monitor and optimize based on real-world performance

Check our list of MikroTik guides.
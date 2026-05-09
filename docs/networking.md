# Networking

## What It Is

Azure networking connects users, services, and private systems through virtual
networks, DNS, ingress, egress, load balancing, and private connectivity.

## When To Use It

- Use VNets for private address spaces and subnet isolation.
- Use Private Endpoints for private access to PaaS services.
- Use Application Gateway for regional layer 7 ingress and WAF.
- Use Front Door for global HTTP routing, acceleration, and edge WAF.
- Use VPN or ExpressRoute for hybrid connectivity.
- Use NAT Gateway for predictable outbound IPs.

## When Not To Use It

- Do not put everything into a VNet without a routing and DNS model.
- Do not expose PaaS services publicly when private access is required.
- Do not use Network Security Groups as your only application security boundary.
- Do not skip outbound egress planning for partner allowlists.

## Common Patterns

- Hub-spoke network for shared firewall, DNS, and connectivity.
- Private Endpoint for SQL, Storage, Key Vault, and Service Bus.
- API Management internal mode with Application Gateway or Front Door.
- NAT Gateway for stable egress from Functions Premium, App Service VNet
  integration, or Container Apps.

## Common Gotchas

- Private Endpoint requires correct private DNS zones.
- App Service VNet integration controls outbound traffic, not inbound traffic.
- Some services need delegated subnets.
- Forced tunneling can break platform dependencies if not planned.
- IP restrictions can block deployment agents and health probes.

## Security Notes

- Use WAF for internet-facing HTTP workloads.
- Restrict management access and deployment endpoints.
- Keep private DNS zones under infrastructure ownership.
- Log firewall, NSG, and ingress diagnostics.

## Cost Considerations

- Private Endpoints, NAT Gateway, Application Gateway, Front Door, Firewall, and
  ExpressRoute all add network cost.
- Cross-region and internet egress can be expensive.
- Diagnostic logs for network devices can be high volume.

## Examples

```bicep
resource vnet 'Microsoft.Network/virtualNetworks@2023-11-01' = {
  name: 'vnet-cheatsheet-dev'
  location: resourceGroup().location
  properties: {
    addressSpace: {
      addressPrefixes: [
        '10.20.0.0/16'
      ]
    }
    subnets: [
      {
        name: 'snet-private-endpoints'
        properties: {
          addressPrefix: '10.20.10.0/24'
        }
      }
    ]
  }
}
```

## Official Docs

- [Azure Virtual Network documentation](https://learn.microsoft.com/azure/virtual-network/)
- [Azure Private Link documentation](https://learn.microsoft.com/azure/private-link/)
- [Azure Front Door documentation](https://learn.microsoft.com/azure/frontdoor/)
- [Application Gateway documentation](https://learn.microsoft.com/azure/application-gateway/)

```YAML
BaseDO: &BaseDO {type: http, behavior: domain, format: mrs, interval: 86400}
BaseIP: &BaseIP {type: http, behavior: ipcidr, format: mrs, interval: 86400}

# ===== 规则订阅 =====
rule-providers: 

# 域名规则
  direct:    {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/direct.mrs}
  proxy:     {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/proxy.mrs}
  special:   {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/special.mrs}
  reject:    {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/reject.mrs}
 
# IP规则
  directip:  {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/directip.mrs}
  proxyip:   {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/proxyip.mrs}
  specialip: {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/special.mrs}

# ===== 规则路由 =====
rules: 

# 拦截规则
  - AND,((DST-PORT,443),(NETWORK,UDP),(NOT,((GEOIP,CN)))),REJECT
  - RULE-SET,reject,REJECT

# 域名规则
  - RULE-SET,direct,DIRECT
  - RULE-SET,special,Select
  - RULE-SET,proxy,Proxy

  
# IP规则
  - RULE-SET,directip,DIRECT,no-resolve
  - RULE-SET,specialip,Select,no-resolve
  - RULE-SET,proxyip,Proxy,no-resolve


# 兜底规则
  - GEOIP,CN,DIRECT
  - MATCH,Proxy

```

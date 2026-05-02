```YAML
BaseDO: &BaseDO {type: http, behavior: domain, format: mrs, interval: 86400}
BaseIP: &BaseIP {type: http, behavior: ipcidr, format: mrs, interval: 86400}

# ===== 规则订阅 =====
rule-providers: 

  privateip:    {<<: *BaseIP, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/privateip.mrs}

  direct:    {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/direct.mrs}
  proxy:     {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/proxy.mrs}
  special:   {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/special.mrs}
  reject:    {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/reject.mrs}
 
  proxyip:    {<<: *BaseIP, url: https://cdn.jsdelivr.net/gh/jkjkit/clash@main/rule/proxyip.mrs}

# ===== 规则路由 =====
rules: 

# 拦截规则
  - AND,((DST-PORT,443),(NETWORK,UDP),(NOT,((GEOIP,CN)))),REJECT
  - RULE-SET,reject,REJECT
  - RULE-SET,privateip,DIRECT

  - RULE-SET,direct,DIRECT
  - RULE-SET,special,Select
  - RULE-SET,proxy,Proxy

  - RULE-SET,proxyip,Proxy

# 兜底规则
  - GEOIP,CN,DIRECT
  - MATCH,Proxy

```

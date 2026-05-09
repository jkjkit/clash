```YAML
BaseDO: &BaseDO {type: http, behavior: domain, format: mrs, interval: 86400}
BaseIP: &BaseIP {type: http, behavior: ipcidr, format: mrs, interval: 86400}
DOMAIN: &DOMAIN {type: http, behavior: domain, format: text, interval: 86400}
IPCIDR: &IPCIDR {type: http, behavior: ipcidr, format: text, interval: 86400}

rule-providers: 

  cn_cn: {<<: *DOMAIN, url: https://easytalk.dpdns.org/https://raw.githubusercontent.com/jkjkit/clash/refs/heads/main/rule/direct.list}
  no_on: {<<: *DOMAIN, url: https://easytalk.dpdns.org/https://raw.githubusercontent.com/jkjkit/clash/refs/heads/main/rule/proxy.list}
  un_un: {<<: *BaseDO, url: https://easytalk.dpdns.org/https://raw.githubusercontent.com/jkjkit/clash/refs/heads/main/rule/reject.mrs}
  ip_cn: {<<: *IPCIDR, url: https://easytalk.dpdns.org/https://raw.githubusercontent.com/jkjkit/clash/refs/heads/main/rule/ipdirect.list}
  ip_no: {<<: *IPCIDR, url: https://easytalk.dpdns.org/https://raw.githubusercontent.com/jkjkit/clash/refs/heads/main/rule/ipproxy.list}

  cn_do: {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@meta/geo/geosite/cn.mrs}
  no_cn: {<<: *BaseDO, url: https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@meta/geo/geosite/geolocation-!cn.mrs}
  cn_ip: {<<: *BaseIP, url: https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@meta/geo/geoip/cn.mrs}

rules:

  - AND,((DST-PORT,443),(NETWORK,UDP),(NOT,((GEOIP,CN)))),REJECT
  - RULE-SET,un_un,REJECT
  - RULE-SET,cn_cn,DIRECT
  - RULE-SET,no_on,Proxy
  - RULE-SET,ip_cn,DIRECT,no-resolve
  - RULE-SET,ip_no,Proxy,no-resolve

  - RULE-SET,cn_do,DIRECT
  - RULE-SET,no_cn,Proxy

  - RULE-SET,cn_ip,DIRECT,no-resolve
  - MATCH,Proxy

```

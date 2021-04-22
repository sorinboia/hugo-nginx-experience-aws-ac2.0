cat << EOF | kubectl apply -f -
apiVersion: appprotect.f5.com/v1beta1
kind: APPolicy
metadata: 
  name: attacksigs
spec:
  policy:
    name: attacksigs
    template:
      name: POLICY_TEMPLATE_NGINX_BASE
    applicationLanguage: utf-8
    enforcementMode: blocking
    signature-sets:
    - name: All Signatures
      block: true
      alarm: true

---
apiVersion: appprotect.f5.com/v1beta1
kind: APLogConf
metadata:
  name: logconf
spec:
  content:
    format: default
    max_message_size: 64k
    max_request_size: any
  filter:
    request_type: all
---
apiVersion: k8s.nginx.org/v1
kind: Policy
metadata:
  name: waf-policy
spec:
  waf:
    enable: true
    apPolicy: "default/attacksigs"
    securityLog:
      enable: true
      apLogConf: "default/logconf"
      logDest: "syslog:server=127.0.0.1:514"
EOF

cat << EOF | kubectl apply -f -
apiVersion: k8s.nginx.org/v1
kind: VirtualServer
metadata:
  name: arcadia
spec:
  host: $nginx_ingress  
  tls:
    secret: arcadia-wildcard # Represents the server certificate
    redirect:
      enable: true # Always redirect to https if incoming request is http
  policies:
    - name: waf-policy
  upstreams:
    - name: arcadia-users
      service: arcadia-users
      port: 80
      healthCheck: # This is the most basic healthcheck config for more info follow this link https://docs.nginx.com/nginx-ingress-controller/configuration/virtualserver-and-virtualserverroute-resources/#upstream-healthcheck
        enable: true
        path: /healthz
    - name: arcadia-login
      service: arcadia-login
      port: 80
      healthCheck:
        enable: true
        path: /healthz
    - name: arcadia-stocks
      service: arcadia-stocks
      port: 80
      healthCheck:
        enable: true
        path: /healthz
    - name: arcadia-stock-transaction
      service: arcadia-stock-transaction
      port: 80
      healthCheck: 
        enable: true
        path: /healthz
    - name: arcadia-frontend
      service: arcadia-frontend
      port: 80
      healthCheck:
        enable: true
        path: /healthz
  routes:
    - path: /v1/user      
# These directives attach the JWT policy to the route that needs authentication extract the username/email address and add it as a header 
      policies:
      - name: jwt-policy
      action:
        proxy:
          upstream: arcadia-users
          requestHeaders:
            set:
            - name: okta-user
              value: \${jwt_claim_email}
    - path: /v1/login      
      action:
        pass: arcadia-login
    - path: /v1/stock      
      action:
        pass: arcadia-stocks
    - path: /v1/stockt      
      policies:
      - name: jwt-policy
      action:
        proxy:
          upstream: arcadia-stock-transaction
          requestHeaders:
            set:
            - name: okta-user
              value: \${jwt_claim_email}
    - path: /      
      action:
        pass: arcadia-frontend
EOF

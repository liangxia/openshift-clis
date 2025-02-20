# replace default ingress certificate
```
$ oc create secret tls MY-apps-tls --cert=</path/to/ingress-ca.crt> --key=</path/to/ingress-ca.key> -n openshift-ingress
$ oc patch ingresscontroller.operator default --type=merge -p '{"spec":{"defaultCertificate":{"name":"My-apps-tls"}}}' -n openshift-ingress-operator
```

# add API server certificates
```
$ oc create secret tls MY-apiserver-tls --cert=</path/to/apiserver-ca.crt> --key=</path/to/apiserver-ca.key> -n openshift-config
$ oc patch apiserver cluster --type=merge -p '{"spec":{"servingCerts":{"namedCertificates":[{"names":["<FQDN>"], "servingCertificate":{"name":"MY-apiserver-tls"}}]}}}'
$ oc get apiserver cluster -o yaml
...
spec:
  servingCerts:
    namedCertificates:
    - names:
      - <FQDN>
      servingCertificate:
        name: MY-apiserver-tls
...
```

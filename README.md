# PIV Authenticator Operator for OpenShift 4

## Operator Installation
To use the operator you need to install all of the supporting infrastructure. You will need the appropriate permissions and it is easiest to do that as Cluster Admin.

```bash
# account
[]$ oc apply -f operator/service-account.yml
# cluster roles
[]$ oc adm reconcile -f operator/cluster-role.yml
[]$ oc adm reconcile -f operator/cluster-role-binding.yml
# kube system roles
[]$ oc adm reconcile -f operator/kube-system-role.yml
[]$ oc adm reconcile -f operator/kube-system-role-binding.yml
# roles on the config namespace
[]$ oc adm reconcile -f operator/openshift-config-role.yml
[]$ oc adm reconcile -f operator/openshift-config-role-binding.yml
# roles on the authentication namespace
[]$ oc adm reconcile -f operator/openshift-authentication-role.yml
[]$ oc adm reconcile -f operator/openshift-authentication-role-binding.yml
# roles on the auth-pivproxy-operator namespace
[]$ oc adm reconcile -f operator/role.yml
[]$ oc adm reconcile -f operator/role-binding.yml
# install the custom resource definition
[]$ oc apply -f operator/crds/v1beta2_pivauth_crd.yml
# install the operator
[]$ oc apply -f operator/operator.yml
```

After this the operator image will be pulled and the operator pod will start running.
```bash
[]$ oc get pods -n auth-pivproxy-operator
```

## Enabling PIV Authentication
After the installation of the operator you can insert a custom resource that has all the information required for enabling PIV Authentication. The only required element is the Smartcard CA bundle for all of the smartcards your instance will accept.

```bash
[]$ oc apply -f - <<EOF
apiVersion: chrisruffalo.github.com/v1beta2
kind: PivAuth
metadata:
  name: production
  namespace: ansible-pivauth-operator
spec:
  smartcard_ca: |
    -----BEGIN CERTIFICATE-----
    MIIDjDCCAnSgAwIBAgIJAI/wSjoGLnmnMA0GCSqGSIb3DQEBDQUAMFsxCzAJBgNV
    BAYTAlNDMRcwFQYDVQQIDA5Tb3V0aCBDYXJvbGluYTEVMBMGA1UEBwwMRGVmYXVs
    dCBDaXR5MRwwGgYDVQQKDBNEZWZhdWx0IENvbXBhbnkgTHRkMB4XDTE4MDkyODE3
    MjYxMloXDTIxMDcxODE3MjYxMlowWzELMAkGA1UEBhMCU0MxFzAVBgNVBAgMDlNv
    dXRoIENhcm9saW5hMRUwEwYDVQQHDAxEZWZhdWx0IENpdHkxHDAaBgNVBAoME0Rl
    ZmF1bHQgQ29tcGFueSBMdGQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
    AQDfwKxbAomeOkKvV4Y/udg2KhTO4fG15TATUVqEjBB7npXifH4o4ekqoieE2DAl
    kwrUlGd46fi4T6H8wuo/wJDGx6N4FeQ9MM1qi+qpaJqk9LdGg1TZ6hX3ZTqcqEUF
    HNtnAXvjpCyG8whtD2hO29ENQ0CxccMwY7RaAknzywM1lpaaeVW42XIkNmV9p8ZG
    zkOyVSU3U8iJrPc/NcSAXtG3WXhb4wD3OyEFFHlT6ClTFsgqgmBxl/xsasG0qixh
    keAMNtu/Ay7p0Sk+zvbsi/qjBp326J1g6bpFIfB9aBrLMqYzST/PEV5BHsbSX7D/
    9Tr6LJvjR4zg370zo+ZbvZdBAgMBAAGjUzBRMB0GA1UdDgQWBBQNIKCZZWiBxGxM
    gzYqcneQ+YBNhzAfBgNVHSMEGDAWgBQNIKCZZWiBxGxMgzYqcneQ+YBNhzAPBgNV
    HRMBAf8EBTADAQH/MA0GCSqGSIb3DQEBDQUAA4IBAQCkRCnrF9Tvkr94EqL86LpV
    /oTYvbnwl2WwbuwsbqzzWW/QONDZ/nkcDZInSmi1wTkCvd4u7fBBd7uTn1KH3OO2
    nb1kdV1wOOMobtc1tjRhQWRt8RtWNjtJdQcpBkcpl2OIJB5X/UJNvVfD3AmwVGIQ
    9p9Wll4SzI4idDTtJ4El4DOGqf1twIylvM5H5Ct+yZaw6CcmjnOOUJIS3DU4NWVa
    5rnQyl5qo+mEJKu5KNNT24M0knc8DlO0Dk6ukcEB7+k5pzVvFpFFcuovXSvCo+mi
    4e6GvvDypWNGNDToih6nVMtvNu+NXC0O4yxQdF+HCpofFHlwoaVycbJGU+twP8eH
    -----END CERTIFICATE-----
EOF
```
openssl genrsa -out rbac-user.key 2048
openssl req -new -key rbac-user.key -out rbac-user.csr -subj "/CN=rbac-user/O=rbac-test"

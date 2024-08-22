https://www.redhat.com/sysadmin/kubernetes-workloads-podman-systemd


Deploying a multi-container application using Podman and Quadlet
Learn about the unit files Quadlet supports and how to use them to deploy containers using Podman and systemd.
Posted: March 2, 2023 | | byYgal Blum (Red Hat)
https://www.redhat.com/sysadmin/multi-container-application-podman-quadlet


openssl req -new -x509 -sha512 -nodes -newkey ec:<(openssl ecparam -name secp384r1) -subj "/C=BR/ST=RJ/L=Sao Paulo/CN=*.localhost/O=LocalOrg" -addext "subjectAltName = DNS:localhost, DNS:127.0.0.1" -keyout certificate.key -out certificate.pem -days 3650

chmod 600 certificate.key

cat certificate.key.pem
cat certificate.key

openssl x509 -in certificate.pem -text


kubectl create secret generic \
    --from-file=certificate.key \
    --from-file=certificate.pem \
    envoy-certificates \
    --dry-run=client \
    -o yaml | \
    podman kube play -


MYSQL_ROOT_PASSWORD=$(tr -dc A-Za-z0-9 </dev/urandom | head -c 13)

kubectl create secret generic \
    --from-literal=password="${MYSQL_ROOT_PASSWORD}" \
    mysql-root-password-kube \
    --dry-run=client \
    -o yaml | \
    podman kube play -


echo -n "${MYSQL_ROOT_PASSWORD}" | \
    podman secret create mysql-root-password-container -

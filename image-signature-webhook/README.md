# Image Signature Webhook

## Usage
```
image-signature-webhook -h
```

```
Usage of image-signature-webhook:
  -grafeas string
    	The Grafeas server address (default "http://grafeas:8080")
  -tls-cert string
    	TLS certificate file. (default "/etc/admission-controller/tls/cert.pem")
  -tls-key string
    	TLS key file. (default "/etc/admission-controller/tls/key.pem")
```

## Build
To build the image-siganture-webhook, be sure to import the appropriate libraries into your project:

``` bash
go get golang.org/x/crypto/openpgp
go get github.com/Jeffail/gabs
go get github.com/Grafeas/client-go/v1alpha1
go get k8s.io/api/admission/v1beta1
go get k8s.io/api/core/v1
go get k8s.io/apimachinery/pkg/apis/meta/v1
```

Build the server:

``` bash
go build -o image-signature-webhook main.go
```

Build the image:

``` bash
docker build image-signature-webhook -t image-signature-webhook:0.0.n
```

## Logging

To attach to your image-signature-webhook pod and get access to more detailed logging, start a shell session and obtain the name of your pod as follows:

``` bash
kubectl get pods
NAME                                       READY     STATUS    RESTARTS   AGE
grafeas-7554b6bffd-6gsfq                   1/1       Running   0          29m
image-signature-webhook-6fd49f765f-8h9z9   1/1       Running   0          1m
```

Attach to the pod:

```
kubectl logs -f image-signature-webhook-6fd49f765f-8h9z9
```

As requests to create pods are submitted to the Kubernetes API, details of the image signature webhook operations will be output to the console:

``` bash
2018/09/28 11:21:03 Validating-Admission-Webhook :: Verify Signature
2018/09/28 11:21:03 ----------------------------------------------------------
2018/09/28 11:21:03 Data: {"occurrences":[{"name":"projects/image-signing/occurrences/165d9a8e-94fd-415d-8069-87faa86de8d6","resourceUrl":"https://docker.io/mysql/mysql-server@sha256:58c5d4635ab6c6ec23b542a274b9881dca62de19c793a8b8227a830a83bdbbdd","noteName":"projects/image-signing/notes/production","kind":"KIND_UNSPECIFIED","attestationDetails":{"pgpSignedAttestation":{"signature":"LS0tLS1CRUdJTiBQR1AgU0lHTkVEIE1FU1NBR0UtLS0tLQpIYXNoOiBTSEExCgpzaGEyNTY6NThjNWQ0NjM1YWI2YzZlYzIzYjU0MmEyNzRiOTg4MWRjYTYyZGUxOWM3OTNhOGI4MjI3YTgzMGE4M2JkYmJkZAotLS0tLUJFR0lOIFBHUCBTSUdOQVRVUkUtLS0tLQpWZXJzaW9uOiBHbnVQRyB2Mi4wLjIyIChHTlUvTGludXgpCgppUUVjQkFFQkFnQUdCUUpicmdsUkFBb0pFT0ZLbGNUeDhTUyt4bTBILzBGNmNtMFJpWW5HcFRIY2tjS3ExZWdQCmJFSGd5bytKdldvaW4zU1BIeVYrcGhPdi9MdXhQeURjY3d6R0pwd3VTeTJnZ0lFclJ6QWQ1THJMemVpZEFaeGcKaHRKQktuL0ZVZ0t5OTE5VG94RFdvSmQ4NXpvcjk1ZUVsVW1LU0U2M1NkaHBLNC95Vm1kVWh4Yy9qankyRDlsdApGTWZpYlE1RjhxZDhURDJLOUowcWtDVWl0OWpkd2N2Qnl6bnhNeGpaQkpsTzI1T0RPa1JXNlRZQzdTem9UbEZvCkdvQXZUKzNaODYvOS9RS2lGQkZvdUJScXNTYVEyYjR3VFBjUkJYZi8xZFV5V2hUd0dIOEVDaGNDN0dwejRVL3AKTWtwWCsvN0hTME8vUUpHdEZzSHpFSVFxOUpJa2VITFVxL3dlZ1loM2FKdEZQditLRGVWMTh0OWZlTDRRU2Z3PQo9N0FNaQotLS0tLUVORCBQR1AgU0lHTkFUVVJFLS0tLS0K","contentType":"CONTENT_TYPE_UNSPECIFIED","pgpKeyId":"F1F124BE"}},"remediation":"","createTime":null,"updateTime":null,"operationName":""}],"nextPageToken":"1"}
2018/09/28 11:21:03 Occurrence Name: projects/image-signing/occurrences/165d9a8e-94fd-415d-8069-87faa86de8d6
2018/09/28 11:21:03 Container Image: docker.io/oracle/nosql:4.3.11
2018/09/28 11:21:03 ResourceUrl: https://docker.io/mysql/mysql-server@sha256:58c5d4635ab6c6ec23b542a274b9881dca62de19c793a8b8227a830a83bdbbdd
2018/09/28 11:21:03 Signature: LS0tLS1CRUdJTiBQR1AgU0lHTkVEIE1FU1NBR0UtLS0tLQpIYXNoOiBTSEExCgpzaGEyNTY6NThjNWQ0NjM1YWI2YzZlYzIzYjU0MmEyNzRiOTg4MWRjYTYyZGUxOWM3OTNhOGI4MjI3YTgzMGE4M2JkYmJkZAotLS0tLUJFR0lOIFBHUCBTSUdOQVRVUkUtLS0tLQpWZXJzaW9uOiBHbnVQRyB2Mi4wLjIyIChHTlUvTGludXgpCgppUUVjQkFFQkFnQUdCUUpicmdsUkFBb0pFT0ZLbGNUeDhTUyt4bTBILzBGNmNtMFJpWW5HcFRIY2tjS3ExZWdQCmJFSGd5bytKdldvaW4zU1BIeVYrcGhPdi9MdXhQeURjY3d6R0pwd3VTeTJnZ0lFclJ6QWQ1THJMemVpZEFaeGcKaHRKQktuL0ZVZ0t5OTE5VG94RFdvSmQ4NXpvcjk1ZUVsVW1LU0U2M1NkaHBLNC95Vm1kVWh4Yy9qankyRDlsdApGTWZpYlE1RjhxZDhURDJLOUowcWtDVWl0OWpkd2N2Qnl6bnhNeGpaQkpsTzI1T0RPa1JXNlRZQzdTem9UbEZvCkdvQXZUKzNaODYvOS9RS2lGQkZvdUJScXNTYVEyYjR3VFBjUkJYZi8xZFV5V2hUd0dIOEVDaGNDN0dwejRVL3AKTWtwWCsvN0hTME8vUUpHdEZzSHpFSVFxOUpJa2VITFVxL3dlZ1loM2FKdEZQditLRGVWMTh0OWZlTDRRU2Z3PQo9N0FNaQotLS0tLUVORCBQR1AgU0lHTkFUVVJFLS0tLS0K
2018/09/28 11:21:03 KeyId: F1F124BE
2018/09/28 11:21:03
2018/09/28 11:21:03 Validating-Admission-Webhook :: Verify Signature :: Result
2018/09/28 11:21:03 ----------------------------------------------------------
2018/09/28 11:21:03 No matched signatures for container image: docker.io/oracle/nosql:4.3.11
2018/09/28 11:21:03
2018/09/28 05:44:13 Validating-Admission-Webhook :: Verify Signature
2018/09/28 05:44:13 ----------------------------------------------------------
2018/09/28 05:44:13 Data: {"occurrences":[{"name":"projects/image-signing/occurrences/4924a803-9251-4047-9a38-7b79977afde6","resourceUrl":"https://docker.io/mysql/mysql-server@sha256:58c5d4635ab6c6ec23b542a274b9881dca62de19c793a8b8227a830a83bdbbdd","noteName":"projects/image-signing/notes/production","kind":"KIND_UNSPECIFIED","attestationDetails":{"pgpSignedAttestation":{"signature":"LS0tLS1CRUdJTiBQR1AgU0lHTkVEIE1FU1NBR0UtLS0tLQpIYXNoOiBTSEExCgpzaGEyNTY6NThjNWQ0NjM1YWI2YzZlYzIzYjU0MmEyNzRiOTg4MWRjYTYyZGUxOWM3OTNhOGI4MjI3YTgzMGE4M2JkYmJkZAotLS0tLUJFR0lOIFBHUCBTSUdOQVRVUkUtLS0tLQpWZXJzaW9uOiBHbnVQRyB2Mi4wLjIyIChHTlUvTGludXgpCgppUUVjQkFFQkFnQUdCUUpicmJ3S0FBb0pFUHVNMTdiTU4zWkxsRDhILzNuMGtFVTJuZHpqcWxEZ0NreUJpd0ZQCm9xVXFEREtOdXBuV1BjeVRjN0J4Z004YThaaEU5aTR5QStoM25PUUJMeUdHZkNEcVI2UEtVM0QzR0tSNlppZkMKVkdib1I5NjNKYk9hSXdtUVJmaEZHYlhlT29NeGVFVkhiODFBRVRJZlZKVUQ5OWR3cUZZcnJ5SEVKUkRRd1B3UgpvR1FEWHZyTWowam4rZ2RnMXlYNXQrOWpOWk43VWZWbzE0Q09DdFdsZGFxcjRoZUYzdE12bjl4eitrQmIrdUJLCk5DaGtSam1yUjVsWHAxMEROejh0STNSTWxxQVdUaFJMT1M1WkVmOXhqWkRybnlOeVMrc1VlZ0p2c0dPOTJTR3EKbkJNRWthZ2l5dkJsSGs1RjY0TVE3V0xqZmdrbmR2MUtSU0lrREUzeXlGYkFyK1Jtc0pTYjFXRlVBUUdUckkwPQo9enF6cgotLS0tLUVORCBQR1AgU0lHTkFUVVJFLS0tLS0K","contentType":"CONTENT_TYPE_UNSPECIFIED","pgpKeyId":"CC37764B"}},"remediation":"","createTime":null,"updateTime":null,"operationName":""}],"nextPageToken":"1"}
2018/09/28 05:44:13 Occurrence Name: projects/image-signing/occurrences/4924a803-9251-4047-9a38-7b79977afde6
2018/09/28 05:44:13 Container Image: docker.io/mysql/mysql-server@sha256:58c5d4635ab6c6ec23b542a274b9881dca62de19c793a8b8227a830a83bdbbdd
2018/09/28 05:44:13 ResourceUrl: https://docker.io/mysql/mysql-server@sha256:58c5d4635ab6c6ec23b542a274b9881dca62de19c793a8b8227a830a83bdbbdd
2018/09/28 05:44:13 Signature: LS0tLS1CRUdJTiBQR1AgU0lHTkVEIE1FU1NBR0UtLS0tLQpIYXNoOiBTSEExCgpzaGEyNTY6NThjNWQ0NjM1YWI2YzZlYzIzYjU0MmEyNzRiOTg4MWRjYTYyZGUxOWM3OTNhOGI4MjI3YTgzMGE4M2JkYmJkZAotLS0tLUJFR0lOIFBHUCBTSUdOQVRVUkUtLS0tLQpWZXJzaW9uOiBHbnVQRyB2Mi4wLjIyIChHTlUvTGludXgpCgppUUVjQkFFQkFnQUdCUUpicmJ3S0FBb0pFUHVNMTdiTU4zWkxsRDhILzNuMGtFVTJuZHpqcWxEZ0NreUJpd0ZQCm9xVXFEREtOdXBuV1BjeVRjN0J4Z004YThaaEU5aTR5QStoM25PUUJMeUdHZkNEcVI2UEtVM0QzR0tSNlppZkMKVkdib1I5NjNKYk9hSXdtUVJmaEZHYlhlT29NeGVFVkhiODFBRVRJZlZKVUQ5OWR3cUZZcnJ5SEVKUkRRd1B3UgpvR1FEWHZyTWowam4rZ2RnMXlYNXQrOWpOWk43VWZWbzE0Q09DdFdsZGFxcjRoZUYzdE12bjl4eitrQmIrdUJLCk5DaGtSam1yUjVsWHAxMEROejh0STNSTWxxQVdUaFJMT1M1WkVmOXhqWkRybnlOeVMrc1VlZ0p2c0dPOTJTR3EKbkJNRWthZ2l5dkJsSGs1RjY0TVE3V0xqZmdrbmR2MUtSU0lrREUzeXlGYkFyK1Jtc0pTYjFXRlVBUUdUckkwPQo9enF6cgotLS0tLUVORCBQR1AgU0lHTkFUVVJFLS0tLS0K
2018/09/28 05:44:13 KeyId: CC37764B
2018/09/28 05:44:13
2018/09/28 05:44:13 Validating-Admission-Webhook :: Verify Signature :: Result
2018/09/28 05:44:13 ----------------------------------------------------------
2018/09/28 05:44:13 Using public key: /etc/admission-controller/pubkeys/CC37764B.pub
2018/09/28 05:44:13 Signature verified for container image: docker.io/mysql/mysql-server@sha256:58c5d4635ab6c6ec23b542a274b9881dca62de19c793a8b8227a830a83bdbbdd
```
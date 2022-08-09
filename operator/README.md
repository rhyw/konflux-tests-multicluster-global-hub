# hub-of-hubs-operator

The operator of Hub-of-Hubs (see: https://github.com/stolostron/hub-of-hubs)

## Prerequisites

1. Connect to a Kubernetes cluster with `kubectl`
2. ACM or OCM is installed on the Kubernetes cluster
3. PostgreSQL is installed and a database is created for hub-of-hubs, also a secret with name `postgresql-secret` that contains the credential should be created in `open-cluster-management` namespace:

```bash
kubectl create secret generic postgresql-secret -n "open-cluster-management" \
    --from-literal=host=<postgresql-host> \
    --from-literal=port=<postgresql-port> \
    --from-literal=user=<admin-user-of-hub-of-hubs-database> \
    --from-literal=password=<admin-user-password> \
    --from-literal=database=<name-of-hub-of-hubs-database>
```

4. Kafka is installed and two topics `spec` and `status` are created, also a secret with name `kafka-secret` that contains the kafka access information should be created in `open-cluster-management` namespace:

```bash
kubectl create secret generic kafka-secret -n "open-cluster-management" \
    --from-literal=bootstrap_server=<kafka-bootstrap-server-address> \
    --from-literal=CA=<CA-for-kafka-server>
```

## Getting started

### Running on the cluster

1. Build and push your image to the location specified by `IMG`:

```bash
make docker-build docker-push IMG=<some-registry>/hub-of-hubs-operator:<tag>
```

2. Deploy the controller to the cluster with the image specified by `IMG`:

```bash
make deploy IMG=<some-registry>/hub-of-hubs-operator:<tag>
```

3. Install Instances of Custom Resource:

```bash
kubectl apply -k config/samples/
```

### Uninstall CRD

To delete the CRD from the cluster:

```bash
make uninstall
```

### Undeploy controller

Undeploy the controller from the cluster:

```bash
make undeploy
```

## Contributing

### Test It Out Locally

1. Install CRD and run operator locally:

```bash
make install run
```

2. Install Instances of Custom Resource:

```bash
kubectl apply -k config/samples/
```

### Modifying the API definitions

If you are editing the API definitions, generate the generated code and manifests such as CRs, CRDs, CSV using:

```bash
make generate manifests bundle
```

NOTE: Run `make --help` for more information on all potential make targets
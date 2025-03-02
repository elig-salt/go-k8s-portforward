A Go library for creating port forwards into pods running in a Kubernetes cluster.

This is a fork of: justinbarrick/go-k8s-portforward

This code is heavily inspired by the implementations in kubectl, fission, and helm:

* https://github.com/kubernetes/helm/blob/master/pkg/kube/tunnel.go
* https://github.com/kubernetes/kubernetes/blob/master/pkg/kubectl/cmd/portforward.go
* https://github.com/fission/fission/blob/master/fission/portforward/portforward.go

See [godoc.org](https://godoc.org/elig-salt/go-k8s-portforward/go-k8s-portforward) for full documentation.

# Example

A minimal example which will forward to the 

```
package main

import (
	"log"
	"time"
	portforward "elig-salt/go-k8s-portforward/go-k8s-portforward"
	metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
)

func main() {
    namespace := "default"
	port := 8080
	listenPort := 9000
    labels := map[string]string{ "app": "hello-world" }

	pf, err := portforward.NewPortForwarder(namespace, metav1.LabelSelector{
		MatchLabels: labels,
	}, port)
	if err != nil {
		log.Fatal("Error setting up port forwarder: ", err)
	}
	pf.Name = pod
	pf.ListenPort = listenPort

	ctx, stop := signal.NotifyContext(context.Background(), os.Interrupt)
	defer stop()
	err = pf.Start(ctx)
	if err != nil {
		log.Fatal("Error starting port forward: ", err)
	}

	log.Printf("Started tunnel on %d\n", pf.ListenPort)
	time.Sleep(60 * time.Second)
}
```

Also see `cmd/main.go`.

# Kubeconfig

By default, it will load a Kubernetes configuration file from ~/.kube/config or $KUBECONFIG.

It is possible to provide your own Kubernetes client by instantiating the PortForward struct
directly instead of calling the `NewPortForwarder` method.

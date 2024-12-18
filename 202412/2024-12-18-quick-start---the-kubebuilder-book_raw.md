Title: Quick Start - The Kubebuilder Book

URL Source: https://book.kubebuilder.io/quick-start.html

Markdown Content:
![Image 3: The Kubebuilder Book](https://book.kubebuilder.io/logos/logo-single-line.png)
----------------------------------------------------------------------------------------

[Quick Start](https://book.kubebuilder.io/quick-start.html#quick-start)
-----------------------------------------------------------------------

This Quick Start guide will cover:

*   [Creating a project](https://book.kubebuilder.io/quick-start.html#create-a-project)
*   [Creating an API](https://book.kubebuilder.io/quick-start.html#create-an-api)
*   [Running locally](https://book.kubebuilder.io/quick-start.html#test-it-out)
*   [Running in-cluster](https://book.kubebuilder.io/quick-start.html#run-it-on-the-cluster)

[Prerequisites](https://book.kubebuilder.io/quick-start.html#prerequisites)
---------------------------------------------------------------------------

*   [go](https://golang.org/dl/) version v1.22.0+
*   [docker](https://docs.docker.com/install/) version 17.03+.
*   [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) version v1.11.3+.
*   Access to a Kubernetes v1.11.3+ cluster.

[Installation](https://book.kubebuilder.io/quick-start.html#installation)
-------------------------------------------------------------------------

Install [kubebuilder](https://sigs.k8s.io/kubebuilder):

```
# download kubebuilder and install locally.
curl -L -o kubebuilder "https://go.kubebuilder.io/dl/latest/$(go env GOOS)/$(go env GOARCH)"
chmod +x kubebuilder && sudo mv kubebuilder /usr/local/bin/
```

[Create a Project](https://book.kubebuilder.io/quick-start.html#create-a-project)
---------------------------------------------------------------------------------

Create a directory, and then run the init command inside of it to initialize a new project. Follows an example.

```
mkdir -p ~/projects/guestbook
cd ~/projects/guestbook
kubebuilder init --domain my.domain --repo my.domain/guestbook
```

[Create an API](https://book.kubebuilder.io/quick-start.html#create-an-api)
---------------------------------------------------------------------------

Run the following command to create a new API (group/version) as `webapp/v1` and the new Kind(CRD) `Guestbook` on it:

```
kubebuilder create api --group webapp --version v1 --kind Guestbook
```

**OPTIONAL:** Edit the API definition and the reconciliation business logic. For more info see [Designing an API](https://book.kubebuilder.io/cronjob-tutorial/api-design) and [What’s in a Controller](https://book.kubebuilder.io/cronjob-tutorial/controller-overview).

If you are editing the API definitions, generate the manifests such as Custom Resources (CRs) or Custom Resource Definitions (CRDs) using

```
make manifests
```

Click here to see an example. (api/v1/guestbook\_types.go)

```
// GuestbookSpec defines the desired state of Guestbook
type GuestbookSpec struct {
    // INSERT ADDITIONAL SPEC FIELDS - desired state of cluster
    // Important: Run "make" to regenerate code after modifying this file

    // Quantity of instances
    // +kubebuilder:validation:Minimum=1
    // +kubebuilder:validation:Maximum=10
    Size int32 `json:"size"`

    // Name of the ConfigMap for GuestbookSpec's configuration
    // +kubebuilder:validation:MaxLength=15
    // +kubebuilder:validation:MinLength=1
    ConfigMapName string `json:"configMapName"`

    // +kubebuilder:validation:Enum=Phone;Address;Name
    Type string `json:"alias,omitempty"`
}

// GuestbookStatus defines the observed state of Guestbook
type GuestbookStatus struct {
    // INSERT ADDITIONAL STATUS FIELD - define observed state of cluster
    // Important: Run "make" to regenerate code after modifying this file

    // PodName of the active Guestbook node.
    Active string `json:"active"`

    // PodNames of the standby Guestbook nodes.
    Standby []string `json:"standby"`
}

// +kubebuilder:object:root=true
// +kubebuilder:subresource:status
// +kubebuilder:resource:scope=Cluster

// Guestbook is the Schema for the guestbooks API
type Guestbook struct {
    metav1.TypeMeta   `json:",inline"`
    metav1.ObjectMeta `json:"metadata,omitempty"`

    Spec   GuestbookSpec   `json:"spec,omitempty"`
    Status GuestbookStatus `json:"status,omitempty"`
}
```

[Test It Out](https://book.kubebuilder.io/quick-start.html#test-it-out)
-----------------------------------------------------------------------

You’ll need a Kubernetes cluster to run against. You can use [KIND](https://sigs.k8s.io/kind) to get a local cluster for testing, or run against a remote cluster.

Install the CRDs into the cluster:

```
make install
```

For quick feedback and code-level debugging, run your controller (this will run in the foreground, so switch to a new terminal if you want to leave it running):

```
make run
```

[Install Instances of Custom Resources](https://book.kubebuilder.io/quick-start.html#install-instances-of-custom-resources)
---------------------------------------------------------------------------------------------------------------------------

If you pressed `y` for Create Resource \[y/n\] then you created a CR for your CRD in your samples (make sure to edit them first if you’ve changed the API definition):

```
kubectl apply -k config/samples/
```

[Run It On the Cluster](https://book.kubebuilder.io/quick-start.html#run-it-on-the-cluster)
-------------------------------------------------------------------------------------------

When your controller is ready to be packaged and tested in other clusters.

Build and push your image to the location specified by `IMG`:

```
make docker-build docker-push IMG=<some-registry>/<project-name>:tag
```

Deploy the controller to the cluster with image specified by `IMG`:

```
make deploy IMG=<some-registry>/<project-name>:tag
```

[Uninstall CRDs](https://book.kubebuilder.io/quick-start.html#uninstall-crds)
-----------------------------------------------------------------------------

To delete your CRDs from the cluster:

```
make uninstall
```

[Undeploy controller](https://book.kubebuilder.io/quick-start.html#undeploy-controller)
---------------------------------------------------------------------------------------

Undeploy the controller to the cluster:

```
make undeploy
```

[Next Step](https://book.kubebuilder.io/quick-start.html#next-step)
-------------------------------------------------------------------

*   Now, take a look at the [Architecture Concept Diagram](https://book.kubebuilder.io/architecture) for a clearer overview.
*   Next, proceed with the [Getting Started Guide](https://book.kubebuilder.io/getting-started), which should take no more than 30 minutes and will provide a solid foundation. Afterward, dive into the [CronJob Tutorial](https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial) to deepen your understanding by developing a demo project.

[](https://book.kubebuilder.io/architecture "Previous chapter")[](https://book.kubebuilder.io/getting-started "Next chapter")

[](https://book.kubebuilder.io/architecture "Previous chapter")[](https://book.kubebuilder.io/getting-started "Next chapter")

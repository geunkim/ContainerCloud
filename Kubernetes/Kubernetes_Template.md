## Template

```yaml
apiVersion: v1
kind: Pod     # Pod 오브젝트의 작업
metadata:
spec:
```

| 필드 key | 설명 |
|:-----:|:----------:|
| apiVersion | 사용하려는 쿠버네티스 API 버전을 명시    |
| kind | 어떤 오브젝트 또는 컨트롤러의 작업인지 명시 (예: Pod, Deployment, Ingress) |
| metadata | 메타데이터를 설정, 해당 오브젝트의 이름이나 레이블 설정 |
| spec | Pod가 어떤 컨테이너를 가지고 실행하며 실행할 때 어떻게 동작해야 하는지 명시 |

Kubernetes cluster에서 사용가능한 API 버전을 확인하는 명령어는 다음과 관다. 

```bash
$kubectl api-versions
```
```bash
admissionregistration.k8s.io/v1
apiextensions.k8s.io/v1
apiregistration.k8s.io/v1
apps/v1
authentication.k8s.io/v1
authorization.k8s.io/v1
autoscaling/v1
autoscaling/v2
batch/v1
certificates.k8s.io/v1
coordination.k8s.io/v1
discovery.k8s.io/v1
events.k8s.io/v1
flowcontrol.apiserver.k8s.io/v1beta2
flowcontrol.apiserver.k8s.io/v1beta3
networking.k8s.io/v1
node.k8s.io/v1
policy/v1
rbac.authorization.k8s.io/v1
scheduling.k8s.io/v1
storage.k8s.io/v1
v1
```

다음 명령을 실행하면 pod 템플릿에서 사용하는 하위 필드에 대한 정보를 출력한다.
Pod는 호스트에서 실행할 수 있는 컨터이너들의 모임으로 클라이언트에 의해서 만들어지는 자원으로 호스트 중 하나로 스케줄된다. 
Pod 객체의 하위 필드는 ``apiVersion``, ``kind``, ``metadata``, ``spac``, ``status`` 가 있다. 

```bash
$kubectl explain pods
```
```bash
KIND:       Pod
VERSION:    v1

DESCRIPTION:
    Pod is a collection of containers that can run on a host. This resource is
    created by clients and scheduled onto hosts.
    
FIELDS:
  apiVersion	<string>
    APIVersion defines the versioned schema of this representation of an object.
    Servers should convert recognized schemas to the latest internal value, and
    may reject unrecognized values. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

  kind	<string>
    Kind is a string value representing the REST resource this object
    represents. Servers may infer this from the endpoint the client submits
    requests to. Cannot be updated. In CamelCase. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

  metadata	<ObjectMeta>
    Standard object's metadata. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

  spec	<PodSpec>
    Specification of the desired behavior of the pod. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status

  status	<PodStatus>
    Most recently observed status of the pod. This data may not be up to date.
    Populated by the system. Read-only. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status
```

``metadata``, ``spec`` 은 다양한 하위 필드가 있다. ``metadata`` 의 하위 필드는 다음 명령어로 확인 가능하다.

```bash
$kubectl explain pods.metadata
```
```bash
geunkim@Geunhyungui-MacBookAir kubenetes % kubectl explain pods.metadata
KIND:       Pod
VERSION:    v1

FIELD: metadata <ObjectMeta>

DESCRIPTION:
    Standard object's metadata. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata
    ObjectMeta is metadata that all persisted resources must have, which
    includes all objects users must create.
    
FIELDS:
  annotations	<map[string]string>
    Annotations is an unstructured key value map stored with a resource that may
    be set by external tools to store and retrieve arbitrary metadata. They are
    not queryable and should be preserved when modifying objects. More info:
    https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations

  creationTimestamp	<string>
    CreationTimestamp is a timestamp representing the server time when this
    object was created. It is not guaranteed to be set in happens-before order
    across separate operations. Clients may not set this value. It is
    represented in RFC3339 form and is in UTC.
    
    Populated by the system. Read-only. Null for lists. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

  deletionGracePeriodSeconds	<integer>
    Number of seconds allowed for this object to gracefully terminate before it
    will be removed from the system. Only set when deletionTimestamp is also
    set. May only be shortened. Read-only.

  deletionTimestamp	<string>
    DeletionTimestamp is RFC 3339 date and time at which this resource will be
    deleted. This field is set by the server when a graceful deletion is
    requested by the user, and is not directly settable by a client. The
    resource is expected to be deleted (no longer visible from resource lists,
    and not reachable by name) after the time in this field, once the finalizers
    list is empty. As long as the finalizers list contains items, deletion is
    blocked. Once the deletionTimestamp is set, this value may not be unset or
    be set further into the future, although it may be shortened or the resource
    may be deleted prior to this time. For example, a user may request that a
    pod is deleted in 30 seconds. The Kubelet will react by sending a graceful
    termination signal to the containers in the pod. After that 30 seconds, the
    Kubelet will send a hard termination signal (SIGKILL) to the container and
    after cleanup, remove the pod from the API. In the presence of network
    partitions, this object may still exist after this timestamp, until an
    administrator or automated process can determine the resource is fully
    terminated. If not set, graceful deletion of the object has not been
    requested.
    
    Populated by the system when a graceful deletion is requested. Read-only.
    More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

  finalizers	<[]string>
    Must be empty before the object is deleted from the registry. Each entry is
    an identifier for the responsible component that will remove the entry from
    the list. If the deletionTimestamp of the object is non-nil, entries in this
    list can only be removed. Finalizers may be processed and removed in any
    order.  Order is NOT enforced because it introduces significant risk of
    stuck finalizers. finalizers is a shared field, any actor with permission
    can reorder it. If the finalizer list is processed in order, then this can
    lead to a situation in which the component responsible for the first
    finalizer in the list is waiting for a signal (field value, external system,
    or other) produced by a component responsible for a finalizer later in the
    list, resulting in a deadlock. Without enforced ordering finalizers are free
    to order amongst themselves and are not vulnerable to ordering changes in
    the list.

  generateName	<string>
    GenerateName is an optional prefix, used by the server, to generate a unique
    name ONLY IF the Name field has not been provided. If this field is used,
    the name returned to the client will be different than the name passed. This
    value will also be combined with a unique suffix. The provided value has the
    same validation rules as the Name field, and may be truncated by the length
    of the suffix required to make the value unique on the server.
    
    If this field is specified and the generated name exists, the server will
    return a 409.
    
    Applied only if Name is not specified. More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#idempotency

  generation	<integer>
    A sequence number representing a specific generation of the desired state.
    Populated by the system. Read-only.

  labels	<map[string]string>
    Map of string keys and values that can be used to organize and categorize
    (scope and select) objects. May match selectors of replication controllers
    and services. More info:
    https://kubernetes.io/docs/concepts/overview/working-with-objects/labels

  managedFields	<[]ManagedFieldsEntry>
    ManagedFields maps workflow-id and version to the set of fields that are
    managed by that workflow. This is mostly for internal housekeeping, and
    users typically shouldn't need to set or understand this field. A workflow
    can be the user's name, a controller's name, or the name of a specific apply
    path like "ci-cd". The set of fields is always in the version that the
    workflow used when modifying the object.

  name	<string>
    Name must be unique within a namespace. Is required when creating resources,
    although some resources may allow a client to request the generation of an
    appropriate name automatically. Name is primarily intended for creation
    idempotence and configuration definition. Cannot be updated. More info:
    https://kubernetes.io/docs/concepts/overview/working-with-objects/names#names

  namespace	<string>
    Namespace defines the space within which each name must be unique. An empty
    namespace is equivalent to the "default" namespace, but "default" is the
    canonical representation. Not all objects are required to be scoped to a
    namespace - the value of this field for those objects will be empty.
    
    Must be a DNS_LABEL. Cannot be updated. More info:
    https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces

  ownerReferences	<[]OwnerReference>
    List of objects depended by this object. If ALL objects in the list have
    been deleted, this object will be garbage collected. If this object is
    managed by a controller, then an entry in this list will point to this
    controller, with the controller field set to true. There cannot be more than
    one managing controller.

  resourceVersion	<string>
    An opaque value that represents the internal version of this object that can
    be used by clients to determine when objects have changed. May be used for
    optimistic concurrency, change detection, and the watch operation on a
    resource or set of resources. Clients must treat these values as opaque and
    passed unmodified back to the server. They may only be valid for a
    particular resource or set of resources.
    
    Populated by the system. Read-only. Value must be treated as opaque by
    clients and . More info:
    https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#concurrency-control-and-consistency

  selfLink	<string>
    Deprecated: selfLink is a legacy read-only field that is no longer populated
    by the system.

  uid	<string>
    UID is the unique in time and space value for this object. It is typically
    generated by the server on successful creation of a resource and is not
    allowed to change on PUT operations.
    
    Populated by the system. Read-only. More info:
    https://kubernetes.io/docs/concepts/overview/working-with-objects/names#uids
```

특정 필드의 하위 필드를 한꺼번에 보기 위해서는 ``--recursive`` 옵션을 사용한다. pod 아래의 모든 하위 필드를 보기 위해서는 
다음 명령어를 사용한다.

```bash
$kubectl explain pods --rucursive
```

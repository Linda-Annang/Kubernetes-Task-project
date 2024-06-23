We need a cluster of 3 MariaDB database engines, deployed on kubernetes,
that can take client connections (select & insert & delete queries). All
database engines should have the same data(in sync with each other). Of
course, if one of the engines goes down, clients will reconnect and should be
routed to one of the other instances that is up.

To run 3 mariaDB on a kubernetes cluster, I employ the use of the galera cluster.
So I first create a statefulset to deploy the MariaDB pods with consistent naming and stable
storage. With the creation of persistent volume claim, I provision persistent storage for data,
enabling data persistence across restarts or pod failures. In addition, within the statefulset
manifest, I create a secret to secure the password for the environment.
I also create a headless service to expose endpoint in the cluster, and also to provide DNS
records for each pod to facilitate cluster communication.
Furthermore, to reroute users to other nodes when one of the engines or servers is down, I
provisioned the loadbalancer service to ensure high availability.
Finally, I also create a configmap for the cluster to make the mariaDB image reusable in all
environments or new engines.

PS: I added persistent volume claim manifest and deployment manifest for any other use

# Kubernetes Deployment Notes and Troubleshooting

These notes explain how `task kind_create` / `task k8s_deploy` behave and common troubleshooting steps you can use when deploying the app to a local KinD cluster.

- Host-port mappings and fallback:
    - The Kind configuration (`k8s/kind/kind-config.yml`) maps host ports (80, 443, 30090-30092) into the control-plane node so services are accessible from the host.
    - If any of those host ports are already in use on your machine (for example a local web server or another Kubernetes runtime), the PowerShell and shell helpers will detect the conflict and create the Kind cluster without hostPort mappings.
    - When host-port mappings are skipped, the app is still deployed inside the cluster — you'll need to port-forward or use `kubectl port-forward` (or access services via the cluster network) to reach them from your host.

- Postgres readiness and app startup:
    - The application depends on the Postgres pod becoming Ready before it can run Flyway migrations and bring up the Spring Boot application.
    - Sometimes the app starts before Postgres finishes initialization, which causes connection errors and a crash loop. In this setup the app will typically retry on pod restart (and succeed) once Postgres is Ready.
    - If the app repeatedly fails with messages like `Connection to spring-modular-monolith-postgres-svc:5432 refused`, check the Postgres pod logs (see commands below) and ensure it reaches `database system is ready to accept connections`.

- Quick troubleshooting commands
    - Show pods and their status:
        - `kubectl get pods -A -o wide`
    - Show the nodes
        - `kubectl get nodes`
    - Describe a failing pod to see events:
        - `kubectl describe pod <pod-name> -n <namespace>`
    - Show logs for a container (current + previous):
        - `kubectl logs <pod-name> -c <container-name>`
        - `kubectl logs <pod-name> -c <container-name> --previous`
    - If Postgres is not Ready, inspect its logs:
        - `kubectl logs <postgres-pod-name> -c postgres`
    - If hostPort mappings were skipped and you want to access the app locally:
        - Port-forward the service to localhost: `kubectl port-forward svc/spring-modular-monolith-svc 8080:8080`
        - Then visit: `http://localhost:8080`

- If you want hostPort mappings enforced
    - Free the host ports (80, 443, 30090-30092) on your machine, or edit `k8s/kind/kind-config.yml` to remove the hostPort entries.
    - Then recreate the cluster with `task kind_destroy` followed by `task kind_create`.


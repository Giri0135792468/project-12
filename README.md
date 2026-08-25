# Project 12 — Kubernetes Jobs & CronJobs

## Objective

Learn how Kubernetes runs:

* **One-time tasks using Jobs**
* **Scheduled tasks using CronJobs**

---

## Project Structure

```text
project-12/
└── k8s/
    ├── namespace.yaml
    ├── job.yaml
    └── cronjob.yaml
```

---

## Kubernetes Job

A **Job** runs a task until it completes successfully.

### Flow

```text
Job
 ↓
Creates Pod
 ↓
Pod runs the command
 ↓
Task completes
 ↓
Pod status = Completed
```

Our Job printed:

```text
Hello from Kubernetes Job
```

After completion:

```text
kubectl get jobs -n project-12
```

Output showed:

```text
hello-job   Complete   1/1
```

---

## Kubernetes CronJob

A **CronJob** runs Jobs automatically according to a schedule.

Our schedule:

```text
*/1 * * * *
```

Meaning:

> Run every minute.

### Flow

```text
CronJob
    ↓
Scheduled time
    ↓
Creates Job
    ↓
Job creates Pod
    ↓
Pod executes task
    ↓
Completed
```

Our CronJob printed:

```text
Hello from Kubernetes CronJob
```

---

## Main Difference

| Job                      | CronJob                     |
| ------------------------ | --------------------------- |
| Runs a task once         | Runs tasks on a schedule    |
| Creates a Pod            | Creates Jobs                |
| Pod completes after task | Jobs are created repeatedly |

---

## Important Commands

Apply all resources:

```bash
kubectl apply -f k8s/
```

Check all resources:

```bash
kubectl get all -n project-12
```

Check Jobs:

```bash
kubectl get jobs -n project-12
```

Check CronJobs:

```bash
kubectl get cronjob -n project-12
```

Check Pods:

```bash
kubectl get pods -n project-12
```

---

## Interview Summary

> **A Kubernetes Job is used to run a task until it completes successfully. A CronJob is used to create Jobs automatically according to a defined schedule.**

## Project 12 Status: ✅ Completed

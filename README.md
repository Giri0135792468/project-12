# Project 12 — Kubernetes Jobs & CronJobs

## Objective

Learn how Kubernetes runs:

* **One-time tasks using Jobs**
* **Scheduled tasks using CronJobs**

---

# 1. Kubernetes Job

A **Job** is used to run a task until it successfully completes.

Unlike a Deployment, a Job does not keep the application running forever.

### Flow

```text
Job
 ↓
Creates Pod
 ↓
Pod runs the task
 ↓
Task finishes
 ↓
Pod = Completed
 ↓
Job = Complete
```

### Real-world examples

* Database backup
* Database migration
* File processing
* Report generation
* Sending batch emails

---

## Job YAML

```yaml
apiVersion: batch/v1
kind: Job

metadata:
  name: hello-job
  namespace: project-12

spec:
  template:
    spec:
      restartPolicy: Never

      containers:
        - name: hello
          image: busybox

          command:
            - /bin/sh
            - -c
            - echo "Hello from Kubernetes Job"; sleep 5
```

### Important parts

```yaml
kind: Job
```

Creates a Kubernetes Job.

```yaml
restartPolicy: Never
```

The container will not restart after finishing.

```yaml
echo "Hello from Kubernetes Job"; sleep 5
```

The container prints the message, waits 5 seconds, and finishes.

---

# 2. Kubernetes CronJob

A **CronJob** automatically creates Jobs according to a schedule.

### Flow

```text
Scheduled Time
      ↓
CronJob
      ↓
Creates Job
      ↓
Job creates Pod
      ↓
Pod runs task
      ↓
Completed
```

### Real-world examples

* Daily database backup
* Hourly cleanup
* Weekly reports
* Scheduled data processing

---

## CronJob YAML

```yaml
apiVersion: batch/v1
kind: CronJob

metadata:
  name: hello-cronjob
  namespace: project-12

spec:
  schedule: "*/1 * * * *"

  jobTemplate:
    spec:
      template:
        spec:
          restartPolicy: Never

          containers:
            - name: hello
              image: busybox

              command:
                - /bin/sh
                - -c
                - echo "Hello from Kubernetes CronJob"
```

---

## Schedule

```yaml
schedule: "*/1 * * * *"
```

Cron format:

```text
Minute Hour Day-of-Month Month Day-of-Week
```

Our schedule:

```text
*/1 * * * *
```

means:

> Run every minute.

---

# Job vs CronJob

| Job            | CronJob                     |
| -------------- | --------------------------- |
| Runs once      | Runs on a schedule          |
| Creates a Pod  | Creates Jobs                |
| Task completes | Creates new Jobs repeatedly |

---

# Important Commands

Apply all YAML files:

```bash
kubectl apply -f k8s/
```

Check Jobs:

```bash
kubectl get jobs -n project-12
```

Watch Jobs:

```bash
kubectl get jobs -n project-12 --watch
```

Check CronJobs:

```bash
kubectl get cronjob -n project-12
```

Watch CronJobs:

```bash
kubectl get cronjob -n project-12 --watch
```

Check Pods:

```bash
kubectl get pods -n project-12
```

Watch Pods:

```bash
kubectl get pods -n project-12 --watch
```

Stop watching with:

```text
Ctrl + C
```

---

# Key Flow to Remember

```text
Job
 ↓
Pod
 ↓
Task
 ↓
Completed
```

```text
CronJob
 ↓
Job
 ↓
Pod
 ↓
Task
 ↓
Completed
```

## Interview Answer

> **A Kubernetes Job is used to run a task until it completes successfully. A CronJob is used to run Jobs automatically according to a defined schedule.**

## Project 12 Status: ✅ Completed

---
title: "Summaries"
linkTitle: "Summaries"
weight: 1
type: docs
description: >
    Prints Summary of currently working resources and their states
---



{{< alert color="success" title="TL;DR" >}}
- Get a Summary of Resources Running in the Cluster
{{< /alert >}}

# Summarizing Resources

## Motivation

Quickly summarizing a collection of Resources and their state.

Summarizing Resource State using a columnar format is the most common way to view cluster
state when developing applications or triaging issues.  The **columnar view gives a compact
summary of the most relevant information** for a collection of Resources.

## Get

The `kubectl get` reads Resources from the cluster and formats them as output.  The examples in
this chapter will query for Resources by providing Get the *Resource Type* as an argument.
For more query options see [Queries and Options](queries_and_options.md).

### Default

If no output format is specified, Get will print a default set of columns.

**Note:** Some columns *may* not directly map to fields on the Resource, but instead may
be a summary of fields.

```bash
kubectl get deployments nginx
```

```bash
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
nginx     1         1         1            0           5s
```

---

### Wide

Print the default columns plus some additional columns.

**Note:** Some columns *may* not directly map to fields on the Resource, but instead may
be a summary of fields.

```bash
kubectl get -o=wide deployments nginx
```

```bash
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE       CONTAINERS   IMAGES    SELECTOR
nginx     1         1         1            1           26s       nginx        nginx     app=nginx
```

---

### Custom Columns

Print out specific fields as Columns.

**Note:** Custom Columns can also be read from a file using `-o custom-columns-file`.

```bash
kubectl get deployments -o custom-columns="Name:metadata.name,Replicas:spec.replicas,Strategy:spec.strategy.type"
```

```bash
Name      Replicas   Strategy
nginx     1          RollingUpdate
```

---

#### Labels

Print out specific labels each as their own columns

```bash
kubectl get deployments -L=app
```

```bash
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE       APP
nginx     1         1         1            1           8m        nginx
```

---

### Show Labels

Print out all labels on each Resource in a single column (last).

```bash
kubectl get deployment --show-labels
```

```bash
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE       LABELS
nginx     1         1         1            1           7m        app=nginx
```

---

### Show Kind

Print out the Group.Kind as part of the Name column.

**Note:** This can be useful if the user did not specify the group in the command and
they want to know which API is being used.

```bash
kubectl get deployments --show-kind
```

```bash
NAME                          DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/nginx   1         1         1            1           8m
```
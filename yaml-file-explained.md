In a Deployment manifest, everything you listed falls under the top-level spec: block of the Deployment.

Here’s the structure (simplified tree view):

<img width="803" height="635" alt="image" src="https://github.com/user-attachments/assets/b3f677d4-022d-4e73-a750-6a2cec32c516" />

🔑 Key thing to remember:

The outer spec: belongs to the Deployment (defines replicas, selector, and pod template).

The inner spec: (under template) belongs to the Pod (defines containers, images, resources, volumes, etc.).

👉 Trainer tip: Think of it as nested specs:

Deployment spec → "How many Pods do I want and how to manage them?"

Pod spec (inside template) → "What’s inside each Pod?"

🔎 What is template in Kubernetes Deployment?

The template is basically the Pod blueprint.

It tells Kubernetes: “Whenever I need a new Pod, create it using this exact template.”

This template includes Pod metadata (labels, annotations) and the Pod spec (containers, volumes, etc.).

Key parts of template

metadata (inside template)

Labels and annotations applied to the Pods.

These must match the selector of the Deployment/ReplicaSet.

Example:

metadata:
  labels:
    app: nginx


👉 Without matching labels, the Deployment won’t recognize/manage those Pods.

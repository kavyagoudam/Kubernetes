Kubernetes: Node Selector, Affinity & Anti-Affinity: What's the Difference? 🤔

Ever wondered how Kubernetes decides where to place your pods? Let's break down three key concepts that influence this decision:

🎯 NODE SELECTOR
The simplest way to control pod placement.
It's like telling Kubernetes, "Hey, only put this pod on nodes with these specific labels!" 
For example, you might have some servers suitable for web apps and some suitable for training ML models. You can assign them labels like "app", "data-processing". Then in your backend app's Pod spec, you set NodeSelector to "app".
Node selector is the easiest, but also not super flexible.

🧲 AFFINITY
A more sophisticated version of Node Selector.
It's like saying, "I prefer these pods to run on nodes with these characteristics, but it's not mandatory. When possible, schedule X pod on Y node."
Affinity gives you more granular control and allows for soft preferences. It's great for scenarios like "Try to keep these pods together for better performance, but don't fail if it's not possible."
eg- You can use Affinity to always place your backend app pod and redis pod on the same worker node for low network latency.

🛑 ANTI-AFFINITY
The opposite of Affinity. It's Kubernetes' way of saying, "Keep these pods away from each other!"
This is super useful for high-availability setups. You can spread your app instances across different nodes to ensure your service stays up even if one node goes down.
eg- When deploying multiple nginx Pods, you can set anti-affinity in your Deployment Spec to ensure that all nginx pods are running on different nodes, hence making your load balancer highly available.

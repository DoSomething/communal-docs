## Spec changelog

### 1.2.0
Added a uuid to the `meta` property.

### 1.1.0
Added users `ip` to help with cross domain tracking. Note: Due to the fact the IP is added server side and deployments happening asynchronous, there is going to be a small disrepency between events that have the IP and the 1.1.0 spec version.

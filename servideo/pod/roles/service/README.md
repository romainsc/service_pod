Servidéo services pod role
=========

## References

- [Chapter 4. Running Containers as systemd Services with Podman Red Hat Enterprise Linux Atomic Host 7 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_atomic_host/7/html/managing_containers/running_containers_as_systemd_services_with_podman)
- [Building, running, and managing containers Red Hat Enterprise Linux 8 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/index)

## Service logic

* Almost everything is driven by the role variables values.
* Service is registered and deployed in user space.
* Port forwarding are system units
### Deployment (`state == present`)

1. Ensure the service user exists
2. If build is needed:
   1. Creates build directory
   2. Copy container definition file
   3. Build the image and store it in the user local registry
   4. Clean the build directory
3. Copy the service pod
4. You need to ensure all the service needed folders exist
5. Register the service as a systemd unit.
6. Start the service
7. Set up the port proxy systemd unit:
   * `socat` for UDP
   * `systemd-proxyd` for TCP
### Undeploy (`state == absent`)

1. Stop the port forwarding
2. remove the port forwarding unit
3. Stop the service
4. Killall user processes
5. Remove user
6. If specified, remove user directory while removing user

## Requirements

* [containers.podman](https://galaxy.ansible.com/containers/podman)

* [ikke_t.podman_container_systemd](https://galaxy.ansible.com/ikke_t/podman_container_systemd)

Role Variables
--------------

* Mandatory:
  
  * **service_user**: user the containerized service will be run on host.
  
  * **service_name**: Name of the systemd service
  
  * **service_container_image_name**: name of the image that will be pushed in the default registry.

* Optional:
  
  - **builddir**: Directory where the container will be built. Default “build”
  
  - **build_basedir**: Directory where the builddir will be created. Default “/tmp“
  
  - **ext_ip_address**: IP address where the container port will be exposed
  
  - **port_mapping**: Dictionnary with item as follow:
    
    - **exposed_port**: Port that will be exposed from the container to the host
    
    - **ext_port**: Port that will be mapped on the ext_ip_address.
    
    - **protocol**: Level 4 protocol (udp or tcp)

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

This role is intended to be used in other role, the main.yml is like the following:

```yaml
---
# tasks file for dns
- name: Create and run DNS pod service
  import_role:
    name: servideo.pod.service
```

But you will have to update the vars and Dockerfile.j2 template accordingly.

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

# 

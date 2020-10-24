DNS server for Servidéo
=========

## References

- [Chapter 4. Running Containers as systemd Services with Podman Red Hat Enterprise Linux Atomic Host 7 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_atomic_host/7/html/managing_containers/running_containers_as_systemd_services_with_podman)
- [Building, running, and managing containers Red Hat Enterprise Linux 8 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/index)

## Service logic

- Built on the target host

- run as non priviledged user

- Expose port  (`-p 1053:1053/udp` is your friend)

- Runned via systemd

- Deployed via the [podman_container_systemd role](https://galaxy.ansible.com/ikke_t/podman_container_systemd)

- UDP will be proxied with socat

- TCP is proxied via systemd-socket-proxyd

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

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

# 

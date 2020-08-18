# Ansible Role for EUDAQ2

This role installs and deploys EUEDAQ2 through Ansible.

## Requirements

This role currently only supports Ubuntu/Debian systems.

## Role Variables

The following role variables can be used to configure this role and select additional components of EUDAQ2 to be deployed. All variables are listed along with default values as defined in `defaults/main.yml`.

### EUDAQ2 source and target

* `eudaq_repository: "https://github.com/eudaq/eudaq.git"` - Git repository to fetch EUDAQ2 code from. Replace with your own repository if you have additional code which is not contained in the main EUDAQ2 repository.
* `eudaq_branch: v2.4.3` - EUDAQ2 version or Git branch to be deployed. The configured branch or tag has to exist in the selected repository.
* `eudaq_build_folder: /tmp/eudaq2` - Temporary folder for building EUDAQ2 on the target machine.
* `eudaq_install_folder: /usr/local` - Target installation folder of EUDAQ2 on the target machine.

### EUDAQ2 features

* `eudaq_build_executables: true` - Select whether executable files or only the library components of EUDAQ2 should be built
* `eudaq_build_gui: true` - Select whether the GUI should be built or not
* `eudaq_build_stdevent_monitor: true` - Select whether the StandardEvent Online Monitor should be built

### Additional features & producers

* `eudaq_build_tlu_aida: true` - Enable support for the AIDA-TLU. The following additional configuration settings allow to configure the dependencies and installation target directories for the Cactus uHAL library required for communicating with the AIDA-TLU:
    ```yml
    eudaq_tlu_aida:
      ipbus_repository: "https://github.com/ipbus/ipbus-software.git"
      ipbus_branch: v2.7.5
      ipbus_build_folder: /tmp/ipbus-software
      # if you change this, you need to make sure to update LD_LIBRARY_PATH to include cactus!
      ipbus_install_folder: /usr/local
      controlhub_environment_file: /usr/local/share/controlhub
    ```
* `eudaq_build_tlu_eudet: true` - Enable support for the EUDET-TLU. The following additional configuration settings allow to configure the source paths of the TLU firmware and the ZestSC1 USB driver required to communicate with the TLU. Files are copied from AFS either directly on the target machine or from a locally mounted AFS share on the Ansible machine to the target machine.
    ```yml
    eudaq_tlu_eudet:
      base_path: "/afs/desy.de/group/telescopes/tlu"
      zestsc1_path: "/afs/desy.de/group/telescopes/tlu/ZestSC1/linux"
      tlufirmware_path: "/afs/desy.de/group/telescopes/tlu/tlufirmware"
    ```
* `eudaq_build_eudet: true` - Enable support for the MIMOSA26/Ni DAQ of the EUDET-type beam telescopes
* `eudaq_build_caribou: false` - Enable support for the Caribou DAQ system. This requires the role `ansible-role-peary` as described below.
* `eudaq_build_pistage: false` - Enable support for the PI rotation and translation stages.


## Dependencies

Depending on the selected configuration of the EUDAQ2 build, additional roles might be required such as [`ansible-role-peary`](https://github.com/simonspa/ansible-role-peary) or [`ansible-role-root`]().

## Example Playbook

The following is an example playbook for installing EUDAQ2 and enabling AIDA-TLU support:

```yml
  - hosts: runcontrols

    vars:
      eudaq_build_tlu_aida: true

    roles:
      - role: ansible-role-eudaq
```

## License

BSD

## Author Information

Simon Spannagel (<simon.spannagel@desy.de>) DESY

# Example project for `ansible-builder`

This repo is just a sample to use for making custom execution environments to
use with `ansible-navigator` and the rest of the Ansible Automation Platform.

# Usage

Clone this repo to your workspace and manipulate the files to make your own
changes for your project.

```shell
$ git clone https://github.com/Caseraw/ansible-builder-example-project.git
```

## Structure

```shell
.
├── context                     [1]
│   ├── _build
│   │   ├── ansible.cfg
│   │   ├── bindep.txt
│   │   ├── requirements.txt
│   │   └── requirements.yml
│   └── Containerfile
├── ansible.cfg                 [2]
├── bindep.txt                  [3]
├── requirements.txt            [4]
├── requirements.yml            [5]
├── execution-environment.yml   [6]
└── README.md
```

1. The context directory is generated every `ansible-builder` build run and
   contains all the files for building the execution environment image.
2. The [Ansible configuration](https://docs.ansible.com/ansible/latest/installation_guide/intro_configuration.html) file.
3. File containing system-level dependencies to install during the multi staged
   build using [bindep](https://docs.opendev.org/opendev/bindep/latest/readme.html).
4. File containing [Python pip](https://pip.pypa.io/en/stable/user_guide/#requirements-files) dependencies.
5. File containing [Ansible Roles and Collection](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html) dependencies.
6. File containing the [Ansible Builder definition](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html/ansible_builder_guide/assembly-using-builder#con-building-definition-file) file.

## Build

When ready, build the execution environment from within your project directory.
The `ansible-builder` comes with several options, this example sticks to the
defaults.

```shell
$ ansible-builder build
Running command:
  podman build -f context/Containerfile -t ansible-execution-env:latest context
Complete! The build context can be found at: /path/to/your/project/context
```

When the multi staged build is done, the following images should show up.

```shell
$ podman images
REPOSITORY                       TAG         IMAGE ID      CREATED         SIZE
localhost/ansible-execution-env  latest      653a3b682ea3  45 minutes ago  895 MB     [1]
<none>                           <none>      cdeca837b865  45 minutes ago  1.12 GB    [2]
<none>                           <none>      7085c4626412  46 minutes ago  831 MB     [3]
quay.io/ansible/ansible-runner   <none>      f8c5935ee58c  3 hours ago     807 MB     [4]
quay.io/ansible/ansible-builder  <none>      b0348faa7f41  6 weeks ago     779 MB     [5]
```

1. The newly built execution environment, packed in an Ansible Runner image.
2. Multi staged image build artifact.
3. Multi staged image build artifact.
4. Original Ansible Runner image used for the build.
6. Original Ansible Builder image used for the build.

# Resources

For more information follow these links:

- [Introduction to Ansible Builder](https://www.ansible.com/blog/introduction-to-ansible-builder)
- [Ansible Builder docs](https://ansible-builder.readthedocs.io/en/stable/)
- [Ansible Runner docs](https://ansible-runner.readthedocs.io/en/stable/)
- [Ansible Navigator docs](https://ansible-navigator.readthedocs.io/)
- [Red Hat Ansible Automation Platform 2.1](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1)
- [Red Hat Ansible Automation Platform 2.1 - Ansible Builder Guide](https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.1/html-single/ansible_builder_guide/index)
- [Ansible Collection index](https://docs.ansible.com/ansible/latest/collections/index.html)
- [Ansible Modules and Plugins index](https://docs.ansible.com/ansible/latest/collections/all_plugins.html)
`---`

`- name: Configure Elk VM with Docker`
  
`hosts: elk`

`remote_user: ansible`

`tasks:`

`## Use apt module`
  
    - name: Install docker.io
      apt:
        force_apt_get: yes
        name: docker.io
        state: present

`## Use apt module`

    - name: Install python3-pip
      apt:
        force_apt_get: yes
        name: python3-pip
        state: present

`## Use pip module`

    - name: Install Docker module
      pip:
        name: docker
        state: present

`## Use command module`

    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

`## Use shell module`
    
    - name: Increase virtual memory on automatically on VM restart
      shell: echo "vm.max_map_count=262144" >> /etc/sysctl.conf

`## Use docker_container module`

    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always

`## The ports the ELK runs on`

        published_ports:
          - 5601:5601
          - 9200:9200
          - 5044:5044


`## End of script`
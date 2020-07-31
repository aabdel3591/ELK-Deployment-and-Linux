    ---
    - name: Install and Launch Filebeat
      hosts: webservers
      become: yes
      tasks:
      - name: Download filebeat .deb file`
        command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.8.0-amd64.deb

      - name: Install filebeat .deb
        command: dpkg -i filebeat-7.8.0-amd64.deb

      - name: Drop in filebeat.yml
        copy:
          src: /etc/ansible/filebeat-config.yml
          dest: /etc/filebeat/filebeat.yml

      - name: Change owner of filebeat.yml
        command: chown root:root /etc/filebeat/filebeat.yml

      - name: Enable and Configure System Module
        command: sudo filebeat modules enable system

      - name: Setup filebeat
        command: filebeat setup

      - name: Start filebeat servce
        command: service filebeat start


    ## Installing Metricbeat

    - name: Install and Launch Metricbeat`
      hosts: webservers
      become: yes
      tasks:
      - name: Download metricbeat .deb file
        command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb

      - name: Install metricbeat .deb
        command: dpkg -i metricbeats-7.4.0-amd64.deb

      - name: Drop in metricbeat.yml
        copy:
          src: /etc/ansible/metricbeat-config.yml
          dest: /etc/metricbeat/metricbeat.yml

      - name: Change owner of metricbeat.yml
        command: chown root:root /etc/metricbeat/metricbeat.yml

      - name: Enable and Configure System Module
        command: sudo metricbeat modules enable docker

      - name: Setup metricbeat
        command: metricbeat setup

      - name: Start metricbeat service
        command: service metricbeat start


    ## End of script
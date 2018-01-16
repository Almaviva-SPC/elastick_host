# Ansible Role: Kibana

An Ansible Role that installs Kibana on RedHat/CentOS or Debian/Ubuntu.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    kibana_version: "5.6"

The version of kibana to install (major and minor only).

    kibana_server_port: 5601
    kibana_server_host: "0.0.0.0"

The FQDN or IP address and port Kibana should use.

    kibana_elasticsearch_url: "http://localhost:9200"
    - We use parameter for kibana_elasticsearch_url defined inside hosts file under elastic_server_master

The URL (including port) over which Kibana will connect to Elasticsearch.

## Dependencies

None.

## Example Playbook

    - hosts: kibana
      roles:
        - ansible-kibana

## License

MIT / BSD

## Author Information

This role was created in 2018 by [Gianluca Del Brusco && Giovanni Alo], author of [Ansible for DevOps](https://www.ansiblefordevops.com/).

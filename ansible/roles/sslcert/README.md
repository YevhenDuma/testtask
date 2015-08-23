# SSLCERT role
## Role to create SSL self-signed certificate.

This role simple generate ssl certificate, with all needed data stored in playbook and changes permissions and ownership for key, according to data in playbook (preffered value is same user as apache - www-data for ubuntu and o-rwx - to remove read/write/permissions for others)

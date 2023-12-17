# Cassandra Security Considerations

Cassandra, out of the box, does not provide any security settings. Therefore, the basic Cassandra settings expose the platform to potential attacks, putting data at significant risk of exposure. It is advised to carefully define security measures when planning to store critical data.

Without proper settings, malicious users can easily:

- Craft internode messages to insert users into the authentication schema
- Craft internode messages to truncate or drop the schema
- Use tools such as sstableloader to overwrite system_auth tables
- Attach to the cluster directly to capture write traffic

## Security Components in Cassandra

There are three main components to the security features provided by Cassandra:

1. TLS/SSL encryption for client and inter-node communication
2. Client authentication
3. Authorization

All these settings are configured in the `cassandra.yaml` file, where the settings to be enabled and their properties are defined. A detailed description and the method of configuring these precautions can be found in Cassandra's documentation, specifically in the "Cassandra Security" section[[1.1]](https://cassandra.apache.org/doc/stable/cassandra/operating/security.html).

The most common attacks and issues that occur in Cassandra databases include:

- Data theft
- Execution of arbitrary commands
- Denial of Service (DOS)


References:
- [1.1 Cassandra Security documentation](https://cassandra.apache.org/doc/stable/cassandra/operating/security.html)
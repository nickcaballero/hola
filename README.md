[ ![Download](https://api.bintray.com/packages/nickcaballero/maven/hola/images/download.svg) ](https://bintray.com/nickcaballero/maven/hola/_latestVersion)

Hola
====

Hola is a minimalist Java implementation of Multicast DNS Service Discovery (mDNS-SD). The purpose of Hola is to give Java developers a dead-simple API for finding Zeroconf-enabled services on a local network. It follows RFCs [6762](https://tools.ietf.org/html/rfc6762) and [6763](https://tools.ietf.org/html/rfc6763) and is compatible with Apple's [Bonjour](https://developer.apple.com/bonjour/) mDNS-SD implementation.

# Features

Hola is a work-in-progress. The following features are currently supported:

 - Browse (synchronously) for instances of services on a local network
 - Retrieve information about discovered services, including network addresses, ports, and user-friendly names
 - Supports both IPv4 and IPv6 networks

# API Example

To search for services, create a `Query` specifying the type of service you're looking for and the domain to search. You can execute a blocking search with the `runOnce()` method; this will return a list of `Instance` objects representing the discovered instances. As an example, the following code  will search for TiVo devices on the user's local network:

    public class TivoFinder {
        final static Logger logger = LoggerFactory.getLogger(TivoFinder.class);

        public static void main(String[] args) {
            try {
                Service service = Service.fromName("_tivo-mindrpc._tcp");
                Query query = Query.createFor(service, Domain.LOCAL);
                List<Instance> instances = query.runOnce();
                instances.stream().forEach(System.out::println);
            } catch (UnknownHostException e) {
                logger.error("Unknown host: ", e);
            } catch (IOException e) {
                logger.error("IO error: ", e);
            }
        }
    }

Each `Instance` will have a user-visible name, a list of IP addresses, a port number, and a set of attributes:

    String userVisibleName = instance.getName();
    List<InetAddress> addresses = instance.getAddresses();
    int port = instance.getPort();
    if (instance.hasAttribute("platform")) {
        String platform = instance.lookupAttribute("platform");
    }

An asynchronous `run()` method is planned for performing a continuous service discovery operation, but this feature is not yet implemented.

# Requirements

Hola requires Java 8 or higher. It handles logging via SLF4J, so the slf4j-api.jar must also be in your Hola-enabled project's class path.

# License

Hola is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.
# Project 6 - DNS Server

## Group Members
Joseph Hirsch

## High-Level Approach
For this project, functionality was implemented in the `Server` and `Query` classes. These are defined and instantiated in the `4700dns` file.

### `Server`

The `Server` class is responsible for handling DNS queries and responses. It
implements both authoritative and recursive DNS server functionalities. When
initialized, it sets up the socket, loads the zone file, and initializes
necessary variables. The `run` method enters a loop where it listens for
incoming DNS requests and processes them accordingly.

### `Query`

The `Query` class is responsible for maintaining the state of an ongoing
recursive DNS query. It keeps track of the query ID, the question, the client
address, and other useful state. It allows many queries to be handled in
parallel by the server by encapsulating the state of each query in separate
objects. This is essential because UDP is connectionless.

#### Key Methods

- `parse_zone_file`: Loads DNS records from the specified zone file.
- `send`: Sends a DNS response to a specified address.
- `ask_question`: Sends a DNS query to another DNS server.
- `recv`: Receives DNS requests and responses from the socket.
- `handle_message`: Determines whether a message is a request or a response and processes it accordingly.
- `handle_request`: Processes incoming DNS requests, either authoritatively,
  recursively, or from the cache.
- `handle_response`: Processes incoming DNS responses and either continues the recursion or sends the response to the client.
- `resend_old_queries`: Resends queries that have not received a response within a timeout period.
- `fail_old_queries`: Sends a SERVFAIL response for queries that have timed out after multiple attempts.

## What I think is good about the design

- The design has robust and accurate caching of every DNS record it encounters.
- The server handles CNAME chaining and bailiwick checks smoothly and securely.
- The server resends old queries and handles timeouts gracefully, picking up
  where it left off when resending.
- There is a robust logging system that logs all DNS requests and responses, as
  well as useful state data, in a separate file for each client query and
  its corresponding recursion.

## Challenges

The biggest challenge in this project was ensuring the correct handling of CNAME
chaining and bailiwick checks. Additionally, implementing a robust caching
mechanism that behaves the same as it would without the cache was difficult.
Finally, there were various edge cases in the handling of DNS behavior which
were difficult to manage without the code becoming very complicated.

## Testing

I tested my implementation by running the server with the various zone files and
DNS queries. These simulated different network conditions and verified the
correctness of the responses, as well as testing the server's ability to handle
timeouts and resend queries.
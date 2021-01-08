# NAME

Kafka::Consumer::Avro - Avro message consumer for Apache Kafka.

# SYNOPSIS

    use Kafka qw/DEFAULT_MAX_BYTES/;
    use Kafka::Connection;
    use Kafka::Consumer::Avro;
    use Confluent::SchemaRegistry;
    
    my $connection = Kafka::Connection->new( host => 'localhost' );
    
    my $consumer = Kafka::Consumer::Avro->new( Connection => $connection , SchemaRegistry => Confluent::SchemaRegistry->new() );
    
    # Consuming messages
    my $messages = $consumer->fetch(
          'mytopic',            # topic
          0,                    # partition
          0,                    # offset
          $DEFAULT_MAX_BYTES    # Maximum size of MESSAGE(s) to receive
    );
    
    if ($messages) {
          foreach my $message (@$messages) {
                  if ( $message->valid ) {
                          say 'payload    : ', $message->payload;
                          say 'key        : ', $message->key;
                          say 'offset     : ', $message->offset;
                          say 'next_offset: ', $message->next_offset;
                  }
                  else {
                          say 'error      : ', $message->error;
                  }
          }
    }
    
    # Closes the consumer and cleans up
    undef $consumer;
    $connection->close;
    undef $connection;

# DESCRIPTION

`Kafka::Consumer::Avro` main feature is to provide object-oriented API to 
consume messages according to _Confluent SchemaRegistry_ and _Avro_ serialization.

`Kafka::Consumer::Avro` inerhits from and extends [Kafka::Consumer](https://metacpan.org/pod/Kafka%3A%3AConsumer).

# INSTALL

Installation of `Kafka::Consumer::Avro` is a canonical:

    perl Makefile.PL
    make
    make test
    make install

## TEST NOTES

Tests are focused on verifying Avro-formatted messages and theirs interactions with Confluent Schema Registry and are intended to extend `Kafka::Consumer` test suite.

They expect that in the target machine are available Kafka and Schema Registry listening on `localhost` and default ports, otherwise most of the test are skipped.

# USAGE

## CONSTRUCTOR

### `new`

Creates new consumer client object.

`new()` takes arguments in key-value pairs as described in [Kafka::Consumer](https://metacpan.org/pod/Kafka%3A%3AConsumer) from which it inherits.

In addition, takes in the following arguments:

- `SchemaRegistry => $schema_registry` (**mandatory**)

    Is a [Confluent::SchemaRegistry](https://metacpan.org/pod/Confluent%3A%3ASchemaRegistry) instance.

## METHODS

The following methods are defined for the `Kafka::Avro::Consumer` class:

### `schema_registry`()

Returns the [Confluent::SchemaRegistry](https://metacpan.org/pod/Confluent%3A%3ASchemaRegistry) instance supplied to the construcor.

### `get_error`()

Returns a string containing last error message.

### `fetch( %params )`

Gets messages froma a Kafka topic.

Please, see [Kafka::Consumer](https://metacpan.org/pod/Kafka%3A%3AConsumer-%3Efetch%28%29) for more details.

# AUTHOR

Alvaro Livraghi, <alvarol@cpan.org>

# CONTRIBUTE

[https://github.com/alivraghi/Kafka-Consumer-Avro](https://github.com/alivraghi/Kafka-Consumer-Avro)

# BUGS

Please use GitHub project link above to report problems or contact authors.

# COPYRIGHT AND LICENSE

Copyright 2018 by Alvaro Livraghi

This program is free software; you can redistribute it and/or modify it under the same terms as Perl itself.

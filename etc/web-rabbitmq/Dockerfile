FROM rabbitmq:latest

RUN apt-get update

RUN apt-get install -y curl

RUN curl http://www.rabbitmq.com/community-plugins/v3.6.x/rabbitmq_delayed_message_exchange-0.0.1.ez > $RABBITMQ_HOME/plugins/rabbitmq_delayed_message_exchange-0.0.1.ez

RUN rabbitmq-plugins enable rabbitmq_management

RUN rabbitmq-plugins enable rabbitmq_delayed_message_exchange

RUN rabbitmq-plugins enable rabbitmq_consistent_hash_exchange
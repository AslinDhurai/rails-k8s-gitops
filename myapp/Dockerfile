FROM ruby:3.2.2

RUN apt-get update -qq && apt-get install -y nodejs postgresql-client

WORKDIR /myapp

COPY Gemfile* ./
RUN ls -l && cat Gemfile
RUN bundle install

COPY . .

CMD ["rails", "server", "-b", "0.0.0.0"]


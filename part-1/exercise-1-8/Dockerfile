# Start from the ubuntu image
FROM ubuntu:18.04

# Use /usr/src/app as our workdir. The following instructions will be executed in this location.
WORKDIR /usr/src/app

# Copy the input.sh file from this location to /usr/src/app/ creating /usr/src/app/input.sh
COPY input.sh .

#  execution permissions during the build
RUN chmod +x input.sh

RUN apt-get update
RUN apt-get install curl -y

CMD ./input.sh


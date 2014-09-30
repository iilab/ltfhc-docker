FROM debian:wheezy

MAINTAINER Jun Matsushita jun@iilab.org

# Install python3
RUN     apt-get update
RUN     apt-get install -y python3

# Copy html
VOLUME ["/src"]
WORKDIR /src

# Run http server on port 8080
EXPOSE  8080
CMD ["python3", "-m", "http.server", "8080"]

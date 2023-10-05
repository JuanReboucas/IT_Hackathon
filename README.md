# IT_Hackathon

Grupo
Ramon Bento;
Juan Rebouças;
Comandos:
vagran init;
vagrant up;
docker-compose up;
docker network create minha-rede;

version: '3'
services:
  mongodb:
    image: mongo:latest
    container_name: mongodb
    volumes:
      - mongo_data:/data/db
    ports:
      - "27017:27017"
    networks:
      - minha-rede

  go-app:
    image: juanarsam/hackgolang:latest
    container_name: go-app
    ports:
      - "9080:9080"
    networks:
      - minha-rede
    depends_on:
      - mongodb

networks:
  minha-rede:
    driver: bridge

volumes:
  mongo_data:



  # syntax=docker/dockerfile:1

FROM golang:1.19

# Set destination for COPY
WORKDIR /app

# Download Go modules
COPY go.mod go.sum ./
RUN go mod download

# Copy the source code. Note the slash at the end, as explained in
# https://docs.docker.com/engine/reference/builder/#copy
COPY *.go ./

# Build
RUN CGO_ENABLED=0 GOOS=linux go build -o /docker-gs-ping

# To bind to a TCP port, runtime parameters must be supplied to the docker command.
# But we can (optionally) document in the Dockerfile what ports
# the application is going to listen on by default.
# https://docs.docker.com/engine/reference/builder/#expose
EXPOSE 8080

# Run
CMD [ "/docker-gs-ping" ]


Vagrant.configure("2") do |config|

  # Maquina 01 (Servidor1)

  config.vm.define "servidor1" do |servidor1|

    servidor1.vm.box = "ubuntu/trusty64"

    servidor1.vm.network "private_network", type: "static", ip: "192.168.0.3"

    servidor1.vm.hostname = "Juan ReboucasRamon Bento"

    servidor1.vm.provision "shell", inline: <<-SHELL

      sudo apt-get update

      sudo apt-get install -y curl vim htop

    SHELL

  end

 

 

 

 

 

   # Maquina 02 (Servidor2)

   config.vm.define "servidor2" do |servidor2|

    servidor2.vm.box = "centos/7"

    servidor2.vm.network "private_network", type: "static", ip: "192.168.0.4"

    servidor2.vm.hostname = "Ramon BentoJuan Reboucas"

    servidor2.vm.provider "virtualbox" do |vb|

      vb.memory = 1024

      vb.cpus = 1

    end

    servidor2.vm.provision "shell", inline: <<-SHELL

      sudo yum install -y epel-release

      sudo yum install -y docker git wget

      sudo yum install -y docker-compose

      sudo systemctl start docker

      sudo systemctl enable docker

    SHELL

  end

end

 

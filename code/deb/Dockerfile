FROM debian:buster

RUN apt-get update -y \                                                                                        
    && apt-get install -y \                                                                                    
    	postgresql \
	postgresql-contrib \
	python3 \
	python3-pip \
	git \
	curl \
    && rm -rf /var/lib/apt/lists/*

sudo curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add

# Create the repository configuration file:
sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'

#
# Install pgAdmin
#

# Install for both desktop and web modes:
sudo apt-get install -y pgadmin4


# Configure the webserver, if you installed pgadmin4-web:
#sudo /usr/pgadmin4/bin/setup-web.sh

RUN git clone https://github.com/mattwhite180/mattwhite180/
RUN cp mattwhite180/configFiles/bashrc ~/.bashrc
RUN cp mattwhite180/configFiles/bash_prompt.sh ~/.bash_prompt.sh 
RUN rm -rf mattwhite180/


RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY requirements.txt .

RUN pip3 install --no-cache-dir -r requirements.txt

COPY . .

CMD echo "Done!"

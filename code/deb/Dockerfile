FROM debian:buster

RUN apt-get update -y \                                                                                        
    && apt-get install -y \
	python3 \
	python3-pip \
	git \
	vim \
	neovim \
	curl \
	wget \
	gnupg \
	gnupg2 \
	software-properties-common

# Create the file repository configuration:
RUN sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Import the repository signing key:
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

# Update the package lists:
RUN apt-get update

# Install the latest version of PostgreSQL.
# If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
RUN apt-get -y install postgresql-12

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

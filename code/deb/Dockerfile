FROM debian:buster

RUN apt-get update -y \                                                                                        
    && apt-get install -y \                                                                                    
    	postgresql \
	postgresql-contrib \
	python3 \
	python3-pip \
	git \
	vim \
    && rm -rf /var/lib/apt/lists/*


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

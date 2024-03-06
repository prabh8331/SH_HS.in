## Anaconda setup on ubuntu server

<!-- https://docs.anaconda.com/free/anaconda/install/linux/ -->

https://docs.anaconda.com/free/miniconda/


```bash
source ~/.bashrc
conda activate base


# conda install jupyter

sudo ufw allow 8888


# cd /home/userver/DataScience

# jupyter notebook --no-browser --ip=0.0.0.0 --port=8888


# http://your_server_ip:8888/?token=your_token_here


conda deactivate
```

```bash
conda update --all
```


Docker Installation 
https://docs.anaconda.com/free/anaconda/applications/docker/

```bash
# sudo docker run -i -t -p 8888:8888 -v /home/userver:/opt/notebooks --restart always continuumio/miniconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser --allow-root"

# sudo docker run -i -t -p 8888:8888 -v /home/userver:/opt/notebooks --restart always continuumio/miniconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser --allow-root && chown -R userver:userver /opt/notebooks"
# docker run -i -t -p 8888:8888 -v /home/userver:/opt/notebooks --restart always continuumio/miniconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser --allow-root && chown -R userver:userver /opt/notebooks"
docker run -i -t -p 8888:8888 -v /home/userver:/opt/notebooks --restart always continuumio/miniconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser chown -R userver:userver /opt/notebooks"



copy token and paste in 
http://192.168.1.111:8888/lab

```
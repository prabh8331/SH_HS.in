# Simplilearn Data Science Program

## Anaconda setup on ubuntu server

https://docs.anaconda.com/free/anaconda/install/linux/

https://docs.anaconda.com/free/miniconda/

```bash
source ~/.bashrc
conda activate base


conda install jupyter

sudo ufw allow 8888


cd /home/userver/DataScience

jupyter notebook --no-browser --ip=0.0.0.0 --port=8888


http://your_server_ip:8888/?token=your_token_here


conda deactivate
```



Update Conda: THis in case of errors
```bash
conda update conda

conda update anaconda

conda update jupyter notebook

conda update --all

conda create -n myenv python=3.11
conda activate myenv
conda install jupyter notebook

jupyter notebook --no-browser --ip=0.0.0.0 --port=8888
```

Docker Installation 
https://docs.anaconda.com/free/anaconda/applications/docker/

```bash
sudo docker run -i -t -p 8888:8888 continuumio/miniconda3 /bin/bash \
-c "/opt/conda/bin/conda install jupyter -y --quiet && mkdir \
/opt/notebooks && /opt/conda/bin/jupyter notebook \
--notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 \
--no-browser --allow-root"

# sudo docker run -i -t -p 8888:8888 -v /home/userver/DataScience:/opt/notebooks --restart always continuumio/miniconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser --allow-root"

# sudo docker run -i -t -p 8888:8888 -v /home/userver:/opt/notebooks --restart always continuumio/miniconda3 /bin/bash -c "source activate base && /opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser --allow-root"


# run this wiyh out sudo to use userer user previlages
sudo usermod -aG docker $USER
sudo chown $USER:docker /var/run/docker.sock


# sudo docker run -i -t -p 8888:8888 -v /home/userver:/opt/notebooks --restart always continuumio/miniconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser --allow-root"


# docker run -i -t -p 8888:8888 -v /home/userver:/opt/notebooks --user $(id -u):$(id -g) --restart always continuumio/miniconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser --allow-root"


sudo chown -R userver:userver /home/userver/DataScience
ls -l /home/userver/DataScience 

sudo docker run -i -t -p 8888:8888 -v /home/userver:/opt/notebooks --restart always continuumio/miniconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip=0.0.0.0 --port=8888 --no-browser --allow-root"

```



setup git
```bash
# open terminal in jupyter
cd /opt/notebooks

git config --global --add safe.directory /opt/notebooks/DataScience
git status
git pull origin master


git config --global user.name "prabh8331"
git config --global user.email "prabh8331@gmail.com"

ssh-keygen -t rsa -b 4096 -C "prabh8331@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa

### the the public key from here and add it in github
cat ~/.ssh/id_rsa.pub


# use this in cell in jupiter notebook
!git status

!git pull origin master

# Example commands in a Jupyter Notebook cell
!git add .
!git commit -m "Your commit message"
!git push origin master


```

http://192.168.1.111:8888/

https://jup.prabshhs.in/tree

https://jup.prabshhs.in/lab
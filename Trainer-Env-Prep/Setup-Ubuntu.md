# Trainer Tasks

```
sudo apt -y update
sudo apt -y install openssl tree ranger zip wget unzip gcc
```

## Add Swap Space
- Create new Premium SSD and attach to the VM
- Increase main disk space of VM to 16 GB
- Follow: https://devconnected.com/how-to-add-swap-space-on-ubuntu-20-04/
- Run below commands:
```
sudo swapon --show
sudo fdisk -l
sudo fdisk /dev/sdc
	# Specify n command and create primary partition
	# Specify t command and use 82 code to change partition type to Swap
	# Specify w to write the changes
sudo mkswap /dev/sdc1
sudo swapon /dev/sdc1
sudo swapon --show
sudo blkid | grep "swap"
sudo nano /etc/fstab
	# UUID=<copied value>   none   swap  defaults   0   0
sudo reboot
sudo swapon --show
```

## Setup multiple users in Ubuntu
- For each participant, we need to setup login accounts
```
for ((i=1;i<=50;i++)); do
	export username="u$i"
	sudo useradd -m -p "p2" $username;sudo usermod -aG sudo $username;sudo echo $username:p | sudo /usr/sbin/chpasswd;sudo chown -R  $username:root /home/$username
done
```

- Customize linux prompt
```
cat ~/.bashrc
echo 'export PS1="\e[0;31m\e[50m\u@\h\n\w \e[m\n$ "'   >> ~/.bashrc
cat ~/.bashrc
exit
```

## Create Virtual Env
```
sudo python3 -m venv /opt/pyspark_venv
chmod -R a+w /opt/pyspark_venv
source /opt/pyspark_venv/bin/activate
```

## Install Spark
```
sudo apt install default-jdk scala git -y
java -version; javac -version; scala -version; git --version
wget https://archive.apache.org/dist/spark/spark-3.0.1/spark-3.0.1-bin-hadoop2.7.tgz
tar xvf spark-*
sudo mv spark-3.0.1-bin-hadoop2.7 /opt/spark
echo "export SPARK_HOME=/opt/spark" >> ~/.profile
echo 'export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin' >> ~/.profile
echo "export PYSPARK_PYTHON=/opt/pyspark_venv/bin/python" >> ~/.profile
source ~/.profile
```

## Start Spark
```
start-master.sh
curl http://127.0.0.1:8080/
start-slave.sh spark://localhost:7077
```

## Test Spark Shell
```
spark-shell
```

## Test Python in Spark
```
pyspark
```

## Basic Commands to Start and Stop Master Server and Workers
```
start-master.sh
stop-master.sh
stop-slave.sh
start-slave.sh
start-all.sh
stop-all.sh
```

## Install PySpark packages in Python
```
source /opt/pyspark_venv/bin/activate
pip install pyspark
pip install findspark
```

##  Clone Git Repo
```
cd
git clone
cd
```

## Install Jupyter Notebook
```
source /opt/pyspark_venv/bin/activate
pip install jupyter
```

## Start Jupyter Notebook
```
source /opt/pyspark_venv/bin/activate
cd
nohup jupyter notebook --ip=0.0.0.0 &
```

### Initialize Spark Environment in Jupyter Notebook
```
import findspark
findspark.init("/opt/spark")
import pyspark
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
spark

data = [('James','','Smith','1991-04-01','M',3000),
  ('Michael','Rose','','2000-05-19','M',4000),
  ('Robert','','Williams','1978-09-05','M',4000),
  ('Maria','Anne','Jones','1967-12-01','F',4000),
  ('Jen','Mary','Brown','1980-02-17','F',-1)
]

columns = ["firstname","middlename","lastname","dob","gender","salary"]
df = spark.createDataFrame(data=data, schema = columns)
df.show()
```

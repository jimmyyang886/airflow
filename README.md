# In Unbuntu airflow installation(MySQL)

# enviroment  
mkdir -p \~/airflow  
vim \~/.bashrc    
export AIRFLOW_HOME=\~/airflow  
export SLUGIFY_USES_TEXT_UNIDECODE=yes  
source ~/.bashrc  

# virtualenv(Anaconda is installed) and pip install
conda create --name airflow-env python='3.7'  
conda activate airflow-env  
pip install apache-airflow  
pip install "airflow[mysql, crypto]"  

# Airflow initialization by MySQL
mysql>CREATE DATABASE airflow CHARACTER SET utf8 COLLATE utf8_unicode_ci;  
mysql>create user 'airflow'@'localhost' identified by ‘airflow’;  
mysql>grant all privileges on airflow.* to 'airflow'@'localhost';  
mysql>flush privileges;  
mysql>quit  

#Update the airflow.cfg file (should be available in ~/airflow/ directory)  
sql_alchemy_conn = mysql://airflow:airflow@localhost/airflow  

\#initialize database and start webserver  
airflow initdb  
airflow webserver  

\#open web browser (ip:8080)

# systemd(optional)
sudo vim /etc/systemd/system/airflow-webserver.service

 \# Unless required by applicable law or agreed to in writing,  
 \# software distributed under the License is distributed on an  
 \# “AS IS” BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY  
 \# KIND, either express or implied. See the License for the  
 \# specific language governing permissions and limitations  
 \# under the License.  
  [Unit]
  Description=Airflow webserver daemon  
  After=network.target mysql.service  
  Wants=mysql.service  
  [Service]  
  Environment= /home/user/airflow   
  User=user  
  Group=user   
  Type=simple  
  ExecStart=/bin/bash -c 'source /home/spark/anaconda3/bin/activate airflow-env && airflow webserver'    
  Restart=on-failure  
  RestartSec=5s  
  PrivateTmp=true  
  [Install]
  WantedBy=multi-user.target  

# start service by following command
sudo systemctl daemon-reload  
sudo service airflow-webserver start  
sudo service airflow-webserver status  

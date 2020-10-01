# In Unbuntu airflow installation(MySQL)

1. enviroment
vim \~/.bashrc  
mkdir -p \~/airflow  
export AIRFLOW_HOME=\~/airflow  
export SLUGIFY_USES_TEXT_UNIDECODE=yes  
source ~/.bashrc  

2. virtualenv(Anaconda is installed and )
conda create --name airflow-env python='3.7'  
conda activate airflow-env  
pip install apache-airflow  
pip install "airflow[mysql, crypto]"  

3. MySQL
mysql>CREATE DATABASE airflow CHARACTER SET utf8 COLLATE utf8_unicode_ci;
mysql>create user 'airflow'@'localhost' identified by ‘airflow’;
mysql>grant all privileges on airflow.* to 'airflow'@'localhost';
mysql>flush privileges;
mysql>quit

4. *Update the airflow.cfg file (should be available in ~/airflow/ directory.
sql_alchemy_conn = mysql://airflow:airflow@localhost/airflow

5. systemd
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
  Environment= /home/spark/airflow   
  User=user  
  Group=user   
  Type=simple  
  ExecStart= /bin/bash -c 'conda activate airflow-env && airflow webserver'  
  Restart=on-failure  
  RestartSec=5s  
  PrivateTmp=true  
  [Install]
  WantedBy=multi-user.target  

6. start service by following command
sudo systemctl daemon-reload  
sudo service airflow-webserver start  
sudo service airflow-webserver status  














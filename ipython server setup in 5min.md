# IPython Notebook Server Configuration in 5 minutes or less

### Starting from a fresh Ubuntu 14.4 EC2 instance...

1) Open security group ports 22/ssh, 80/http and 443/https.
2) Make sure you have the credentials key file to ssh into the machine.
3) Install the boiler-plate stuff:

``bash
sudo apt-get install git emacs python3.4 python3-dev python-virtualenv liblapack-dev libatlas-dev gfortran nginx
``

4) Create a local virtual environment:

``bash
virtualenv venv -p python3.4
source venv/bin/activate
pip install numpy pandas scipy scikit-learn boto tornado pyzmq jinja2 requests ipython jsonschema
``

5) Create a self-assigned certificate:

``bash
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
``

6) Generate the server password:

``python
In [1]: from IPython.lib import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
``

6) Configure & Run the server:

``bash
ipython profile create nbserver
emacs -nw ~/.ipython/profile_nbserver/ipython_notebook_config.py
``

``python
c = get_config()
c.IPKernelApp.pylab = 'inline'  # if you want plotting support always
c.NotebookApp.certfile = u'/absolute/path/to/your/certificate/mycert.pem'
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.password = u'sha1:bcd259ccf...[your hashed password here]'
c.NotebookApp.port = 9999``
``
``bash
ipython notebook --profile=nbserver
``

7) Configure & Run Nginx Reverse Proxy

``bash
sudo emacs -nw /etc/nginx/conf.d/ipython.conf
``

``nginx
server {
    listen 80;
    rewrite        ^ https://$host$request_uri? permanent;
}
server {
    listen 443;
    ssl on;
    ssl_certificate /home/ubuntu/mycert.pem;
    ssl_certificate_key /home/ubuntu/mycert.pem;
    ssl_session_timeout 5m;
    ssl_protocols SSLv2 SSLv3 TLSv1;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    error_log /home/ubuntu/ipython/error.log;
    location ^~ /static/ {
        alias /home/ubuntu/ipython/venv/lib/python3.4/site-packages/IPython/html/static/;
    }
    location / {
        proxy_pass https://localhost:9999;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-NginX-Proxy true;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 86400;
    }
}
``

Now make sure to _comment out_ the built-in default config:

``bash
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
sudo emacs -nw /etc/nginx/nginx.conf
``

``nginx
...
\#       include /etc/nginx/sites-enabled/*;
...
``

Now restart the server:

``bash
sudo service nginx restart
``
 
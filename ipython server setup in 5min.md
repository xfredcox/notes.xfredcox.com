# IPython Notebook Server Configuration in 5 minutes or less

### Starting from a fresh Ubuntu 14.4 EC2 instance...

1. Open security group ports 22/ssh, 80/http and 443/https.
2. Make sure you have the credentials key file to ssh into the machine.
3. Install the boiler-plate stuff:

``bash
sudo apt-get install git emacs python3.4 python3-dev python-virtualenv liblapack-dev libatlas-dev gfortran nginx
``

4. Create a local virtual environment:

``bash
virtualenv venv -p python3.4
source venv/bin/activate
pip install numpy pandas scipy scikit-learn boto tornado pyzmq jinja2 requests ipython jsonschema
``

5. Create a self-assigned certificate:

``bash
openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
``

6. Configure the IPython Notebook server:

``python
In [1]: from IPython.lib import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:67c9e60bb8b6:9ffede0825894254b2e042ea597d771089e11aed'
``
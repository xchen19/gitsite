# Python虚拟环境配置

安装Python3.8：

sudo add-apt-repository ppa:deadsnakes/ppa  
sudo apt-get update  
sudo apt install python3.8  


开启虚拟环境：
python3.8 --version  
sudo apt install python3.8-venv  
python3.8 -m venv ~/venvt  
source ~/venvt/bin/activate  

退出虚拟环境：  
deactivate

删除虚拟环境：  
rm -rf ~/venvtestenv

安装依赖：  
pip3 install -r requirements.txt --no-cache-di

启动服务：  
gunicorn -c gunicorn_conf.py api.wsgi


vscode debug:  
launch.json  
```json
{
    "version": "0.2.0",
    "configurations": [
      {
        "name": "Python: Django",
        "type": "python",
        "request": "launch",
        "program": "/home/xchen/sdssapc/src/api/manage.py",
        "args": ["runserver"],
        "django": true,
        "justMyCode": true,
        "pythonPath": "/home/xchen/venvtest/bin/python3.8" 
      }
    ]
  }

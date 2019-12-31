# Arrplat

企业级PaaS解决方案. Web operation system and AppStore for business. Help SaaS developers and enterprise manage system data and business in one platform.

![Arrplat Overview](/_media/overview.png)

## Tech Stack

Front End

- NodeJS =12.2.0
- Lerna
- Typescript
- Vue
- Nuxt

Services

- Python >3.6
- [Flask](https://www.palletsprojects.com/p/flask/): Web Framework
- [SQLAlchemy](https://github.com/pallets/flask-sqlalchemy): ORM Framework
- [FlaskMigrate](https://github.com/miguelgrinberg/Flask-Migrate): Database migrations
- [JWT](https://jwt.io): JSON Web Token
- Redis
- Mysql >5.6

![Arrplat Overview](/_media/layer.png)

## Features

- User Center: user and follow with roles、auths、groups、departments
- Admin: User Interface
- CRM: The best crm in open source world
- NameCard: Wechat mini-program name card 

Build With

- Docker
- Gitlab CI
- Kubernetes

## Run Manually

- Clone project

```
git clone http://git.arrway.cn/springs/springs-services.git
```

- Create and use virtual environment (Optional) 

```
pip install virtualenv
virtualenv venv
source venv/bin/activate　　　
```

- Install requirements

```
pip install -r requirements.txt
```

For the network problem: 
`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt`

- Start server

```
python main.py
```

Query api from 'http://localhost:5500/'

- Init Database (If a new server)

Change settings in `config.py` and

You can then generate an initial migration:

```bash
python manage.py db migrate
```

Then you can apply the migration to the database:

```bash
python manage.py db upgrade
```

- Install Front Dependencies

```
yarn
```

- Start admin panel

```
cd packages/admin
yarn run dev
```

and goto your bower: http://localhost:3000/

## Run in Docker (TODO!! Not working NOW!!)

```bash
docker build -t arrway/flask-starter .
docker run --rm -d -p 5500:5000 --name starter arrway/flask-starter
```

## Config CI to Kubernetes

The four config file in CI:

- .gitlab-ci: gitlab ci config file
- Dockerfile: The docker file
- deployment.yaml: The kubernetes application config file


### Step1: Create new private docker repository

Go [Aliyun 容器镜像服务](https://cr.console.aliyun.com/repository/cn-beijing/) and add a new repository


### Step2: Change configs

1. You need mv ci config file and deployment.yaml file:

```
mv .gitlab-ci.example.yml .gitlab-ci.yml 
mv deployment.yaml.example.yaml deployment.yaml 
```

2. Change deployment.yaml `spec -> template -> spec -> image` to the on you create in step1
and all test of `flask-starter` to your product name(which is the name you want kubernetes application was).

And one thing you should notice is `meta -> namespace`, change to the one you want your application deployment

### Step3: Change Gitlab CI Environment variables

Go gitlab project page and follow `Settings > CI/CD Environment Variables`
You need set 4 env:

- kube_config

To get this, you can run 

```
echo $(cat ~/.kube/config | base64) | tr -d " "
```

In the compute which already login kubernetes, by default, you can use:

- REGISTRY_ADDRESS: The address you add in Step1
- REGISTRY_USERNAME: 锐途科技有限公司
- REGISTRY_PASSWORD: The password of repository

After this all setup, go to push code to the git branch(default is test)
and the runner will execute automatically.
 
## Configuration
 
You can change config in `config.py` which include:
 
- product: Basic information of the product
- db: Mysql connection details
- redis: Redis connection details
- 七牛: CDN vendor
- system: System basic configuration
- message: The text message config
 
## Folder

![](/_media/folder.png)

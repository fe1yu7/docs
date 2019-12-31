# Getting Start
## Environmental Science  
> To ensure the stability of the program, please install according to the following environment version
  
### Front End
- NodeJS =12.2.0
- Lerna
- Typescript
- Vue
- Nuxt  

### Services
- Python >3.6
- Flask: Web Framework
- SQLAlchemy: ORM Framework
- FlaskMigrate: Database migrations
- JWT: JSON Web Token
- Redis
- Mysql >5.6  

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
For the network problem: pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
- Start server
```
python main.py
```
It is more recommended to use [gunicorn](https://pypi.org/project/gunicorn/) to start, because it will make the program more robust.  
Query api from 'http://localhost:5500/'
- Init Database (If a new server)
Change settings in config.py and
You can then generate an initial migration:
```
python manage.py db migrate
```
Then you can apply the migration to the database:
```
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

**`By now, we can see the results`**

---
> If you want to build your project with docker, please follow these steps

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
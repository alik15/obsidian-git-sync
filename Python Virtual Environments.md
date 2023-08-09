#### Creating and Activating the virtual Environment 

  
```bash
python3 -m venv venv 
```

```bash
source venv/bin/activate 
```


#### Install the Flask python module(for example) under the virtual environment.

  
```bash
pip install Flask
```

  
  

#### Deactivating a Virtual Environment

  


```bash 
deactivate 
```
  
  

#### Creating the requirements file

The below command will create the requirements.txt file with the installed packages under the current environment. This file is useful for installing module at deployments.

  
```bash
pip freeze > requirements.txt  
```
*(generally a good idea to do this after the project had been made)*

  
  
  
  

####  Using the requirements file

  
```bash
pip install -r requirements.txt
```

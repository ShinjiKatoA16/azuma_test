# Django installation

## Install miniconda

- execute Miniconda-Latest-Windows_x86_64 in Q:\04_Python
- Confirm python interactive shell
  + open command terminal START->Anaconda3->Anaconda Prompt
  + `python`
  + type `exit()` to terminate python shell
- Edit python program using ATOM or notepad (ATOM is preferred)

## Virtual environment Mangement excersize
Following conda commands are used. Change env_name to proper one.

- `conda info -e`
- `conda create -n env_name python=xx.xx`
- `conda atrivate env_name`
- `conda deactivate`
- `conda remove -n env_name` --all

### Sample operation

#### Crate new virtual environment py35

- `conda info -e`
- `conda create -n py35 python=3.5`
- `conda info -e`

#### install package on py35

- `conda activate py35`
- Confirm that envrionment name in prompt is changed to **(py35)**
- `pip list`
- `pip install numpy`
- `pip list`

#### Return to base envrironment and check that numpy is not installed

- `conda deactivate`
- `pip list`

#### Remove py35

- `conda remove -n py35` --all
- `conda info -e`

## Create virtual envrionment for Django

- `conda create -n django python~=3.7`
- `conda activate django`
- `pip install Django`
- `pip list`

# Django project (application)

## Create working directory for Django project

- move to your home directory (C:\Users\NExxxxx)
- `mkdir djangogirls`
- `cd djangogirls`
- Confirm that directory is changed properly (C:\Users\NExxxxx\djangogirls)

## Create Django project

- Confirm that virtual envrinment **django** is activated. Prompt is (django)C:\Users\NExxxx\djangogirls
- `django-admin.exe startproject mysite .`  <- take care not to forget period.
- `dir`
- <- confirm that manage.py and mysite exist
- run web server
- `python manage.py runserver`
- Access the server 127.0.0.1:8000 using web browser (Chrome)
- Check the source code of HomePage (which is dynamically created)
- Terminate the web server Ctrl-C (Ctrl-Break  Ctrl-Fn-B ?)

### Change settings

- `atom mysite\settings.py`
  + TIME_ZONE = 'Asia/Tokyo'
  + LANGUAGE_CODE = 'ja'
  + STATIC_ROOT = os.path.join(BASE_DIR, 'static')
  
Try other than **ja** in LANGUAGE_CODE. 
STATIC_ROOT is to be written after STATIC_URL = '/static/'

### Migrate

- `python manage.py migrate`

### Verification

Restart web server

- `python manage.py runserver`
- Access the server 127.0.0.1:8000 using web browser (Chrome)
- Terminate the web server Ctrl-C (Ctrl-Break  Ctrl-Fn-B ?)

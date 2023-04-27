# CONFIGURAR PROYECTO DJ-REST-AUTH
- `python3 -m venv env`
- `source env/bin/activate` (On Windows use .\activate)
- En .\env\Scripts\ actualizamos el pip `python -m pip install --upgrade pip`
- Instalamos Django, Django REST Framework y dj_rest_auth
  - → `pip install django`
  - → `pip install djangorestframework`
  - → `pip install dj_rest_auth`
**( *OJO! dj_rest_auth no tiene social login ni registro, utiliza el de la libreria allauth, lo instalamos en caso de que haya dependencias a la hora de hacer los endpoints, el JWT es opcional* )**
  - → `pip install django-allauth`

## EMPEZAR PROYECTO
- `django-admin startproject nombreProyecto` 
- Modificamos el archivo settings.py. Añadimos en INSTALLED_APPS → `rest_framework`, `rest_framework.authtoken`, `dj_rest_auth`,     `django.contrib.sites`, `allauth` y `allauth.account`
- Creamos una app para los usuarios `python manage.py startapp users` y añadimos en setting.py en INSTALLED_APPS el nombre de la app

## CREAR MODELO PARA LOS USUARIOS (TEMPLATE SI ES NECESARIO)
- Crear modelo customizado de usuario default de django, en models.py [( *Ver repo allauth* )](https://github.com/natproject/test_social_login/blob/main/test_social_login/users/models.py)
```
Diferencia entre AbstractBaseUser y AbstractUser:
AbstractBaseUser → permite crear un modelo de usuario personalizado desde cero, pero tendrás que definir todos los campos que necesites para tu modelo de usuario. 
AbstractUser → es una clase base más completa que incluye muchos de los campos y métodos necesarios para la autenticación y gestión de contraseñas. Al utilizar AbstractUser, tendrás acceso a campos predefinidos como username, email, first_name, last_name, y otros campos que se utilizan comúnmente para el modelo de usuario. 
```
- Modificamos el archivo settings.py. Añadimos en INSTALLED_APPS → `users`
- Añadimos `AUTH_USER_MODEL = 'users.CustomUser'`
- Añadimos `SITE_ID = 1`
- Editamos el admin.py de users customizando el UserAdmin por CustomUserAdmin y registramos ambos `admin.site.register(CustomUser, CustomUserAdmin)` si queremos que el admin pueda editar los campos customizados [( *Ver repo allauth* )](https://github.com/natproject/test_social_login/blob/main/test_social_login/users/admin.py)
- - Hacemos migración.
  - → `python manage.py makemigrations`
  - → `python manage.py migrate` (si no funciona la migración de users probar `python manage.py makemigrations users` y luego `migrate`)
- Creamos superuser → `python manage.py createsuperuser`

## AÑADIR ENDPOINTS DE DJ-REST-AUTH (VER [REGISTRATION](https://github.com/natproject/test-dj-rest-auth/edit/main/README.md#a%C3%B1adir-endpoints-registration) Y [SOCIAL AUTH](https://github.com/natproject/test-dj-rest-auth/edit/main/README.md#a%C3%B1adir-endpoint-social-auth))
- Vamos a nuestra app `users`y creamos un archivo urls.py. Vamos a importar de `dj_rest_auth.views` las views LoginView y LogoutView y añadimos el enrutado: 
``` 
urlpatterns = [
    path('login/', LoginView.as_view()),
    path('logout/', LogoutView.as_view()),
]
```
- En urls.py del proyecto añadimos debajo de la ruta de admin `path('auth/', include('users.urls')),`
- Run server → `python manage.py runserver`
- Los [endpoints básicos ](https://dj-rest-auth.readthedocs.io/en/latest/api_endpoints.html#basic) de dj-rest-auth son login, logout. Guardan el token que viene por defecto en django (TokenAuthentication)
- Enpoint para **registrarse**:
  - 

## AÑADIR ENDPOINTS REGISTRATION
## AÑADIR ENDPOINT SOCIAL AUTH

## JSON WEB TOKEN JWT (optional)

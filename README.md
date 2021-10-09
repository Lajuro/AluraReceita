# PROJETO - Alura Receita

Neste projeto será aprendido sobre o framework de desenvolvimento web **Django**, nele, até o atual momento, foi aprendido sobre os seguintes tópicos:

- Virtual Environment (venv):
  > Para criação do ambiente virtual do Python, é ncessário executar o seguinte comando:

  ```powershell
    python -m venv venv
  ```

  Neste comando, é definido que o nome do ambiente virtual vai ser "venv".

  Para iniciar o ambiente virtual, é diferente dependendo do console utilizado:

  > Os comandos a seguir, é no cénario que esteja em uma pasta fora do ambiente virtual criado e que o nome que foi dado para o ambiente tenha sido `venv`.

  - Powershell:

    ```powershell
      .\venv\Scripts\Activate.ps1
    ```

  - Prompt de Comando do Windows (cmd):

    ```cmd
      .\venv\Scripts\activate.bat
    ```
  
  Se estiver em outro sistema operacional, não sendo Windows, a estrutura de pasta da variável de ambiente é diferente, ao invés de "Scripts" será "bin", e ao invés de ser `activate.bat` ou `Activate.ps1`, será apenas `activate`.

  ## Instalando o Django

  Para instalar o Django no ambiente virtual, é necessário que esteja dentro do ambiente, que é apenas executar os comandos anteriormente informados que caso o nome do ambiente seja `venv`, irá aparecer `(venv)` ao lado do caminho da pasta no prompt de comando.

  O comando de instalação do Django é o seguinte:

  ```powershell
  pip install django
  ```

  Pode ocorrer do pip estar desatualizado, caso esteja, o comando de atualizar o pip é o seguinte:

  ```powershell
  python -m pip install --upgrade pip
  ```

  > Após executar esses comandos, o seu ambiente virtual já estará configurado para que possa prosseguir.

  ## Dica

  Caso o projeto seja enviado ao GitHub, é interessante adicionar um arquivo `.gitignore` no projeto antes pedindo para ignorar a pasta do ambiente virtual, que neste caso é `venv`.

  **.gitignore**

  ```.gitignore
  ./venv
  ```

- Iniciando um Projeto Django:
  
  Para iniciar um projeto é necessário que rode o seguinte script do Django:
  
  ```cmd
  django-admin startproject alurareceita
  ```

  Ao executar esse comando será criado um projeto chamado `alurareceita`, porém dentro do diretório terá outra pasta com o mesmo nome, para não acontecer isso e usar o diretório atual como o diretório pai do projeto, rode o seguinte comando ao invés do anterior:

  ```cmd
  django-admin startproject alurareceita .
  ```

  Esse `.` é o que fará com que utilize o local atual.

  Após isso o projeto estará configurado com os arquivos iniciais de um projeto Django.

  ## Subindo o servidor

  Para conseguir subir o servidor do Django, é necessário digitar o seguinte comando:

  ```powershell
  python manage.py runserver
  ```

  Neste momento, o projeto já está no ar, e é possível acessar através da URL padrão `http://127.0.0.1:8000/` ou então `http://localhost:8000/`.

## Estrutura de Arquivos

A seguir, deixarei algumas anotações sobre os arquivos criados pelo Django:

- `__init__.py`
  
  Esse arquivo diz ao Python que o diretório deve ser considerado um pacote.

- `settings.py`
  
  Esse é um arquivo extremamente importante, nele contem todas as configurações relacionadas a aplicação, nele é possível mudar várias coisas, inclusive o idioma e o timezone.

  No projeto foi mudado o `TIME_ZONE` para `America/Sao_Paulo` e o `LANGUAGE_CODE` para `pt-br`.

- `urls.py`

  Esse arquivo é a declaração de todas as `urls` do projeto, funcionando como um índice para o Django.

- `wsgi.py`

  É um ponto de integração para os servidores web compatíveis com WSGI, porém é um assunto que não é tratado neste curso.

## Criação do App

Para a criação do aplicativo, é necessário alguns procedimentos iniciais, o comando de criação de um aplicativo pertencente ao Alura Receitas, é o seguinte:

```powershell
python manage.py startapp receitas
```

O comando `startapp` é o qual cria uma aplicação com alguns arquivos padrões do próprio Django. Neste caso foi criado uma aplicação chamada `receitas`.

Dentro do arquivo `app.py`, que foi criado na aplicação `receitas`, você consegue definir o nome da aplicação, no nosso caso é `receitas`.

Já no arquivo `settings.py` agora devemos adicionar `receitas` na lista `INSTALLED_APPS`.

### Configurando arquivo `urls.py` da aplicação `receitas`

Este arquivo não vem criado, então é necessário criá-lo e nele será definido as urls da aplicação.

Primeiramente é necessário realizar o `import` do pacote `urls`:

```python
from django.urls import path
```

Também necessário importar todas as urls, para isso faça o seguinte:

```python
from django.urls import path
from . import views
```

O arquivo `views.py` é o responsável por exibir na tela o que for criado por aqui. Vamos agora criar o a variável `urlpatterns` e inserir o primeiro `path`, para que possa pegar a rota principal, apenas deve passar o primeiro parâmetro como `''`, o segundo parâmetro como `views.index` que é o responsável por atender a requisição e finalmente o terceiro e último parâmetro, que é o `name='index'` sendo o namespace do aplicativo para estas entradas `urls`.

O arquivo, por enquanto, ficará assim:

```python
from django.urls import path
from . import views

urlpatterns = [
  path('', views.index, name='index')
]
```

Precisamos agora criar a view `index` que será renderizada ao acessar a url principal `/` da aplicação.

Para isso é preciso criar uma função chamado `index` que tem como primeiro parâmetro a requisição que é recebida.

Será necessário importar o pacote `http` do Django que é o `HttpResponse`, essa função será responsável por dar uma resposta na página.

O arquivo `views.py` da aplicação `receitas` ficará da seguinte forma:

```python
from django.http import HttpResponse

def index(req):
  return HttpResponse("<h1>Receita</h1>")
```

Após salvar tanto o arquivo `urls.py` e o `views.py`, ao atualizar a página, verá que nada mudou, isso porque ainda falta um último detalhe.

Assim como foi necessário atualizar o arquivo `urls.py`, que é onde está todas as `urls` de `receitas`, dentro de `alurareceita` tem outro `urls.py` que é o responsável por todas as `urls` de toda a aplicação.

Sendo assim, abra o arquivo `urls.py` e dentro de `urlpatterns`, será adicionado um outro `path` além do que já está ali e para isso será necessário utilizar o método `include()` que já é do próprio `django.urls`. Será necessário então falar para o Django que a rota principal, que no caso é representado uma string vazia `''` deverá incluir todas as urls da aplicação `receitas` a partir do path `''`, sendo assim o arquivo `urls.py` da pasta `alurareceita` ficará da seguinte forma:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('receitas.urls')),
    path('admin/', admin.site.urls),
]
```

Dessa forma, ao salvar esse arquivo e colocar para atualizar a página, verá que agora aparece **`Receitas`** na página principal.
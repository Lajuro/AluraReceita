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

### Incorporando uma página HTML

Ao invés de ser feito completamente na mão, da forma que estava, agora foi usado uma outra estrutura totalmente diferente onde renderiza um arquivo `.html`, e o Django, por padrão irá olhar em uma pasta criada chamada `templates` utilizando apenas o método `render` e não mais o `HttpResponse`, sendo assim esse import pode ser removido.

Vamos lá, então crie uma pasta chamada `templates` dentro da aplicação `receitas`, dentro de templates, crie um arquivo chamado `index.html`, esse arquivo será o que será renderizado na página final.

Após criar a página html, podemos seguir para o arquivo `views.py` e então modificar retirando o `HttpResponse` e trocando apenas pelo `render()`, sendo o primeiro parâmetro a requisição recebida pela função `index()` e o segundo parâmetro o próprio arquivo `index.html` sem precisar passar o caminho, já que conforme informado, o Django já olha no diretório `templates`. O resultado dessas alterações ficou assim:

```python
from django.shortcuts import render

def index(req):
  return render(req,'index.html')
```

### Renderizando arquivos estáticos

Para que seja possível renderizar arquivos estáticos é necessário que no arquivo `settings.py` seja feito algumas modificações:

Em `TEMPLATES`, terá a opção `DIRS`, você pode adicionar nessa parte o seguinte comando:

```python
os.path.join(BASE_DIR, 'receitas/templates')
```

Verifique que é utilizado o pacote `os`, ele precisa ser importado no início do arquivo utilizando o seguinte comando:

```python
import os
```

Estamos utilizando esse pacote para fazer a manipulação dos caminhos, juntando o diretório base com o complemento do caminho para chegar na pasta `templates` da aplicação `receitas`.

Mais abaixo, na seção que faz configurações sobre os arquivos estáticos, adicione o `STATIC_ROOT` e `STATICFILES_DIRS` da seguinte forma:

```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'alurareceita/static')
]
```

Observe que em `STATIC_ROOT` é informado onde será armazenado a raíz dos arquivos estáticos, nesse caso será no diretório base do projeto, na pasta `static`. Usamos esta configuração para indicar o caminho absoluto, onde o `collectstatic` coletará os arquivos estáticos.

Na pasta `static` dentro do projeto `alurareceita` ficará os mesmos arquivos estáticos. Em `STATICFILES_DIRS` define os locais adicionais que o aplicativo `staticfiles` percorrerá se o localizador FileSystemFinder estiver ativado, por exemplo. Se você usar o comando de gerenciamento `collectstatic` ou `findstatic` ou usar a exibição de exibição de arquivo estático.

Sobre o `STATIC_URL`, usamos para referência a arquivos estáticos localizados no `STATIC_ROOT`.

Existe um comando que pode executar no terminal para que o Django colete esses arquivos estáticos e coloquem no repositório do projeto, o comando é o seguinte:

```powershell
python manage.py collectstatic
```

Fazendo isso, o projeto já estará configurado para encontrar os arquivos estáticos.

Agora, dentro dos arquivos `index.html` e `receitas.html` você conseguirá mencionar para que seja utilizado os arquivos estáticos, primeiro é preciso carregar eles, para isso no início do arquivo digite o seguinte:

```django
{% load static %}
```

E agora todos os arquivos estáticos da página (html, css, js e imagens) podem ser encontrados da seguinte maneira:

```django
<!-- Favicon -->
<link rel="icon" href="{% static 'img/core-img/favicon.ico' %}">

<!-- Stylesheet -->
<link rel="stylesheet" href="{% static 'site.css' %}">
```

Observe que é apenas necessário envolver o conteúdo do atributo entre `{% %}` e mencionar que trata-se de um arquivo `static`.

### Trabalhando com mais páginas

Em um mundo normal da web, temos na maioria das vezes mais do que uma única página, sendo assim é interessante que possamos exibir mais do que uma página, e de acordo com a rota passada, renderize uma determinada página, desta forma, aqui vamos aprender a como fazer isso.

Primeiramente, vamos aproveitar que temos um arquivo a mais em `templates` que é o `receita.html`, esse arquivo é uma outra página da nossa aplicação, para que ela seja renderizada temos que fazer algumas coisas:

1. No arquivo `urls.py`, devemos adicionar em `urlpatterns` um novo path que será a nova página, para isso faça o seguinte:
  
    ```python
    urlpatterns = [
      [...],
      path('receita', views.receita, name='receita')
    ]
    ```

    Isso fará com que agora ele possa chamar a função `receita` dentro de `views.py` e renderize a página `receita.html` quando entrar na rota `/receita`.

2. No arquivo `views.py`, devemos criar a função `receita`, que é a que pedimos para executar quando entrarmos na rota, basicamente conforme falado no passo anterior, conforme o seguinte código:
   
    ```python
    def receita(req):
      return render(req, 'receita.html')
    ```

Agora a aplicação já poderá renderizar a página, porém ai já vemos um problema, toda aquela configuração que fizemos anteriormente para que funcionasse a página `index.html` também está acontecendo para a página `receita.html`. Além disso, foi observado muito código repetido e que da para melhorar bastante, utilizando alguns conceitos do Django.

Se observar o arquivo tem uma base, começa e encerra da mesma forma, sendo assim, no Django podemos definir um arquivo que será a base para todas as outras páginas, nesse caso vamos criar um arquivo chamado `base.html` que será a base de toda a aplicação, já importando o CSS e o JavaScript no final do arquivo.

Para definir onde queremos inserir o conteúdo das páginas nessa base, devemos criar um bloco do Django, para criar faça o seguinte:

```django

    <!-- Stylesheet -->
    <link rel="stylesheet" href="{% static 'site.css' %}">

</head>

<body>

    {% block content %} {% endblock %}

    <!-- ##### All Javascript Files ##### -->
    <!-- jQuery-2.2.4 js -->
    <script src="{% static 'js/jquery/jquery-2.2.4.min.js' %}"></script>

```

Observe que temos um `{% block content %}` e também um `{% endblock %}` e isso serve para que possamos definir onde será o conteúdo da página, o nome `content` é um nome que é dado para esse bloco para que possa ser usado para definir na página que vai utilizar essa base depois, onde começará o bloco e onde vai acabar.

Sendo assim, no arquivo `index.html`, você deverá pedir para `extender` o arquivo `base.html`, além disso definir onde começará o bloco e onde acaba, da seguinte forma:

```django
{% extends 'base.html' %}
{% load static %}
{% block content %}
  <!-- Todo o conteúdo aqui -->
{% endblock content %}
```

É importante ter o `{% load static %}` tanto na `base.html` quanto no arquivo da página que extende esse arquivo, pois nesse arquivo que estamos extendendo a base, possui referências aos arquivos estáticos.

Isso deverá ser repetido na página `receita.html` também, para que funcione igualmente.

#### Partials

Agora outro problema que da para ser observado é que em ambos os arquivos o `menu` e o `footer` são iguais, e caso quiséssemos alterar alguma coisa em algums dos dois, teria que alterar em todos os arquivos, isso pode se tornar um trabalho gigantesco em alta escala.

Para resolver esse problema, existem as `partials`, podemos, por organização, criar uma pasta chamada `partials` dentro da pasta `templates` e ali armazenar um arquivo `menu.html` com o menu e outro arquivo `footer.html` que vai armazenar o rodapé da página.

Sendo assim, devemos extrair da página `index.html` e da página `receita.html` o menu e o rodapé e então passar para os arquivos que criamos.

Após isso, se atualizarmos a página, obviamente não teremos mais nem um menu e nem um rodapé, sendo assim devemos pedir para o Django incluir um determinado arquivo que encontra-se em determinada localização em nosso HTML, para isso faça o seguinte:

```django
<!-- Conteúdo antes do menu -->
{% include 'partials/menu.html' %}
<!-- Conteúdo depois do menu e antes do footer -->
{% include 'partials/footer.html' %}
```

Isso fará com que seja renderizado cada parte na página, fazendo com que não precisemos repetir código.

Importante não esquecer de colocar o `{% load static %}` em cada partial.

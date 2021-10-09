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



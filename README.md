# Project SIRIOS

> Projeto SIRIOS — interface web simples para acionamento de GPIO (LED) usando Apache + CGI + Python.

<div align="center">
<img width="500" src="https://github.com/henriqueotogami/project-sirios/blob/main/resources/images/rounded-header-otogami.png?raw=true">
</div>
<br>
<div align="center">
<img src="https://img.shields.io/github/release-date/henriqueotogami/project-sirios">
</div>
<br>
<div align="center">
<img alt="GitHub last commit" src="https://img.shields.io/github/last-commit/henriqueotogami/project-sirios">
<img src="https://img.shields.io/github/checks-status/henriqueotogami/project-sirios/main">
<img src="https://img.shields.io/github/issues/henriqueotogami/project-sirios">
</div>
<br>
<div align="center">
<img src="https://img.shields.io/github/forks/henriqueotogami/project-sirios?style=flat">
<img src="https://img.shields.io/github/stars/henriqueotogami/project-sirios?style=flat">
<img src="https://img.shields.io/github/license/henriqueotogami/project-sirios">
</div>
<div align="center">
<br>
<a href="https://wakatime.com/badge/user/1e53636e-c916-4d50-9ce1-f3ac75a883e3/project/3b42f6ec-a049-4cac-a785-feee89bd1321"><img src="https://wakatime.com/badge/user/1e53636e-c916-4d50-9ce1-f3ac75a883e3/project/3b42f6ec-a049-4cac-a785-feee89bd1321.svg" alt="wakatime"></a>
</div>

## Artigo

### [Medium - Projeto Sirios — Apache + Python para Acionar LEDs via Web na Banana Pi](https://medium.com/@henriqueotogami/projeto-sirios-apache-python-led-324299f72391)
<div align="center">
<a href="https://medium.com/@henriqueotogami/projeto-sirios-apache-python-led-324299f72391" target="_blank"><img width="500" src="https://github.com/henriqueotogami/project-sirios/blob/main/resources/images/sirios-fluxo-arquitetura-1.png?raw=true"></a>
</div>



## Descrição
Este repositório contém artefatos e documentação do projeto SIRIOS: uma interface web minimalista que aciona pinos GPIO (ex.: ligar/desligar um LED) através de scripts Python executados como CGI pelo Apache em sistemas compatíveis (ex.: Banana Pi / Raspberry Pi).

O projeto inclui uma página HTML/CSS para a interface, exemplos de scripts Python (CGI) que escrevem em `/sys/class/gpio` e um artigo com instruções de instalação/segurança em `resources/articles/project-sirios.txt`.

## Recursos
- Página web responsiva para ligar/desligar um LED (`src/html/index.html`).
- Estilos em `src/css/style.css`.
- Scripts CGI Python para ligar (`gpio_on.py`) e desligar (`gpio_off.py`) o LED (`src/python/`).
- Documentação e notas de implantação em `resources/articles/project-sirios.txt`.


## Requisitos
- Servidor web Apache2 com suporte a CGI (módulo cgi habilitado).
- Python 3 (os scripts usam shebang /usr/bin/env python).
- Acesso root ou sudo para configurar permissões e copiar arquivos para diretórios do sistema.

## Estrutura do repositório
```
LICENSE
README.md
resources/
	articles/project-sirios.txt   # guia de instalação e segurança
	excalidraw/, figma/, images/
src/
	html/index.html              # interface web
	css/style.css                # estilos
	python/gpio_on.py            # CGI: liga o LED
	python/gpio_off.py           # CGI: desliga o LED
```

## Instalação rápida (ex.: Banana Pi / Debian-based)
1. Instale o Apache e habilite CGI:
```bash
sudo apt update
sudo apt install -y apache2
sudo a2enmod cgi
sudo systemctl restart apache2
```
2. Copie os arquivos da interface para o diretório do Apache (ajuste caminhos conforme necessário):
```bash
sudo cp src/html/index.html /var/www/html/index.html
sudo cp src/css/style.css /var/www/html/style.css
```
3. Instale os scripts CGI no diretório `/usr/lib/cgi-bin/` e torne-os executáveis:
```bash
sudo cp src/python/gpio_on.py /usr/lib/cgi-bin/gpio_on.py
sudo cp src/python/gpio_off.py /usr/lib/cgi-bin/gpio_off.py
sudo chmod +x /usr/lib/cgi-bin/gpio_on.py /usr/lib/cgi-bin/gpio_off.py
```
4. Conceda permissões sudo restritas para o usuário do Apache (`www-data`) para os comandos necessários (exemplo — editar com `sudo visudo`):
```text
www-data ALL=(ALL) NOPASSWD: /usr/bin/tee, /bin/echo, /usr/bin/gpio, /usr/bin/top, /bin/cat, /bin/df
```
Adapte a lista para incluir apenas os comandos estritamente necessários. Evite liberar mais permissões do que o necessário.

5. Reinicie o Apache:
```bash
sudo systemctl restart apache2
```

6. Acesse a interface no navegador apontando para o IP da placa: `http://<IP-da-placa>/`.

## Como funciona (resumido)
- A página `index.html` contém formulários que fazem GET para `/cgi-bin/gpio_on.py` e `/cgi-bin/gpio_off.py`.
- Os scripts Python são executados como CGI pelo Apache e usam comandos de shell (`echo | sudo tee /sys/class/gpio/...`) para exportar/configurar o pino e escrever o valor.
- A resposta do CGI é exibida em um iframe na página para feedback simples.

## Segurança e recomendações
- Evite expor essa interface diretamente à internet sem autenticação e HTTPS.
- Restrinja as entradas e minimize os comandos com sudo permitidos a `www-data` no `visudo`.
- Considere alternativas mais seguras (ex.: um daemon com serviço protegido, uso de bibliotecas que manipulam GPIO nativamente, autenticação, TLS).
- Teste em ambiente controlado antes de conectar hardware sensível.

## Licença
Este projeto está licenciado sob a MIT License — veja `LICENSE`.

## Autor
Henrique Otogami

## Referências
- `resources/articles/project-sirios.txt` — guia completo de instalação/configuração (Apache + CGI + scripts Python).
- Exemplo de uso e estrutura disponível em `src/`.

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/f6BkGbeV_mU/0.jpg)](https://www.youtube.com/watch?v=f6BkGbeV_mU)

<div align="center">
<br>
<a href="https://ko-fi.com/henriqueotogami/tip" target="_blank"><img width="500" src="https://github.com/henriqueotogami/project-sirios/blob/main/resources/images/kofi-henrique-otogami.jpg?raw=true"></a>
</div>
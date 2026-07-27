# Infra Labs — Portfólio de Infraestrutura de TI

![Status](https://img.shields.io/badge/status-em%20desenvolvimento-blue)
![Linux](https://img.shields.io/badge/Linux-Ubuntu%20Server-E95420?logo=ubuntu&logoColor=white)
![Virtualização](https://img.shields.io/badge/Virtualização-VirtualBox-183A61?logo=virtualbox&logoColor=white)
![Servidor Web](https://img.shields.io/badge/Servidor%20Web-Nginx-009639?logo=nginx&logoColor=white)
![Documentação](https://img.shields.io/badge/Documentação-Markdown-000000?logo=markdown&logoColor=white)

Bem-vindo ao meu repositório de portfólio prático em **Infraestrutura de TI**.

Este repositório reúne laboratórios, configurações, testes, procedimentos de troubleshooting e documentações desenvolvidos durante meus estudos nas áreas de **suporte técnico, redes, sistemas operacionais, servidores, segurança básica e monitoramento**.

O objetivo é apresentar, de forma organizada e baseada em evidências, minha evolução técnica e minha capacidade de aplicar conhecimentos em ambientes simulados e virtualizados.

---

## Sumário

- [Sobre o portfólio](#sobre-o-portfólio)
- [Competências desenvolvidas](#competências-desenvolvidas)
- [Tecnologias e ferramentas](#tecnologias-e-ferramentas)
- [Laboratórios disponíveis](#laboratórios-disponíveis)
- [Destaque do portfólio](#destaque-do-portfólio)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Próximos projetos](#próximos-projetos)
- [Objetivo profissional](#objetivo-profissional)
- [Como visualizar os projetos](#como-visualizar-os-projetos)
- [Autor](#autor)

---

## Sobre o portfólio

Os laboratórios deste repositório foram criados para desenvolver experiência prática com atividades comuns da área de infraestrutura, como:

- criação e configuração de máquinas virtuais;
- instalação e administração de servidores Linux;
- utilização de comandos de diagnóstico;
- configuração e verificação de redes;
- configuração de endereços IPv4 estáticos;
- instalação e administração de serviços;
- configuração de firewall;
- testes de conectividade e portas;
- identificação e resolução de problemas;
- produção de documentação técnica;
- registro de evidências em imagens;
- organização de projetos com Git e GitHub.

Cada atividade possui documentação própria, contendo:

- objetivo;
- ambiente utilizado;
- arquitetura;
- configurações;
- procedimentos executados;
- comandos utilizados;
- testes de validação;
- dificuldades encontradas;
- correções aplicadas;
- resultados;
- evidências visuais.

---

## Competências desenvolvidas

| Área | Competências práticas |
|---|---|
| Virtualização | Criação e configuração de máquinas virtuais no VirtualBox |
| Linux | Instalação e administração básica do Ubuntu Server |
| Redes | IPv4, máscara, gateway, DNS, NAT e testes de conectividade |
| Servidores | Instalação e gerenciamento do Nginx |
| Segurança | Configuração do firewall UFW e controle de portas |
| Troubleshooting | Diagnóstico com `ping`, `ip`, `ss`, `curl`, `systemctl` e PowerShell |
| Documentação | Produção de README técnico em Markdown |
| Versionamento | Organização e publicação dos projetos no GitHub |

---

## Tecnologias e ferramentas

| Categoria | Tecnologias e ferramentas |
|---|---|
| Virtualização | Oracle VirtualBox |
| Sistemas operacionais | Ubuntu Server e Windows |
| Administração Linux | Terminal, Bash, `systemd` e ferramentas nativas |
| Configuração de rede | Netplan |
| Redes | TCP/IP, IPv4, DNS, gateway, NAT e redirecionamento de portas |
| Servidores | Nginx e serviços HTTP |
| Segurança | UFW |
| Diagnóstico | `ping`, `ip`, `ss`, `curl`, `systemctl` e `Test-NetConnection` |
| Versionamento | Git e GitHub |
| Documentação | Markdown |
| Desenvolvimento web | HTML |

---

# Laboratórios disponíveis

## Laboratório 01 — Instalação e validação de um servidor Ubuntu

Criação de uma máquina virtual no Oracle VirtualBox, instalação do Ubuntu Server, realização do primeiro login e execução de comandos básicos para validar o ambiente.

### Objetivos

- criar uma máquina virtual;
- instalar o Ubuntu Server;
- realizar o primeiro acesso;
- identificar o hostname e o usuário;
- verificar interfaces de rede;
- verificar armazenamento e memória.

### Principais comandos utilizados

```bash
hostname
whoami
ip a
df -h
free -h
```

### Conhecimentos demonstrados

- criação de máquinas virtuais;
- instalação de sistema operacional para servidor;
- acesso ao terminal Linux;
- identificação do hostname;
- identificação do usuário autenticado;
- consulta das interfaces de rede;
- verificação do espaço em disco;
- análise do uso de memória RAM.

### Evidências esperadas

- tela de login do Ubuntu Server;
- resultado do comando `hostname`;
- resultado do comando `whoami`;
- interfaces exibidas por `ip a`;
- utilização de disco com `df -h`;
- utilização de memória com `free -h`.

> Consulte a documentação e as imagens na pasta correspondente ao laboratório.

---

## Laboratório 02 — Servidor Web Ubuntu com Nginx e segurança básica

Implantação de um servidor web em Ubuntu Server utilizando **Nginx**, endereço IPv4 estático, firewall **UFW** e redirecionamento de porta no Oracle VirtualBox.

O servidor foi disponibilizado no computador Windows através do endereço:

```text
http://127.0.0.1:8080
```

### Objetivos

- configurar um endereço IPv4 estático;
- instalar e iniciar o Nginx;
- criar uma página HTML personalizada;
- configurar o firewall UFW;
- publicar a porta 80 da máquina virtual;
- validar a conexão pelo Windows;
- documentar o laboratório com evidências.

### Arquitetura resumida

```text
Navegador no Windows
http://127.0.0.1:8080
          |
          | TCP 8080
          v
NAT e redirecionamento do VirtualBox
127.0.0.1:8080 → 10.0.2.50:80
          |
          | TCP 80
          v
Ubuntu Server
IP: 10.0.2.50/24
Interface: enp0s3
Firewall: UFW
Servidor: Nginx
```

### Configuração de rede

| Item | Valor |
|---|---|
| Interface | `enp0s3` |
| Endereço IPv4 | `10.0.2.50/24` |
| Máscara | `255.255.255.0` |
| Gateway | `10.0.2.2` |
| DNS do VirtualBox | `10.0.2.3` |
| DNS adicionais | `1.1.1.1` e `8.8.8.8` |
| Porta do Nginx | `80/TCP` |
| Porta publicada no Windows | `8080/TCP` |

### Configuração do Netplan

Arquivo utilizado:

```text
/etc/netplan/01-servidor.yaml
```

Configuração:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 10.0.2.50/24
      routes:
        - to: default
          via: 10.0.2.2
      nameservers:
        addresses:
          - 10.0.2.3
          - 1.1.1.1
          - 8.8.8.8
```

Validação e aplicação:

```bash
sudo chmod 600 /etc/netplan/01-servidor.yaml
sudo netplan generate
sudo netplan try
sudo netplan apply
```

### Instalação do Nginx

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable --now nginx
```

Validação do serviço:

```bash
sudo systemctl status nginx --no-pager
curl -I http://127.0.0.1
sudo ss -ltnp | grep ':80'
```

Resultados esperados:

```text
Active: active (running)
HTTP/1.1 200 OK
0.0.0.0:80 LISTEN
```

### Configuração do firewall

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status numbered
```

Política implementada:

```text
Entrada: bloqueada por padrão
Saída: permitida
HTTP: permitido na porta 80/TCP
```

### Redirecionamento de porta no VirtualBox

| Campo | Valor |
|---|---|
| Nome | `HTTP-NGINX` |
| Protocolo | `TCP` |
| IP do hospedeiro | `127.0.0.1` |
| Porta do hospedeiro | `8080` |
| IP do convidado | `10.0.2.50` |
| Porta do convidado | `80` |

### Testes no Windows

```powershell
Test-NetConnection 127.0.0.1 -Port 8080
curl.exe -I http://127.0.0.1:8080
```

Resultados obtidos:

```text
TcpTestSucceeded : True
HTTP/1.1 200 OK
```

### Conhecimentos demonstrados

- configuração de IPv4 estático;
- uso do Netplan;
- instalação de serviços Linux;
- administração do Nginx;
- criação de página HTML;
- configuração de firewall;
- NAT e redirecionamento de portas;
- testes de conectividade;
- troubleshooting em rede;
- documentação baseada em evidências.

### Evidências do laboratório

As imagens devem demonstrar:

1. interface `enp0s3` e endereço IPv4;
2. rota padrão do Ubuntu;
3. conteúdo do arquivo Netplan;
4. endereço estático aplicado;
5. serviço Nginx ativo;
6. porta 80 em estado `LISTEN`;
7. página HTML;
8. configuração do UFW;
9. redirecionamento de porta no VirtualBox;
10. teste TCP com resultado `True`;
11. resposta HTTP `200 OK`;
12. página aberta no navegador.

### Documentação completa

Consulte:

```text
Projeto-Servidor-web/README.md
```

---

# Destaque do portfólio

## Servidor Web Ubuntu

| Item | Resultado |
|---|---|
| Sistema operacional | Ubuntu Server |
| Serviço web | Nginx |
| Endereço estático | `10.0.2.50/24` |
| Segurança | Firewall UFW |
| Porta interna | `80/TCP` |
| Porta no hospedeiro | `8080/TCP` |
| Teste TCP | Concluído com sucesso |
| Resposta HTTP | `200 OK` |
| Acesso pelo navegador | Funcionando |
| Documentação | Passo a passo com evidências |

Este laboratório se destaca por integrar **Linux, redes, segurança, virtualização e troubleshooting** em uma única entrega prática.

---

## Estrutura do repositório

```text
infra-labs_-PORTFOLIO--main/
│
├── README_Portfolio_Infra_Labs.md
│
├── Projeto-Servidor-web/
│   ├── documentação/
│   │   └── README.md
│   └── print/
│       ├── Screenshot de login.png
│       └── Screenshot dos comandos.png
│
└── servidor-web-ubuntu/
    ├── README_Servidor_Web_Ubuntu.md
    ├── configs/
    │   └── 01-servidor.yaml.txt
    ├── site/
    │   └── index.html.txt
    └── docs/
        └── images/
            ├── 01-ip-address.png
            ├── 02-ip-route.png
            ├── 03-netplan-config.png
            ├── 04- netplan apply.png
            ├── 05-nginx status.png
            ├── 06-porta-80-listen.png
            ├── 07-index_html.png
            ├── 08-ufw status.png
            ├── 09-port forwarding.png
            ├── 10-tcp test.png
            ├── 11-http 200.png
            └── 12-pagina-web.png
```

A estrutura poderá ser ampliada conforme novos laboratórios forem desenvolvidos.

---

## Próximos projetos

Os próximos laboratórios planejados incluem:

- configuração de acesso remoto com SSH;
- criação e gerenciamento de usuários e grupos;
- configuração de permissões em arquivos e diretórios;
- implementação de serviços DNS e DHCP;
- compartilhamento de arquivos em rede;
- instalação e configuração do Fail2ban;
- configuração de HTTPS;
- monitoramento de servidores;
- automação de tarefas com scripts;
- laboratórios com Windows Server;
- criação de ambientes com Docker;
- documentação de incidentes e procedimentos de suporte.

---

## Objetivo profissional

Este repositório faz parte da minha preparação para oportunidades de **estágio e vagas de nível inicial em Suporte Técnico, Monitoramento e Infraestrutura de TI**.

Por meio destes projetos, busco demonstrar:

- interesse contínuo em aprender;
- organização e atenção aos detalhes;
- capacidade de documentar processos técnicos;
- conhecimento prático de servidores e redes;
- capacidade de identificar e resolver problemas;
- iniciativa para criar ambientes de estudo;
- evolução constante na área de Tecnologia da Informação.

---

## Como visualizar os projetos

1. Acesse a pasta do laboratório desejado.
2. Abra o arquivo `README.md`.
3. Consulte o objetivo e a arquitetura.
4. Acompanhe o passo a passo.
5. Analise os comandos utilizados.
6. Consulte as imagens na pasta de evidências.
7. Verifique os testes e resultados.
8. Leia a seção de troubleshooting.

---

## Boas práticas adotadas

- documentação em Markdown;
- comandos organizados por etapa;
- evidências com nomes padronizados;
- separação entre configurações, site e imagens;
- descrição dos resultados esperados;
- registro das dificuldades encontradas;
- proteção de informações sensíveis;
- estrutura preparada para novos projetos.

---

## Autor

**Matheus D’Eleutério**  
Estudante de Engenharia de Software, com foco profissional em Suporte Técnico, Monitoramento e Infraestrutura de TI.

- [GitHub](https://github.com/Peacefullsppy)
- [LinkedIn](https://www.linkedin.com/in/matheus-d%E2%80%99eleut%C3%A9rio-de-castro-lopes-2367a034a/)
- E-mail profissional: `matheus.castro2164@gmail.com`

---

## Observação

Este portfólio está em desenvolvimento contínuo. Novos projetos, tecnologias, evidências e documentações serão adicionados conforme o avanço dos estudos e a realização de novos laboratórios.

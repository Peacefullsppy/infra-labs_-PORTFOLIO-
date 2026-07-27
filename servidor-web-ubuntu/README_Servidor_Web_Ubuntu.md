# Servidor Web Ubuntu com Nginx e Segurança Básica

![Status](https://img.shields.io/badge/status-concluído-success)
![Ubuntu](https://img.shields.io/badge/Ubuntu-Server-E95420?logo=ubuntu&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-Web%20Server-009639?logo=nginx&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-NAT-183A61?logo=virtualbox&logoColor=white)

## Sobre o projeto

Este projeto apresenta a criação de um servidor web em **Ubuntu Server**, executado em uma máquina virtual do **Oracle VirtualBox**. O servidor utiliza o **Nginx** para publicar uma página HTTP e o **UFW** para aplicar segurança básica.

Como a máquina virtual está configurada no modo de rede **NAT**, foi criado um redirecionamento de porta no VirtualBox para permitir o acesso ao servidor pelo navegador do Windows.

> Este laboratório Ubuntu é separado da topologia criada no Cisco Packet Tracer. O ASA e as redes do Packet Tracer não se comunicam diretamente com a máquina virtual do VirtualBox.

---

## Objetivos

- Instalar e configurar o Ubuntu Server.
- Configurar um endereço IPv4 estático.
- Instalar e iniciar o Nginx.
- Publicar uma página HTML personalizada.
- Liberar somente o tráfego HTTP necessário no firewall.
- Redirecionar uma porta do Windows para a porta 80 da máquina virtual.
- Validar o serviço com comandos de diagnóstico.
- Documentar o laboratório para portfólio técnico.

---

## Tecnologias utilizadas

| Tecnologia | Finalidade |
|---|---|
| Ubuntu Server | Sistema operacional do servidor |
| Oracle VirtualBox | Virtualização |
| Netplan | Configuração de rede do Ubuntu |
| Nginx | Servidor web |
| UFW | Firewall local |
| PowerShell | Testes de conectividade no Windows |
| HTML | Página publicada |
| Git e GitHub | Versionamento e documentação |

---

## Arquitetura do laboratório

```text
Navegador no Windows
http://127.0.0.1:8080
          |
          | TCP 8080
          v
Redirecionamento de porta do VirtualBox
127.0.0.1:8080 -> 10.0.2.50:80
          |
          | TCP 80
          v
Ubuntu Server
Interface: enp0s3
IP: 10.0.2.50/24
Gateway: 10.0.2.2
          |
          v
Nginx
/var/www/html/index.html
```

---

## Plano de endereçamento

| Componente | Endereço |
|---|---|
| Interface da VM | `enp0s3` |
| IP inicial por DHCP | `10.0.2.15/24` |
| IP estático configurado | `10.0.2.50/24` |
| Gateway do NAT do VirtualBox | `10.0.2.2` |
| DNS do VirtualBox | `10.0.2.3` |
| Porta HTTP no Ubuntu | `80/TCP` |
| Porta publicada no Windows | `8080/TCP` |
| URL de acesso | `http://127.0.0.1:8080` |

---

# Implementação

## 1. Configuração da rede no VirtualBox

Com a máquina virtual desligada, foi configurado:

```text
VirtualBox
└── Configurações
    └── Rede
        └── Adaptador 1
            ├── Habilitar placa de rede: marcado
            ├── Conectado a: NAT
            └── Cabo conectado: marcado
```

O modo NAT permite que o Ubuntu acesse a Internet utilizando a conexão do computador hospedeiro.

---

## 2. Identificação da interface de rede

No Ubuntu Server:

```bash
ip address
```

A interface identificada foi:

```text
enp0s3
```

O endereço inicialmente recebido por DHCP foi:

```text
10.0.2.15/24
```

Uma forma mais resumida de visualizar as interfaces é:

```bash
ip -4 -br address
```

Para visualizar a rota padrão:

```bash
ip route
```

Resultado esperado no modo NAT:

```text
default via 10.0.2.2 dev enp0s3
```

---

## 3. Configuração do Netplan

Como o diretório não existia inicialmente, ele foi criado:

```bash
sudo mkdir -p /etc/netplan
```

O arquivo de configuração foi aberto com o Nano:

```bash
sudo nano /etc/netplan/01-servidor.yaml
```

Conteúdo utilizado:

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

### Salvamento no Nano

```text
Ctrl + O
Enter
Ctrl + X
```

A permissão do arquivo foi restringida:

```bash
sudo chmod 600 /etc/netplan/01-servidor.yaml
```

A sintaxe foi validada:

```bash
sudo netplan generate
```

A configuração foi testada com reversão automática:

```bash
sudo netplan try
```

Depois foi aplicada:

```bash
sudo netplan apply
```

### Verificação

```bash
ip -4 -br address
ip route
```

Resultado esperado:

```text
enp0s3    UP    10.0.2.50/24
```

```text
default via 10.0.2.2 dev enp0s3
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.50
```

Testes de rede:

```bash
ping -c 4 10.0.2.2
ping -c 4 8.8.8.8
ping -c 4 google.com
```

---

## 4. Atualização do sistema

```bash
sudo apt update
sudo apt upgrade -y
```

---

## 5. Instalação do Nginx

```bash
sudo apt install nginx -y
```

O serviço foi iniciado e configurado para iniciar automaticamente:

```bash
sudo systemctl enable --now nginx
```

Verificação:

```bash
sudo systemctl status nginx --no-pager
```

Resultado esperado:

```text
Active: active (running)
```

Teste local:

```bash
curl -I http://127.0.0.1
```

Resultado esperado:

```text
HTTP/1.1 200 OK
Server: nginx
```

Verificação da porta:

```bash
sudo ss -ltnp | grep ':80'
```

Resultado esperado:

```text
LISTEN 0 511 0.0.0.0:80
```

---

## 6. Criação da página HTML

O arquivo padrão do Nginx foi editado:

```bash
sudo nano /var/www/html/index.html
```

Conteúdo utilizado:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Servidor Web Linux</title>
</head>
<body>
    <h1>Servidor Web Linux funcionando!</h1>

    <p>
        Servidor Ubuntu configurado com Nginx,
        firewall UFW e acesso HTTP pelo VirtualBox.
    </p>

    <h2>Tecnologias</h2>

    <ul>
        <li>Ubuntu Server</li>
        <li>Nginx</li>
        <li>UFW</li>
        <li>VirtualBox</li>
    </ul>
</body>
</html>
```

A configuração do Nginx foi validada:

```bash
sudo nginx -t
```

O serviço foi recarregado:

```bash
sudo systemctl reload nginx
```

Teste:

```bash
curl http://127.0.0.1
```

---

## 7. Configuração do firewall UFW

O estado do firewall foi verificado:

```bash
sudo ufw status
```

Foram configuradas políticas padrão:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

A porta HTTP foi liberada:

```bash
sudo ufw allow 80/tcp
```

O firewall foi ativado:

```bash
sudo ufw enable
```

Verificação:

```bash
sudo ufw status numbered
```

Resultado esperado:

```text
Status: active

[1] 80/tcp ALLOW IN Anywhere
```

> Em um ambiente de produção, a origem do acesso deve ser restringida sempre que possível.

---

## 8. Redirecionamento de porta no VirtualBox

A máquina virtual foi desligada:

```bash
sudo poweroff
```

No VirtualBox:

```text
Configurações
└── Rede
    └── Adaptador 1
        └── Avançado
            └── Redirecionamento de Portas
```

Regra criada:

| Campo | Valor |
|---|---|
| Nome | `HTTP-NGINX` |
| Protocolo | `TCP` |
| IP do hospedeiro | `127.0.0.1` |
| Porta do hospedeiro | `8080` |
| IP do convidado | `10.0.2.50` |
| Porta do convidado | `80` |

Fluxo da conexão:

```text
Windows 127.0.0.1:8080
          ↓
VirtualBox NAT
          ↓
Ubuntu 10.0.2.50:80
```

---

## 9. Testes no Windows

No PowerShell:

```powershell
Test-NetConnection 127.0.0.1 -Port 8080
```

Resultado esperado:

```text
TcpTestSucceeded : True
```

Teste HTTP pelo terminal:

```powershell
curl.exe -I http://127.0.0.1:8080
```

Resultado esperado:

```text
HTTP/1.1 200 OK
Server: nginx
```

A página foi aberta no navegador pelo endereço:

```text
http://127.0.0.1:8080
```

Também é possível utilizar:

```text
http://localhost:8080
```

---

# Validação final

## Testes no Ubuntu

```bash
sudo systemctl is-active nginx
curl -I http://127.0.0.1
sudo ss -ltnp | grep ':80'
sudo ufw status numbered
ip -4 -br address
ip route
```

Resultados esperados:

```text
Nginx: active
HTTP: 200 OK
Porta 80: LISTEN
UFW: active
Interface: enp0s3
IP: 10.0.2.50/24
Gateway: 10.0.2.2
```

## Testes no Windows

```powershell
Test-NetConnection 127.0.0.1 -Port 8080
curl.exe -I http://127.0.0.1:8080
```

Resultado esperado:

```text
TcpTestSucceeded : True
HTTP/1.1 200 OK
```

---

# Troubleshooting

## `TcpTestSucceeded` retornando `False`

Foram verificadas as seguintes possibilidades:

1. Nginx inativo.
2. Porta 80 não estava em estado `LISTEN`.
3. Regra de redirecionamento ausente ou incorreta.
4. IP do convidado diferente do IP configurado na regra.
5. Porta `8080` ocupada no Windows.
6. UFW bloqueando a porta 80.
7. Adaptador do VirtualBox fora do modo NAT.

### Diagnóstico no Ubuntu

```bash
sudo systemctl is-active nginx
curl -I http://127.0.0.1
sudo ss -ltnp | grep ':80'
sudo ufw status
ip -4 -br address
```

### Diagnóstico no Windows

```powershell
Test-NetConnection 127.0.0.1 -Port 8080
netstat -ano | findstr :8080
```

### Correção aplicada

A regra de redirecionamento foi configurada com o endereço correto da VM:

```text
127.0.0.1:8080 -> 10.0.2.50:80
```

Após a correção:

```text
TcpTestSucceeded : True
```

---

# Segurança implementada

- Firewall UFW ativo.
- Política padrão de bloqueio para conexões de entrada.
- Liberação explícita somente da porta HTTP.
- Serviço Nginx executado como serviço do sistema.
- Arquivo do Netplan protegido com permissão `600`.
- Acesso publicado apenas em `127.0.0.1:8080`, não em todas as interfaces do Windows.

---

# Melhorias futuras

- Configurar HTTPS com certificado TLS.
- Configurar acesso SSH por chave.
- Desabilitar login SSH do usuário root.
- Instalar e configurar Fail2ban.
- Criar logs centralizados.
- Monitorar o servidor com Zabbix ou Prometheus.
- Executar o Nginx em contêiner Docker.
- Automatizar a implantação com Ansible.
- Criar rotina de backup da página e das configurações.

---

# Evidências sugeridas

Adicione as imagens na pasta `docs/images/`:

```text
docs/
└── images/
    ├── 01-ip-address.png
    ├── 02-netplan.png
    ├── 03-nginx-active.png
    ├── 04-porta-80.png
    ├── 05-ufw-status.png
    ├── 06-port-forwarding.png
    ├── 07-tcp-test.png
    └── 08-pagina-web.png
```

Exemplo para inserir uma evidência no README:

```markdown
## Evidência: página publicada

![Página do servidor Ubuntu](docs/images/08-pagina-web.png)
```

Não publique:

- senhas;
- chaves privadas;
- tokens;
- informações pessoais;
- endereços públicos sensíveis.

---

# Estrutura recomendada do repositório

```text
servidor-web-ubuntu/
├── README.md
├── configs/
│   └── 01-servidor.yaml
├── site/
│   └── index.html
└── docs/
    └── images/
```

---

# Comandos principais

```bash
# Rede
ip -4 -br address
ip route

# Netplan
sudo netplan generate
sudo netplan try
sudo netplan apply

# Nginx
sudo systemctl enable --now nginx
sudo systemctl status nginx
sudo nginx -t
sudo systemctl reload nginx

# Teste HTTP
curl -I http://127.0.0.1

# Porta
sudo ss -ltnp | grep ':80'

# Firewall
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status numbered
```

```powershell
# Teste pelo Windows
Test-NetConnection 127.0.0.1 -Port 8080
curl.exe -I http://127.0.0.1:8080
```

---

# Resultado

O projeto foi concluído com sucesso. O servidor Ubuntu executa o Nginx, publica uma página HTML pela porta 80 e pode ser acessado no Windows através do redirecionamento de porta do VirtualBox:

```text
http://127.0.0.1:8080
```

---

## Autor

**Matheus**

Estudante de Engenharia de Software com interesse em infraestrutura, redes, Linux, DevOps, automação e SRE.

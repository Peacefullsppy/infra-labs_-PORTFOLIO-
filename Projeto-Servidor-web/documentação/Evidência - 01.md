# Infra Labs — Portfólio de Infraestrutura de TI

## Repositório destinado à documentação de laboratórios práticos, configurações de servidores e atividades relacionadas à infraestrutura de TI.

# Laboratório 01 — Instalação e Validação de um Servidor Ubuntu

## Status: Concluído
##Ambiente de virtualização: Oracle VirtualBox

# 1. Objetivo

## Criar e configurar uma máquina virtual com o Ubuntu Server, realizar o primeiro acesso ao sistema e executar comandos básicos para validar a instalação, a identidade do servidor, a configuração de rede e o uso dos recursos computacionais.

# 2. Configuração da Máquina Virtual  

Recurso	Configuração
Nome da máquina virtual	srv-web-01
Sistema operacional informado	Ubuntu Server 25.04 — Plucky Puffin, 64 bits
Memória RAM	4 GB — 4096 MB
Processadores	2 CPUs
Armazenamento	25 GB
Software de virtualização	Oracle VirtualBox

# 3. Atividades Executadas
## Durante o laboratório, foram realizadas as seguintes atividades:

Criação da máquina virtual no Oracle VirtualBox.
Definição dos recursos de hardware virtual.
Instalação do Ubuntu Server.
Inicialização do sistema operacional.
Realização do primeiro login.
Verificação do nome do servidor e do usuário autenticado.
Consulta das interfaces e dos endereços de rede.
Verificação do espaço disponível em disco.
Consulta do consumo de memória RAM.
# 4. Comandos Utilizados

## Comando	Finalidade

hostname	Exibir o nome configurado para o servidor.
whoami	Identificar o usuário atualmente autenticado.
ip a	Exibir as interfaces e os endereços de rede do sistema.
df -h	Consultar o espaço utilizado e disponível nos sistemas de arquivos.
free -h	Verificar o consumo e a disponibilidade de memória RAM.

# 5. Validações Realizadas
## Os testes apresentados nas evidências confirmaram os seguintes resultados:

O servidor foi identificado corretamente como srv-web-01.
O login foi realizado com o usuário matheus.
A interface de rede enp0s3 estava ativa.
O servidor recebeu o endereço IPv4 10.0.2.15/24.
A partição principal possuía aproximadamente 25 GB de capacidade.
No momento da verificação, aproximadamente 3,4 GB do disco estavam em uso.
O sistema reconheceu aproximadamente 3,3 GiB de memória RAM.
Não havia espaço de swap configurado no momento do teste.

# 6. Resultado

## A máquina virtual foi criada e o sistema operacional foi instalado com sucesso. O primeiro acesso ao servidor foi realizado normalmente, e os comandos utilizados confirmaram o funcionamento básico do ambiente.
## As verificações de hostname, usuário, rede, armazenamento e memória demonstraram que o servidor estava operacional e pronto para receber configurações adicionais em laboratórios futuros.

# 7. Dificuldades Encontradas

## Não foram identificadas falhas relevantes durante a criação da máquina virtual, a instalação do sistema operacional ou a execução dos comandos de validação.

# 8. Evidências (ACESSE A PASTA 'print' deste repositório!!!)
## 8.1 Primeiro login no servidor

# Figura 1 (Screenshot de Login) — Inicialização do Ubuntu Server e primeiro login realizado com sucesso.

## 8.2 Verificação do ambiente

# Figura 2 (Screenshot de comandos) — 
## Execução dos comandos 
hostname 
whoami 
ip a
df -h
free -h

# 9. Informação a Ser Validada

# Atenção: a configuração original informa a utilização do Ubuntu Server 25.04 — Plucky Puffin no Virtualbox, porém a tela de login apresentada nas evidências identifica o sistema como Ubuntu 26.04 LTS. A versão efetivamente instalada deve ser confirmada e atualizada nesta documentação para manter a consistência das informações.

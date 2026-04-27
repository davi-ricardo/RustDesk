# RustDesk Server - Docker Setup

Este guia fornece instruções passo a passo para configurar e rodar o seu próprio servidor RustDesk (ID e Relay) utilizando Docker Compose.

## 📂 Estrutura de Arquivos

A pasta do projeto contém os seguintes arquivos essenciais:

- `compose.yml`: Define os serviços `hbbs` (ID Server) e `hbbr` (Relay Server).
- **data/**: Pasta onde são armazenados os dados persistentes, incluindo o banco de dados e as chaves de criptografia.
  - `id_ed25519.pub`: Sua chave pública (necessária para configurar o cliente RustDesk).
  - `id_ed25519`: Sua chave privada (mantenha em segurança).

---

## 🚀 Passo a Passo para Rodar

### 1. Pré-requisitos
Certifique-se de ter o **Docker** e o **Docker Compose** instalados em sua máquina.

### 2. Subir o Servidor
Abra o terminal na pasta onde está o arquivo `compose.yml` e execute:

```bash
docker-compose up -d
```

Este comando irá:
- Baixar a imagem `rustdesk/rustdesk-server`.
- Iniciar o container **hbbs** (ID/Rendezvous server).
- Iniciar o container **hbbr** (Relay server).

### 3. Verificar o Status
Para garantir que tudo está rodando corretamente:

```bash
docker-compose ps
```

---

## 🔑 Configuração do Cliente RustDesk

Para que seus dispositivos se conectem ao seu próprio servidor, você precisará configurar o endereço IP e a **Key**.

### 1. Obter a Chave Pública (Key)
A chave é gerada automaticamente na primeira execução dentro da pasta `data`. Você pode visualizá-la abrindo o arquivo localizado em:
`data/id_ed25519.pub`

Copie o conteúdo deste arquivo.

### 2. Configurar no Aplicativo RustDesk
No cliente RustDesk (seu computador ou celular):
1. Vá em **Configurações** -> **Rede**.
2. Clique em **Desbloquear configurações de rede**.
3. No campo **ID Server**, digite o IP do servidor onde o Docker está rodando.
4. No campo **Relay Server**, digite o mesmo IP (opcional, ele tentará o mesmo do ID Server por padrão).
5. No campo **Key**, cole o conteúdo que você copiou do arquivo `id_ed25519.pub`.
6. Clique em **Aplicar**.

---

## 🛠️ Portas Utilizadas (Importante)

Como o arquivo `compose.yml` utiliza `network_mode: "host"`, o Docker usará as portas diretamente do host. Certifique-se de que as seguintes portas estejam abertas no firewall:

- **TCP**: 21115, 21116, 21117, 21118, 21119
- **UDP**: 21116

> **Nota para usuários Windows:** O modo `host` no Docker Desktop para Windows pode se comportar de forma diferente do Linux. Se encontrar problemas de conexão, verifique as configurações de rede do Docker Desktop.

---

## 🛑 Como Parar o Servidor

Para parar e remover os containers:

```bash
docker-compose down
```

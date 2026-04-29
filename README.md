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
## 🚀 Como Instalar e Configurar na sua VPS

### 1. Pré-requisitos
Certifique-se de que seu servidor (VPS) possua instalados:
- **Git**
- **Docker**
- **Docker Compose**

### 2. Preparação do Servidor
Após acessar sua VPS via terminal (ou através do terminal de sua preferência):

```bash
ssh root@ip_da_vps
```

Execute os passos abaixo para preparar o ambiente e subir os serviços:

```bash
# 1. Criar o diretório do projeto
mkdir -p /opt/rustdesk
cd /opt/rustdesk

# 2. Clonar o repositório (substitua pela URL do seu repositório público/privado)
git clone https://github.com/SEU_USUARIO/SEU_REPOSITORIO.git .

# 3. Iniciar os containers em segundo plano
docker-compose up -d
```

Neste momento, o Docker irá baixar as imagens do RustDesk Server e iniciar os serviços. O diretório `data/` será criado automaticamente e as chaves de criptografia serão geradas.

### 3. Obtendo sua Chave de Segurança (Key)
Para que os seus dispositivos se conectem com segurança ao seu servidor, você precisará da Public Key gerada automaticamente pelo container. 

Execute o comando abaixo para visualizar a chave:

```bash
cat /opt/rustdesk/data/id_ed25519.pub
```

> **Nota:** Copie o código alfanumérico que aparecerá no terminal. Você o usará no próximo passo.

### 4. Configuração do Cliente (Desktop/Mobile)
Com o servidor rodando, siga estes passos no aplicativo RustDesk do seu computador ou celular para se conectar à sua rede privada:

1. Abra o RustDesk e clique no **ícone de menu (três pontos)** ao lado do seu ID.
2. Vá em **Configurações** > **Rede**.
3. Clique em **Desbloquear configurações de rede** (se necessário).
4. Preencha os campos conforme abaixo:
   - **Servidor de ID:** `O_IP_DA_SUA_VPS` (ou seu domínio/subdomínio, ex: `rustdesk.seudominio.com`)
   - **Servidor de Relay:** `O_IP_DA_SUA_VPS` (ou o mesmo domínio acima)
   - **Key:** Cole a chave que você obteve no passo anterior.
5. Clique em **OK** (ou Aplicar).

> ✅ **Sucesso:** Se a bolinha no rodapé do aplicativo ficar verde e exibir a mensagem "Pronto", sua infraestrutura privada está operacional.
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

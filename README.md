# Meu primeiro lab com EC2 na AWS 

---

## REsumo

### EC2
É basicamente um **computador na nuvem** que você "aluga"... Vc escolhe o sistema operacional, o tamanho da máquina e paga pelo tempo que ela fica ligada.

- **Instância**: é a máquina
- **AMI**: é o "molde" da máquina, tipo uma imagem com o sistema já instalado. Você escolhe uma AMI e cria a instância a partir dela. Um copiar e colar.

### EBS
É o **HD da instância**. Os arquivos da máquina ficam guardados nele.

### Snapshot
É uma **foto do HD (EBS)** num momento específico. Serve como backup: se der ruim, dá pra criar um disco novo a partir do snapshot.
O que eu achei legal: os snapshots são **incrementais**, ou seja, depois do primeiro ele só salva o que mudou.

### S3
É um lugar pra **guardar arquivos** na nuvem. Os arquivos ficam dentro de **buckets** (tipo pastas grandes).

---

## Arquitetura que a gente viu

![Diagrama da arquitetura](images/arquitetura-lab.jpg)

Como eu entendi o desenho:

1. O **usuário** usa um app (no computador ou no celular).
2. O app manda arquivos pra um **bucket S3**.
3. A **EC2** pega esses arquivos e processa.
4. Ela salva informações num **banco de dados (RDS)**.
5. E manda o resultado pra **outro bucket S3**.

---

## Instalando a AWS CLI (com muita luta)

Confesso que essa foi a parte que mais me deu trabalho. A CLI é a ferramenta pra controlar a AWS pelo terminal, digitando comandos.

O que eu fiz:

1. Baixei o instalador da AWS CLI v2 no site oficial da AWS e instalei.
2. Abri o terminal e testei:
   ```bash
   aws --version
   ```
3. Configurei com as minhas credenciais:
   ```bash
   aws configure
   ```
   Ele pede 4 coisas: **Access Key ID**, **Secret Access Key**, **região** (ex.: `us-east-1`) e **formato de saída** (eu coloquei `json`).
4. Testei se tava funcionando:
   ```bash
   aws sts get-caller-identity
   ```

O que me travou / o que aprendi apanhando:

- Eu não sabia onde pegar a Access Key. Ela é criada no **IAM**, no seu usuário, em "Credenciais de segurança". Agr eu sei rs...
- Se você coloca a **região errada**, os comandos rodam mas não acham nada

---

## O que eu fiz no lab (passo a passo)

### 1. Criar uma instância
No console da AWS: **EC2 > Instâncias > Executar instância**

- Dei um nome pra ela
- Escolhi uma AMI (sistema operacional)
- Escolhi um tipo pequeno (do nível gratuito)
- Criei um **key pair** e baixei o `.pem`
- Deixei o **security group** liberando só a porta que eu precisava
- Cliquei em **Executar**

A instância começa como `pending` e depois vira `running`.

### 2. Ver as instâncias pela CLI
```bash
aws ec2 describe-instances
```
Sai um textão em JSON, mas dá pra achar o ID da instância e o estado dela ali.

## IMPORTANTE

- Parar a instância **não** zera o custo. O disco continua cobrando.
- Encerrar não tem volta.
- Sempre conferir a **região** lá em cima no console.
- Snapshot é um jeito fácil de fazer backup do disco.

---


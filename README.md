# 📡 action_vps

## 💡 Solução para integrar GitHub Actions com uma VPS via SSH

Este projeto demonstra como utilizar **GitHub Actions** para fazer deploy automático em uma **VPS**, acessando-a via **SSH** com `sshpass`, tudo dentro de um container de action.

---

## 🔐 Como cadastrar os secrets

Para garantir a segurança do acesso à sua VPS, utilizamos **GitHub Secrets** para armazenar dados sensíveis. No caso do `sshpass`, usamos as seguintes variáveis:

| Nome do Secret        | Descrição                          |
|------------------------|-----------------------------------|
| `SSH_USER`             | Usuário de acesso à VPS (ex: root)|
| `SSH_PASSWORD`         | Senha do usuário SSH              |
| `SSH_HOST`             | Endereço IP ou domínio da VPS     |

### 📌 Passo a passo para cadastrar os secrets

1. Acesse o repositório no GitHub
2. Vá em **Settings > Secrets and variables > Actions**
3. Clique em **"New repository secret"**
4. Preencha da seguinte forma:

   - **Name:** `SSH_PASSWORD`  
   - **Secret:** sua_senha_da_vps  

   Repita o processo para `SSH_USER` e `SSH_HOST`.
   
![secret](https://github.com/user-attachments/assets/4622bb3f-9fcd-4392-acbd-e9440776ebae)


### 📌 Passo a passo para fazer o clone na vps

1. Acesse a VPS
2. Faça o clone do repositório
3. Fazer deploy no Github

---

## 🚀 Exemplo do trecho que utiliza sshpass:

No arquivo `.github/workflows/deploy.yml`, usamos o `sshpass` para conectar via SSH automaticamente, sem necessidade de interação:

```yaml
- name: Deploy to production server
  run: |
    sshpass -p '${{ secrets.SSH_PASSWORD }}' ssh -o StrictHostKeyChecking=no ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }} \
    "cd action_vps && git pull && docker compose up -d"
  env:
    SSH_PASSWORD: ${{ secrets.SSH_PASSWORD }}

## Gerenciando-Pol-ticas-em-Acessos-do-Azure
Lab "Criação de Policy para evitar gerar um recurso sem TAG"

# Criar uma Azure Policy que exige TAGs em recursos

### ✅ Objetivo:
Bloquear a criação de qualquer recurso no Azure **sem uma TAG definida**, dentro de um **Resource Group específico**.

---

### 1. **Acessar o Portal do Azure**
- Vá para [https://portal.azure.com](https://portal.azure.com)
- Faça login com sua conta administrativa

---

### 2. **Ir para “Policy”**
- No menu lateral esquerdo, clique em **“Policy”**
- Ou pesquise por “Policy” na barra de busca superior
  <img width="1232" height="342" alt="image" src="https://github.com/user-attachments/assets/1c78935a-f106-4491-be5e-b96f76f3c94f" />


---

### 3. **Criar uma definição de política**
- Clique em **“Definitions”** no menu lateral
- Clique em **“+ Policy definition”**
  <img width="941" height="556" alt="image" src="https://github.com/user-attachments/assets/928cebde-f0a4-4dee-adca-7e04fbc65d76" />


#### Preencha os campos:
- **Name**: `Bloquear recursos sem TAG`
- **Description**: `Impede a criação de recursos sem TAGs atribuídas`
- **Category**: Escolha uma existente ou crie uma nova, como `Governança`
- **Policy Rule**: Cole o seguinte JSON:

```json
{
  "mode": "All",
  "policyRule": {
    "if": {
      "not": {
        "field": "tags",
        "exists": "true"
      }
    },
    "then": {
      "effect": "deny"
    }
  }
}
```

> Esse código verifica se o recurso **não possui nenhuma TAG** e **bloqueia** sua criação.

- Clique em **“Save”**
<img width="923" height="925" alt="image" src="https://github.com/user-attachments/assets/70dd3656-db4b-439e-85ab-8d07d221aa32" />


 

---

### 4. **Atribuir a política ao Resource Group**
- Volte ao menu lateral e clique em **“Assignments”**
- Clique em **“+ Assign Policy”**
  <img width="923" height="450" alt="image" src="https://github.com/user-attachments/assets/ff603e02-691e-4545-b3c5-34d28899bb5d" />


#### Preencha os campos:
- **Scope**: Clique em “...” e selecione o **Resource Group desejado**
- **Policy definition**: Selecione a política que você acabou de criar (`Bloquear recursos sem TAG`)
- **Assignment name**: `Bloqueio de recursos sem TAG`
- Clique em **“Review + Create”** → depois em **“Create”**
  <img width="1058" height="801" alt="image" src="https://github.com/user-attachments/assets/2172c072-68fd-42a6-ba65-98c1263d551f" />



---

### 5. **Testar a política**
- Tente criar um recurso (ex: uma VM ou Storage Account) **sem TAGs** dentro do Resource Group
- O Azure deve **impedir a criação** e exibir uma mensagem de erro
  <img width="829" height="411" alt="image" src="https://github.com/user-attachments/assets/c34de711-0d79-4b71-9e1e-ea427702ea41" />

  

---

## 🧠 Dica Extra: Exigir TAGs específicas
Se quiser **exigir uma TAG com chave específica** (ex: `Owner`), use este JSON:

```json
{
  "if": {
    "not": {
      "field": "tags['Owner']",
      "exists": "true"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

---




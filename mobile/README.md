# 📱 Aplicativo Mobile — IrrigaTech 🌱

> Aplicativo desenvolvido para monitoramento inteligente de plantas, controle de irrigação e acompanhamento das condições do solo em tempo real.

---

## 🌿 1. Sobre o aplicativo

O **IrrigaTech** é um aplicativo mobile desenvolvido utilizando React Native, TypeScript e Expo, integrado ao Supabase para monitoramento inteligente de plantas.

O objetivo é facilitar o acompanhamento das condições de cultivo, permitindo que o usuário visualize informações dos sensores e controle o processo de irrigação de maneira prática e eficiente.

---

## ⚙️ 2. Tecnologias utilizadas

| Tecnologia | Função |
|---|---|
| React Native | Desenvolvimento da interface mobile |
| TypeScript | Tipagem e organização do código |
| Expo | Ambiente de desenvolvimento do aplicativo |
| Expo Router | Navegação entre as telas |
| Supabase | Integração e consulta dos dados |
| PostgreSQL | Armazenamento das informações |
| AsyncStorage | Armazenamento local de informações |

---

## 🚀 3. Funcionalidades desenvolvidas

O aplicativo possui diferentes recursos para facilitar o monitoramento e o controle da irrigação.

### 💧 Monitoramento inteligente

- Monitoramento de três sensores de umidade do solo.
- Acompanhamento da luminosidade do ambiente.
- Consulta das leituras armazenadas no Supabase.
- Atualização automática utilizando Supabase Realtime.
- Indicador de conexão Online/Offline.

### 🚿 Controle de irrigação

- Identificação da necessidade de irrigação quando a umidade é igual ou inferior a 25%.
- Opção de irrigação automática.
- Botão para acionamento manual da irrigação.

### 👤 Gerenciamento de usuários e plantas

- Tela de login.
- Tela de cadastro.
- Tela de perfil do usuário.
- Gerenciamento de plantas.

---

## 🎨 4. Interface do aplicativo

O aplicativo foi desenvolvido com uma interface visual baseada em um tema verde escuro, utilizando elementos que remetem à natureza, tecnologia e sustentabilidade.

A proposta é oferecer uma navegação intuitiva, facilitando o acesso às informações dos sensores e aos controles de irrigação.

---

## 🔄 5. Integração com o Supabase

A integração com o Supabase permite que o aplicativo consulte os registros armazenados no banco de dados e apresente as informações coletadas pelo sistema.

Com o Supabase Realtime, o aplicativo pode receber atualizações automaticamente conforme novas leituras são disponibilizadas.

---

## 🛠️ 6. Como funciona?

O funcionamento do aplicativo acontece por meio das seguintes etapas:

1. **Coleta:** os sensores realizam a leitura da umidade do solo e da luminosidade.
2. **Armazenamento:** as informações são enviadas e registradas no banco de dados.
3. **Consulta:** o aplicativo acessa os dados armazenados no Supabase.
4. **Monitoramento:** o usuário acompanha as condições das plantas pela interface mobile.
5. **Irrigação:** o sistema identifica a necessidade de irrigação e disponibiliza os controles automático e manual.

---

## 🎯 7. Objetivo do projeto

O aplicativo IrrigaTech foi desenvolvido para tornar o monitoramento de plantas mais simples e eficiente.

Por meio da integração entre sensores, aplicativo mobile e banco de dados, o projeto busca auxiliar no cuidado das plantas, melhorar o acompanhamento da umidade do solo e contribuir para o uso consciente da água.

---

**🌱 IrrigaTech — Tecnologia e sustentabilidade trabalhando juntas.**

*Aplicativo mobile desenvolvido com React Native, TypeScript e Expo.*

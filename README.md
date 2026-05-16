# 🏧 Máquina PIX - ESP32

Máquina de vendas autônoma com pagamento via PIX integrado ao Mercado Pago, display TFT, relé para liberação de produto e configuração via Wi-Fi.

---

## ⚙️ Hardware Necessário

| Componente | Detalhe |
|---|---|
| ESP32 Dev Module | Dual Core 240MHz |
| Display TFT ST7735 | 128x160px |
| Relé | Ativo baixo |
| Botão externo | GPIO12 |
| LED | GPIO2 |

### 📌 Pinagem

| Pino | Função |
|---|---|
| GPIO13 | Relé |
| GPIO12 | Botão externo |
| GPIO2 | LED |
| GPIO5 | TFT CS |
| GPIO17 | TFT DC |
| GPIO16 | TFT RST |

---

## 📦 Bibliotecas Necessárias

- ArduinoJson — Benoit Blanchon
- Adafruit GFX Library — Adafruit
- Adafruit ST7735 and ST7789 Library — Adafruit

---

## 🚀 Como Configurar

### 1. Primeiro acesso

Ao ligar sem configuração salva, o ESP32 entra automaticamente no Modo AP.

### 2. Conectar no AP

- Rede: MAQUINA_PIX_CONFIG
- Senha: senha_temporaria
- IP: 192.168.4.1

### 3. Acessar a página de configuração

Abra o browser e acesse http://192.168.4.1

### 4. Preencher os dados

| Campo | Descrição |
|---|---|
| SSID da Rede | Selecione sua rede Wi-Fi |
| Senha da Rede | Senha do Wi-Fi |
| Access Token MP | Token do Mercado Pago (Bearer) |
| Nome da Loja | Aparece na descrição do PIX |
| PosId | ID único da máquina |
| Valor Unitário | Preço por pulso/crédito em R$ |
| Tempo de Pulso | Duração do pulso do relé em ms |

---

## 🔐 Segurança

### Senha de administrador
- Senha padrão: 12345678
- Pode ser alterada na página de configuração
- Necessária para visualizar/editar o Access Token MP

### Access Token MP
- Fica mascarado na tela
- Só é editável após autenticação com senha admin

---

## 🔘 Funções dos Botões

### Botão externo (GPIO12)

| Ação | Função |
|---|---|
| 1 clique em espera | Inicia compra ou adiciona crédito |
| 1 clique aguardando PIX | Cancela a cobrança |

### Botão BOOT (GPIO0) + Botão externo juntos

| Tempo | Ação |
|---|---|
| 3 segundos | Reset total — apaga todas as configurações |

### Botão externo na inicialização

| Tempo | Ação |
|---|---|
| 5 segundos | Entra no Modo AP para reconfigurar |

---

## 💳 Fluxo de Pagamento

1. Cliente aperta botão
2. Acumula valor (R$ x por clique)
3. Timeout 10s sem clique
4. Gera QR Code PIX via Mercado Pago
5. Cliente paga (timeout 3 minutos)
6. Pagamento confirmado
7. Relé aciona N pulsos (valor pago dividido pelo valor unitário)
8. Volta para tela inicial

---

## 🔄 Reset e Recuperação

| Método | Como |
|---|---|
| Reset total (botões) | Segurar BOOT + Botão externo por 3s |
| Modo AP no boot | Segurar botão externo por 5s ao ligar |
| Reset via Serial | Enviar comando RESET com 115200 baud |
| Reset via Web | Acessar http://192.168.4.1/reset |

---

## 📡 Endpoints da API Web (Modo AP)

| Endpoint | Método | Descrição |
|---|---|---|
| / | GET | Página de configuração |
| /save | POST | Salva configurações |
| /scan | GET | Lista redes Wi-Fi disponíveis |
| /reset | GET | Apaga todas as configurações |
| /tools | GET | Ferramentas avançadas |

---

## 🛠️ Comandos Serial (115200 baud)

| Comando | Função |
|---|---|
| RESET | Reset total apaga config |
| RESETWIFI | Reset total apaga config |
| INICIAR | Simula clique do botão |
| PIX | Força geração do QR Code |
| PAGO | Simula pagamento aprovado |

---

## 📋 Histórico de Versões

- 6.0.3 Verificação de pagamento antes do timeout
- 6.0.4 Cancelamento verifica pagamento antes de cancelar
- 6.0.5 Cancelamento invalida cobrança no Mercado Pago
- 6.0.6 Timer countdown no QR Code
- 6.0.7 3 tentativas de conexão Wi-Fi antes do modo AP
- 6.1.0 Scan automático de redes Wi-Fi na configuração
- 6.2.0 Senha admin configurável e token protegido
- 6.3.0 Reset por botão BOOT + botão externo 3 segundos

---

## 👨‍💻 Autor

RetroLuxxo — https://github.com/RetroLuxxo

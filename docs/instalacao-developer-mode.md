# Instalar no LG Developer Mode

Passo a passo para instalar o **webOS Clean Launcher** numa TV LG webOS
usando só o Developer Mode (sem root).

**App ID:** `com.pplferreira.webos-clean-launcher`  
**Pacote:** `com.pplferreira.webos-clean-launcher_<versão>_all.ipk`  
(o artifact `webos-clean-launcher.bundle` é o mesmo `.ipk`; pode renomear
para `.ipk` se a ferramenta exigir.)

PC e TV precisam estar na **mesma rede local**.

---

## 1. Conta de desenvolvedor LG

1. Acesse [webostv.developer.lge.com](https://webostv.developer.lge.com) e faça login
   (ou crie uma conta LG e confirme o e-mail).
2. Use essa mesma conta no app Developer Mode da TV.

Uma conta só mantém Dev Mode ativo em **uma TV por vez**. Se entrar noutra TV
com a mesma conta, a anterior perde o Dev Mode e os apps sideload somem.

---

## 2. Ativar Developer Mode na TV

1. Confirme que a TV está na internet / mesma rede do PC.
2. Abra a **LG Content Store** (LG Apps).
3. Busque **Developer Mode** e instale.
4. Abra o app **Developer Mode**.
5. Faça login com a conta do passo 1.
6. Ative **Dev Mode Status**. A TV reinicia.
7. Abra o Developer Mode de novo e confirme que **Dev Mode Status** está ON.
8. Ative **Key Server** (só precisa estar ligado na hora de conectar o PC pela
   primeira vez / ao buscar a chave).

Anote:

- o **IP** da TV (aparece no app ou em Configurações de rede);
- a **Passphrase** de 6 caracteres (canto do app Developer Mode; é
  case-sensitive).

### Limitações do Dev Mode

- Sessão de cerca de **1000 horas**. Quando acaba, apps instalados via Dev Mode
  são removidos. Renove o tempo no próprio app Developer Mode.
- Após vários reboots sem rede, ou com a sessão expirada, o Dev Mode desliga e
  os apps sideload saem. Basta logar de novo e reativar.

---

## 3. Instalar o pacote (recomendado: webOS Dev Manager)

Ferramenta gráfica, sem precisar do SDK da LG.

1. Baixe o instalador em
   [webosbrew/dev-manager-desktop/releases](https://github.com/webosbrew/dev-manager-desktop/releases)
   (Windows, macOS ou Linux).
2. Abra o **webOS Dev Manager**.
3. **Add Device**:
   - IP da TV
   - Porta: **9922**
   - Usuário: **`prisoner`**
   - Quando pedir, digite a **passphrase** do Developer Mode
4. Na TV, deixe **Key Server** ON enquanto adiciona o device.
5. Com a TV conectada: **Install** (ou equivalente na lista de apps) e escolha
   o arquivo `.ipk` / `webos-clean-launcher.bundle`.
6. Na TV, abra **Clean Launcher** na lista de apps.

O launcher **não substitui o botão Home**. Ele aparece como app normal. Em TV
com root, dá para mapear um botão com
[LG Input Hook](https://github.com/Simon34545/lginputhook) usando o app ID
acima.

---

## 4. Alternativa: webOS CLI (`ares`)

No PC, com Node.js e [`@webos-tools/cli`](https://webostv.developer.lge.com/develop/tools/cli-installation):

```bash
# 1) Cadastrar a TV
ares-setup-device
# escolha add → nome (ex.: tv) → IP → porta 9922 → user prisoner

# 2) Na TV: Key Server ON, depois buscar a chave
ares-novacom --device tv --getkey
# digite a passphrase exibida na TV

# 3) Testar conexão
ares-device --system-info --device tv

# 4) Instalar e abrir
ares-install -d tv com.pplferreira.webos-clean-launcher_1.0.0_all.ipk
ares-launch -d tv com.pplferreira.webos-clean-launcher
```

Ajuste o nome do `.ipk` para a versão que você tiver.

---

## 5. Gerar o `.ipk` a partir do código

```bash
npm install
npm run build
npm run package
# → build/com.pplferreira.webos-clean-launcher_<versão>_all.ipk
```

---

## Problemas comuns

| Sintoma | O que tentar |
|---|---|
| Dev Manager não acha a TV | Mesma Wi‑Fi/LAN; IP certo; Key Server ON; ping no IP da TV |
| Falha na passphrase / chave | Passphrase case-sensitive; Key Server ON; remova e adicione o device de novo |
| App sumiu depois de um tempo | Sessão Dev Mode expirou — renove no app e reinstale o `.ipk` |
| IP muda e perde a conexão | IP estático / reserva DHCP no roteador para a TV |
| Key Server não conecta | Aceite de novo os EULAs em Configurações → Geral → Acordos do usuário, abra o Developer Mode e ligue Key Server |

Documentação oficial LG:
[App Testing with Developer Mode App](https://webostv.developer.lge.com/develop/getting-started/developer-mode-app).

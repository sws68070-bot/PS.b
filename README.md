# Rastreio de Mão — pacote Capacitor pronto pro GitHub

Este pacote contém os arquivos extras que faltam pra transformar o
`gemini-code-1786494563035.html` (rastreamento de mão com MediaPipe) num APK Android.

## Arquivos deste pacote

- `index.html` → cópia do seu HTML original (vai virar a tela do app)
- `capacitor.config.json` → configuração do Capacitor
- `MainActivity.java` → libera a permissão de câmera dentro do WebView
- `AndroidManifest-trechos.xml` → linhas para colar no manifest do Android

**Importante:** `MainActivity.java` e `AndroidManifest-trechos.xml` não são usados
diretamente — eles só existem depois que você rodar `npx cap add android` (isso gera
a pasta `android/` automaticamente). Você vai copiar o conteúdo deles pra dentro
dos arquivos que o Capacitor criar.

---

## Passo a passo

### 1. Criar o repositório no GitHub
Crie um repositório novo (ex: `rastreio-mao-app`) e clone ele localmente:
```bash
git clone https://github.com/SEU_USUARIO/rastreio-mao-app.git
cd rastreio-mao-app
```

### 2. Montar a estrutura do projeto
```bash
npm init -y
npm install @capacitor/core @capacitor/cli @capacitor/camera
npx cap init "RastreioMao" "com.seudominio.rastreiomao"
```
Quando ele perguntar o `webDir`, responda `www`.

### 3. Colocar o HTML na pasta certa
```bash
mkdir www
cp index.html www/index.html
```
(o `index.html` é o arquivo deste pacote — o mesmo conteúdo do seu HTML original)

### 4. Substituir o `capacitor.config.json`
Copie o `capacitor.config.json` deste pacote para a raiz do projeto, substituindo o
que o `cap init` gerou (ele já vem com `androidScheme: https`, que é obrigatório
pra `getUserMedia`/câmera funcionar dentro do WebView).

### 5. Adicionar a plataforma Android
```bash
npx cap add android
```
Isso cria a pasta `android/` com todo o projeto nativo.

### 6. Colar o MainActivity.java
Copie o conteúdo de `MainActivity.java` (deste pacote) para:
```
android/app/src/main/java/com/seudominio/rastreiomao/MainActivity.java
```
(substitua o arquivo que já existe lá — ele por padrão só tem 3 linhas)

### 7. Editar o AndroidManifest.xml
Abra:
```
android/app/src/main/AndroidManifest.xml
```
E cole as permissões de câmera que estão em `AndroidManifest-trechos.xml`
(as instruções de onde colar cada trecho estão comentadas no próprio arquivo).

### 8. Sincronizar
```bash
npx cap sync
```

### 9. Subir tudo pro GitHub
```bash
git add .
git commit -m "Setup inicial do app de rastreio de mão"
git push
```

### 10. Gerar o APK
Você vai precisar do **Android Studio** instalado (não dá pra gerar o APK final
só pelo terminal sem o SDK do Android configurado):
```bash
npx cap open android
```
Isso abre o Android Studio. Lá dentro:
`Build` → `Build Bundle(s) / APK(s)` → `Build APK(s)`

O APK gerado fica em:
```
android/app/build/outputs/apk/debug/app-debug.apk
```

---

## Observações importantes

- **Internet obrigatória:** como o HTML original carrega o MediaPipe via CDN
  (`cdn.jsdelivr.net`), o app vai precisar de conexão com internet para funcionar,
  mesmo depois de instalado. Se quiser que funcione 100% offline, é preciso baixar
  os arquivos do MediaPipe e trocar os `<script src="https://...">` para caminhos
  locais dentro de `www/`.
- **Teste em dispositivo físico:** emuladores geralmente não têm câmera funcional
  de verdade — teste o APK direto num celular Android pra confirmar que a câmera
  está sendo capturada.
- **App em modo debug:** o APK gerado pelo passo 10 é uma build de debug, boa pra
  testar. Pra distribuir de verdade (Play Store ou fora dela), é preciso gerar uma
  build assinada (`release`), o que exige criar uma keystore — posso te ajudar com
  isso se chegar nessa etapa.

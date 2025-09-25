# Guia de Migração para um Projeto Limpo

Este guia detalha o passo a passo para recriar o projeto "Churrasquim Maraponga" do zero, garantindo uma estrutura limpa, com dependências atualizadas e sem arquivos de configuração legados.

---

### Passo 1: Criar o Novo Projeto

Inicie um novo projeto Expo utilizando o template do `expo-router`, que já prepara a estrutura de navegação baseada em arquivos.

```bash
npx create-expo-app NovoChurrasquim --template "expo-router"
cd NovoChurrasquim
```

---

### Passo 2: Instalar as Dependências do Projeto

Instale todas as bibliotecas que o projeto utiliza. Esta lista foi compilada a partir do `package.json` e das nossas conversas anteriores.

```bash
# Copie e cole este comando inteiro no seu terminal
yarn add @supabase/supabase-js @xyzsola/react-native-thermal-printer @expo/vector-icons @react-native-async-storage/async-storage @react-navigation/native expo-constants expo-font expo-linking expo-print expo-splash-screen expo-status-bar expo-system-ui expo-web-browser moti react-native-permissions react-native-reanimated react-native-safe-area-context react-native-screens react-native-url-polyfill react-native-gesture-handler react-native-svg
```

---

### Passo 3: Copiar o Código-Fonte

1.  No projeto antigo, localize a pasta que contém seu código (geralmente `src` ou `app`).
2.  No novo projeto (`NovoChurrasquim`), apague a pasta `app` que foi criada pelo template.
3.  Copie as pastas `src` (ou `app`), `lib` e `plugins` do projeto antigo para a raiz do novo projeto.

---

### Passo 4: Preparar o Ambiente Nativo (Bare Workflow)

Como utilizamos a biblioteca de impressão térmica (`@xyzsola/react-native-thermal-printer`), que requer código nativo, precisamos gerar as pastas `android` e `ios`.

```bash
npx expo prebuild
```

> **Atenção:** Este comando irá perguntar se você deseja sobrescrever alguns arquivos. Geralmente, é seguro aceitar para garantir que as configurações sejam as mais recentes.

---

### Passo 5: Configurar o Projeto Android

1.  **Adicionar Repositório JitPack:**
    Abra o arquivo `android/build.gradle` e verifique se o repositório `maven { url 'https://www.jitpack.io' }` está presente dentro do bloco `allprojects { repositories { ... } }`. O `prebuild` geralmente já faz isso, mas é bom confirmar.

2.  **Adicionar Permissões de Bluetooth:**
    Abra o arquivo `android/app/src/main/AndroidManifest.xml` e adicione as seguintes permissões dentro da tag `<manifest>` (antes da tag `<application>`):

    ```xml
    <uses-permission android:name="android.permission.BLUETOOTH" />
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
    <uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
    <uses-permission android:name="android.permission.BLUETOOTH_SCAN" android:usesPermissionFlags="neverForLocation" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" android:maxSdkVersion="30" />
    ```

---

### Passo 6: Executar o Aplicativo

Com tudo configurado, conecte seu dispositivo Android (com modo de desenvolvedor ativado) e execute o comando para construir e instalar o app.

```bash
npx expo run:android
```

Se tudo correu bem, o aplicativo deve abrir no seu dispositivo, pronto para ser testado.
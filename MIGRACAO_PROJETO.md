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

### Passo 3.5: Verificar e Recriar Assets (Imagens e Fontes)

**Este é o passo mais importante para corrigir o erro de build atual.** Para evitar problemas com arquivos corrompidos do projeto antigo, é crucial recriar ou re-exportar os assets principais.

1.  **Imagens:** Localize os arquivos de imagem originais (como logos e ícones) e exporte-os novamente usando um editor de imagens (Figma, Photoshop, etc.). Salve-os no formato correto (ex: PNG).
2.  **Fontes:** Se usar fontes customizadas, baixe novamente os arquivos (`.ttf`, `.otf`) de suas fontes originais.
3.  Substitua os assets na pasta de assets do seu novo projeto (provavelmente `./assets/images`) pelas versões novas e limpas que você acabou de gerar.

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
    As permissões de Bluetooth são adicionadas automaticamente pelo config plugin `plugins/withBluetoothPermissions.js`, que já está configurado no `app.json`. Nenhuma ação manual é necessária. O comando `npx expo prebuild` cuida de tudo.

---

### Passo 6: Executar o Aplicativo

Com tudo configurado, conecte seu dispositivo Android (com modo de desenvolvedor ativado) e execute o comando para construir e instalar o app.

```bash
npx expo run:android
```

Se tudo correu bem, o aplicativo deve abrir no seu dispositivo, pronto para ser testado.

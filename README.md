# 📸 Open Camera - Hardware Bypass (Ultrawide Default)

Uma modificação no código aberto do aplicativo [Open Camera](https://opencamera.org.br/), desenvolvida para contornar uma falha física de hardware forçando a inicialização direta por um sensor secundário.

## 🚨 O Problema

Após um incidente com água, o sensor principal da câmera do meu smartphone (ID 0) parou de funcionar completamente. No entanto, o sensor Ultrawide (ID 2) permanecia intacto. 

O gargalo: tanto o aplicativo nativo da fabricante quanto aplicativos de terceiros travavam na inicialização, pois o código sempre tentava se comunicar primeiro com a câmera 0. Havia uma solução física viável (a lente 0.5x), mas um bloqueio de software impedia o uso.

## 🛠️ A Solução Técnica

O objetivo deste fork não foi adicionar *features*, mas sim realizar engenharia reversa no processo de inicialização do hardware para injetar uma modificação, que envolveu em:

1. **Atualização do Ambiente Legado:** Migração e resolução de conflitos de versão do Gradle (para 8.10) e Java para compilar um projeto legado em um ambiente moderno.
2. **Manipulação de Preferências (`MainActivity.java`):** 
   * Interceptação dos métodos `getCameraIdPref()` e `getFirstCameraId()` para ignorar as configurações de SharedPreferences e retornar de forma fixada (`hardcoded`) o ID 2.
3. **Bypass de Segurança do Hardware (`Preview.java`):** 
   * Modificação do método `setCamera()` para neutralizar os *guards* de segurança que resetavam o ID para 0 ao receber um número "incomum".
   * Forçamento da flag `useCamera2()` para `true`, garantindo a comunicação via API moderna necessária para enxergar sensores secundários.

## 💡 Principais Aprendizados

* **Troubleshooting Avançado:** A experiência provou que a fronteira entre um hardware "morto" e um dispositivo utilizável muitas vezes reside no controle do software.
* **Leitura de Código de Terceiros:** Navegação, interpretação e modificação de uma base de código (codebase) gigantesca e complexa na qual eu não possuía familiaridade prévia.
* **Resiliência de Compilação:** Resolução profunda de falhas estruturais entre Android SDK, Java e Gradle Wrapper.

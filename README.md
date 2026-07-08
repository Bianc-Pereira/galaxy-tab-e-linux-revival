# Galaxy Tab E Linux Revival

> Transformando um Samsung Galaxy Tab E (SM-T116BU) em um dispositivo Linux funcional.

## 📖 Sobre o projeto

Este projeto documenta todo o processo de revitalização de um Samsung Galaxy Tab E SM-T116BU através da instalação de um sistema operacional Linux.

O objetivo não é apenas reutilizar um dispositivo antigo, mas também explorar conceitos relacionados a:

- Sistemas Embarcados
- Linux ARM
- Bootloaders
- Recovery customizado
- Compilação de kernel
- Engenharia reversa
- Flash de firmware
- Documentação técnica

Todo o processo será registrado, incluindo sucessos, erros, dificuldades e soluções encontradas.

## 🎯 Objetivos

- Instalar um recovery customizado.
- Realizar backup completo da ROM original.
- Instalar o PostmarketOS.
- Avaliar a compatibilidade do hardware.
- Documentar todas as etapas.
- Transformar o tablet em um computador Linux utilizável.

## 📱 Dispositivo

| Item | Valor |
|------|-------|
| Modelo | Samsung Galaxy Tab E |
| Código | SM-T116BU |
| Arquitetura | ARM |
| Memória RAM | 1.5 GB |
| Armazenamento | 8 GB |

## 📂 Estrutura do projeto

```
docs/
├── 01-preparacao.md
├── 02-backup.md
├── 03-recovery.md
├── 04-postmarketos.md
├── 05-testes.md
└── 06-conclusoes.md
```

## 📌 Status

- [x] Escolha do dispositivo
- [x] Preparação do ambiente (Drivers, ADB Platform Tools e WSL2 com Ubuntu 26.04 no PC)
- [x] Instalação do TWRP (Recovery Customizado v3.0.2 funcional)
- [ ] Backup da ROM
- [/] Instalação do Linux (Fase de compilação nativa com pmbootstrap iniciada)
  
## 🚀 Objetivo final

Ao final deste projeto, espera-se que o tablet seja capaz de executar um ambiente Linux funcional para atividades leves, como navegação na web, terminal, edição de arquivos e desenvolvimento básico, além de servir como um estudo prático sobre sistemas Linux embarcados.


# 01. Preparação do Ambiente de Desenvolvimento

Nesta etapa, o ambiente do computador anfitrião (PC) foi configurado para interagir de forma avançada com o tablet e iniciar o processo de engenharia reversa.

## 🛠️ Ferramentas Utilizadas
1. **Drivers USB da Samsung:** Essenciais para o reconhecimento físico do hardware em modo Download/Odin.
2. **Android Platform Tools (Google SDK):** Configuração do utilitário de linha de comando `adb.exe` para comunicação direta através do protocolo de dados.
3. **Subsistema Windows para Linux (WSL 2):** Instalação de uma distribuição nativa do Ubuntu 26.04 LTS dentro do Windows para permitir a compilação do sistema de ficheiros ARM.

## 🚀 Desafios Conhecidos & Resolução de Erros
- **Erro de Comando Não Reconhecido (ADB):** Inicialmente, o terminal do Windows não localizava o executável do ADB. Resolvido através do download direto do pacote isolado do Google e execução do prompt de comando apontando para a diretoria correta (`platform-tools`).


# 03. Instalação e Preparação do Recovery Customizado

O objetivo desta fase foi substituir a partição de recuperação padrão da Samsung por um ambiente robusto capaz de limpar e escrever partições de baixo nível.

## 🔧 Procedimento
1. **Flash do TWRP v3.0.2:** Utilização da interface em lista vertical.
2. **Limpeza Completa da Memória (Wipe):** Através do menu avançado (*Advanced Wipe*), as partições `Dalvik / ART Cache`, `Cache` e `System` foram completamente apagadas. A partição de *Armazenamento Interno* foi poupada.
3. **Ponte de Dados ADB Sideload:** Tentativa inicial de Sideload automatizado via comando `adb reboot sideload` resultou num loop no log da Samsung devido a restrições do bootloader antigo. Resolvido através do acionamento manual do menu `Advanced -> ADB Sideload` diretamente no ecrã do TWRP, abrindo com sucesso a escuta estável para o PC.


# 04. Compilação e Instalação do PostmarketOS

## 🛑 A viragem no Projeto: O Fracasso das Imagens Genéricas
Durante os testes, tentou-se utilizar imagens pré-compiladas genéricas para a arquitetura `armv7` (32-bit) e ambientes de desktop como o KDE Plasma-desktop obtidas diretamente dos servidores espelho do PostmarketOS. 

Contudo, identificou-se que o bootloader rígido da Samsung e o chipset específico **Spreadtrum SC7730SE** rejeitariam um Kernel genérico, resultando em *Kernel Panic* ou falha crítica de inicialização. O dispositivo encontra-se atualmente na categoria "Unmaintained" nos servidores oficiais.

## 🏗️ Solução Adotada: Compilação Nativa via pmbootstrap
Para garantir a funcionalidade de componentes como o ecrã táctil e gestão de energia, optou-se por compilar o sistema sob medida focado no codinome original da placa: **`samsung-gtanm71`**.

### Configuração da Estação de Compilação (PC):
Dentro do ambiente WSL2 Ubuntu, instalou-se a ferramenta de engenharia oficial do ecossistema:
```bash
sudo apt update
sudo apt install pmbootstrap -y
```

---


**Status:** Em desenvolvimento 🚧

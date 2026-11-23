# 🖨️ Controle de Impressora Térmica Elgin em C

Programa de console em **C** que controla uma impressora térmica **Elgin i8 / i9** pela biblioteca oficial da Elgin (`E1_Impressora01.dll`). Ele tem login, menu interativo e 11 operações: imprimir texto, QR Code, código de barras e cupons fiscais SAT, abrir a gaveta de dinheiro e emitir sinal sonoro.

> Projeto acadêmico da disciplina de Programação em C. Testado em uma impressora Elgin real.

---

## 📸 Demonstração

<!-- Coloque aqui um print do menu e uma foto de um cupom impresso -->
<!-- Exemplo: ![Menu do programa](docs/menu.png) -->

```
*************************************************
**************** MENU IMPRESSORA ****************
*************************************************

1  - Configurar Conexao
2  - Abrir Conexao
3  - Fechar Conexao
4  - Imprimir Texto
5  - Imprimir QR Code
6  - Imprimir Codigo de Barras
7  - Imprimir XML SAT
8  - Imprimir XML Cancelamento SAT
9  - Abrir Gaveta Elgin
10 - Abrir Gaveta
11 - Sinal Sonoro
0  - Sair
```

---

## ✨ Funcionalidades

- **Login** com usuário e senha, no máximo 3 tentativas e aviso das tentativas restantes.
- **Menu em laço** até o usuário escolher sair.
- **Conexão USB** com a impressora (configurar, abrir e fechar).
- **Impressões:** texto com várias cópias, QR Code, código de barras CODE 128, cupom fiscal SAT (XML) e cupom de cancelamento SAT.
- **Periféricos:** abertura da gaveta de dinheiro (dois comandos) e sinal sonoro com quantidade escolhida.
- **Segurança de uso:** nenhuma impressão faz perguntas se a conexão estiver fechada.
- **Tratamento de erros:** o retorno de cada comando é conferido, com mensagem de sucesso ou o código do erro.

## 🧠 Destaques técnicos

- **Carregamento dinâmico de DLL** com `LoadLibraryA` e `GetProcAddress` (API do Windows), usando ponteiros para função (`typedef`).
- **Gerenciamento de memória:** os XMLs são lidos com `malloc` e sempre liberados com `free`.
- **Entrada robusta:** leitura com `fgets` e `strtol`; se o usuário digitar letras no lugar de números, o programa pede de novo em vez de travar.
- **Código organizado:** todas as funções de impressora seguem o mesmo padrão de 5 passos (verificar conexão → perguntar → chamar o comando → conferir o retorno → cortar o papel), com comentários em cada etapa.

## 🧰 Tecnologias

| Item | Versão / detalhe |
|---|---|
| Linguagem | C |
| Compilador | gcc 64 bits (MinGW-w64 / WinLibs) |
| Editor | Visual Studio Code |
| Sistema | Windows 10/11 (64 bits) |
| Biblioteca | `E1_Impressora01.dll` da Elgin, versão **x64** |
| Hardware | Impressora térmica Elgin i8 ou i9 (USB) |

## 📁 Estrutura

```
.
├── main.c                 # Código-fonte do programa
├── E1_Impressora01.dll    # Biblioteca da Elgin (x64)
├── XMLSAT.xml             # Cupom SAT de teste (dados fictícios)
├── CANC_SAT.xml           # Cupom de cancelamento de teste (dados fictícios)
└── README.md
```

Todos os arquivos precisam ficar **na mesma pasta**, que é de onde o programa é executado.

---

## 🚀 Como compilar e executar

### 1. Pré-requisitos
- Windows 64 bits
- gcc 64 bits. Confira com:
  ```
  gcc -dumpmachine
  ```
  Deve mostrar `x86_64-w64-mingw32`. Se não tiver o gcc, instale com:
  ```
  winget install BrechtSanders.WinLibs.POSIX.UCRT
  ```
  > Com um compilador de **32 bits** (`mingw32` ou `i686-...`), é preciso usar a versão **x86** da DLL da Elgin.

### 2. Baixar o projeto
```
git clone https://github.com/Gabriel-N-Martins/impressora-elgin-c.git
cd impressora-elgin-c
```
Ou clique em **Code → Download ZIP** e extraia.

### 3. Compilar
```
gcc main.c -o impressora.exe
```

### 4. Executar
Ligue a impressora, conecte o cabo USB e rode:
```
.\impressora.exe
```

---

## 🧪 Como testar

### Login de teste
| Usuário | Senha |
|---|---|
| `admin` | `1234` |

### Com a impressora Elgin
1. Opção **1** e informe: tipo `1` (USB), modelo `i9` (ou `i8`), porta `USB`, parâmetro `0`.
2. Opção **2** deve mostrar "Conexao aberta com sucesso."
3. Teste as opções de **4 a 11**.
4. Opção **3** fecha a conexão e opção **0** sai do programa.

### Sem impressora
Dá para testar a lógica do programa mesmo sem a Elgin:
- Errar a senha 3 vezes encerra o programa com "Acesso negado".
- Digitar letras ou um número fora do menu não trava o programa.
- As opções de **4 a 11** respondem "Conexao fechada" sem fazer perguntas.
- A opção **2** mostra "Erro ao abrir conexao. Codigo de erro: -9999" (impressora não encontrada).

### Problemas comuns
| Mensagem | Causa provável |
|---|---|
| `Nao foi possivel carregar E1_Impressora01.dll (erro 126)` | A DLL não está na mesma pasta do programa |
| `... (erro 193)` | DLL e compilador com bits diferentes (32 x 64) |
| Código `-9999` na opção 2 | Impressora desligada, cabo solto ou modelo errado (`i8` / `i9`) |

---

## 📚 O que aprendi

- Integrar um programa em C com uma biblioteca externa (DLL) e com um hardware real.
- Usar ponteiros para função e a API do Windows para carregar a DLL.
- Gerenciar memória dinâmica com `malloc` e `free`.
- Validar entradas do usuário e conferir o retorno de cada operação.
- Configurar o ambiente de compilação (gcc 64 bits) e versionar o projeto com Git.

## 👥 Autores

(em ordem alfabética)

- Gabriel Ferreira Nobre Martins - 259786
- Julio Cesar de Oliveira Rosario - 260594

# Gestor de Despesas Mensais

Sabes aquela pilha de faturas da água, da luz, da internet, os talões do
supermercado, os bilhetes de comboio… e a conversa de fim de mês sobre quem deve
quanto a quem? Este programa trata disso.

Dás-lhe as faturas e ele devolve-te uma folha de Excel arrumada por ano e por mês,
com tudo somado. Se partilhas casa, ainda faz as contas de quanto paga cada pessoa,
mês a mês, conforme quem lá morava. Se vives sozinho, mostra-te só as tuas despesas.

## O que ele lê

- **PDFs** de faturas, como os que chegam por e-mail;
- **fotografias** de talões, tiradas com o telemóvel;
- o ficheiro que se descarrega do **portal e-fatura** das Finanças.

Não precisas de escrever nada à mão. Ele percebe sozinho de quem é cada fatura,
quanto custou e a que mês pertence. Quando não percebe, diz-te, e resolves com
dois cliques.

## Começar

1. Instala o **Python**, o motor por baixo do programa:
   [python.org/downloads](https://www.python.org/downloads/).
   No Windows, na primeira janela do instalador, marca
   **«Add Python to PATH»**. É o passo que mais gente esquece.
2. Descarrega o **`Despesas-Mensais.zip`** da
   [versão mais recente](https://github.com/tiagompita/gestor-de-despesas/releases/latest)
   e extrai-o para uma pasta à tua escolha.
3. Abre o programa:
   - no **Windows**, dois cliques em `executar_despesas_win.bat`;
   - no **Mac** ou no **Linux**, corre `./executar_despesas.sh` num terminal.

Da primeira vez demora cerca de um minuto a preparar-se. Depois abre um pequeno
assistente que te pergunta o teu nome, se partilhas casa, e que despesas tens.

A partir daí é sempre igual: pões as faturas na pasta `input_faturas`, carregas em
**Atualizar Excel**, e está feito. O botão do lado abre o mapa.

## Versões novas

Não tens de vir aqui buscá-las. Quando houver uma, aparece um **●** no botão
**⚙ Definições**. Lá dentro, em **Atualizações**, carregas em
**Instalar a versão nova**. As tuas faturas, o mapa e a configuração ficam como
estavam.

---

## Guardar o mapa no Google Drive

Isto é **opcional**. Sem isto o programa funciona igual. Mas, se quiseres, no fim
de cada atualização ele envia o mapa e as faturas para uma pasta do teu Google
Drive. Assim tens tudo acessível no telemóvel, e uma cópia de segurança. Só envia o
que mudou, por isso é rápido.

Quem faz o envio é um programinha à parte, o **rclone**. Configura-se uma vez e
nunca mais pensas nele. São três passos.

### 1. Instalar o rclone

- **Windows:** em [rclone.org/downloads](https://rclone.org/downloads/), descarrega
  a versão **«Windows – Intel/AMD – 64 Bit»**. Dentro da pasta do programa, cria uma
  pasta chamada `ferramentas` e extrai lá o zip. O programa encontra-o sozinho.
- **Mac:** `brew install rclone`
- **Linux:** `sudo apt install rclone`

### 2. Dar ao rclone acesso ao teu Drive

Abre um terminal na pasta do programa e escreve:

```
rclone config create upload_despesas_googledrive drive scope=drive
```

> No Windows, troca `rclone` pelo caminho completo do `rclone.exe` que ficou dentro
> de `ferramentas`.

Abre-se o navegador, entras na tua conta Google e carregas em **Permitir**. Já está.

Se quiseres confirmar, este comando tem de mostrar as pastas do teu Drive:

```
rclone lsd upload_despesas_googledrive:
```

### 3. Ligar o envio no programa

Em **⚙ Definições**, liga **«Enviar o mapa e os documentos para o Google Drive»** e
grava. Na próxima atualização, o mapa aparece no teu Drive, numa pasta
**`Despesas`**, dentro de uma subpasta com o teu nome.

### E se quisermos todos a mesma pasta?

Numa casa partilhada dá jeito ter os mapas de toda a gente no mesmo sítio. A forma
segura é esta:

1. Uma pessoa cria uma pasta no Drive e partilha-a com as outras, como
   **Editor**.
2. Cada pessoa abre essa pasta no navegador e copia o código que aparece no
   endereço, depois de `/folders/`.
3. No passo 2, em vez do comando de cima, cada pessoa usa este, com o código no
   fim:

   ```
   rclone config create upload_despesas_googledrive drive scope=drive root_folder_id=O_CODIGO
   ```

Cada um entra com a sua própria conta Google, e os mapas ficam todos lado a lado
na pasta partilhada.

> **Uma regra importante:** não envies a ninguém o ficheiro `rclone.conf`, nem
> aceites o de outra pessoa. Quem o tem entra na conta Google de quem o criou, com
> acesso a tudo, e não só à pasta das despesas.

### Se não estiver a enviar

O programa diz-te sempre porquê, no fim de cada atualização:

- **«O rclone não está instalado»:** falta o passo 1.
- **«…o rclone do sistema não conhece o remote…»:** o passo 2 ficou com outro nome.
  Usa exatamente `upload_despesas_googledrive`, ou escreve o nome que escolheste em
  **⚙ Definições › Remote rclone**.
- **Um erro de autorização do Google:** a ligação expirou. Corre
  `rclone config reconnect upload_despesas_googledrive:` e volta a entrar.

E, se um dia o programa estiver para apagar muitos ficheiros do Drive de uma vez,
ele pergunta-te primeiro.

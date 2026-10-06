# Gestor de Despesas Mensais

Programa que lê faturas e regista as despesas num mapa anual em Excel, organizado
por mês. Numa casa partilhada, divide as contas da casa pelas pessoas que lá
moravam em cada mês. Também pode ser usado só para as despesas de uma pessoa.

## O que ele lê

- **PDFs** de faturas, como os que chegam por e-mail;
- **fotografias** de talões, tiradas com o telemóvel;
- o ficheiro que se descarrega do **portal e-fatura** das Finanças.

Para cada fatura, identifica o emitente, o valor e o mês. As que não consegue
ler ficam numa lista no programa, para lhes dares o valor à mão.

## Começar

1. Instala o **Python** 3.11 ou mais recente:
   [python.org/downloads](https://www.python.org/downloads/).
   No Windows, na primeira janela do instalador, marca
   **«Add Python to PATH»**.
2. Descarrega o **`Despesas-Mensais.zip`** da
   [versão mais recente](https://github.com/tiagompita/gestor-de-despesas/releases/latest)
   e extrai-o para uma pasta à tua escolha.
3. Abre o programa:
   - no **Windows**, dois cliques em `executar_despesas_win.bat`;
   - no **Mac** ou no **Linux**, corre `./executar_despesas.sh` num terminal.

Da primeira vez demora cerca de um minuto a preparar-se e abre um assistente que
pergunta o teu nome, se partilhas casa e que despesas tens.

Depois disso: pões as faturas na pasta `input_faturas` e carregas em
**Atualizar Excel**. O botão **Abrir o Excel** abre o mapa.

## Versões novas

Quando há uma versão nova, aparece um **●** no botão **⚙ Definições**. Instala-se
em **⚙ Definições › Atualizações › Instalar a versão nova**. As faturas, o mapa e a
configuração não são alterados.

---

## Guardar o mapa no Google Drive

Opcional e desligado por omissão. Quando está ligado, no fim de cada atualização
o programa envia o mapa e as faturas arquivadas para uma pasta do teu Google
Drive. Só envia o que mudou desde o último envio.

O envio é feito pelo **rclone**, um programa à parte. Configura-se uma vez, em três
passos.

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

Abre-se o navegador. Entra na tua conta Google e carrega em **Permitir**.

Para confirmar, este comando tem de mostrar as pastas do teu Drive:

```
rclone lsd upload_despesas_googledrive:
```

### 3. Ligar o envio no programa

Em **⚙ Definições**, liga **«Enviar o mapa e os documentos para o Google Drive»** e
grava. Na próxima atualização, o mapa aparece no teu Drive, numa pasta
**`Despesas`**, dentro de uma subpasta com o teu nome.

### E se quisermos todos a mesma pasta?

Para guardar os mapas de várias pessoas na mesma pasta:

1. Uma pessoa cria uma pasta no Drive e partilha-a com as outras, como
   **Editor**.
2. Cada pessoa abre essa pasta no navegador e copia o código que aparece no
   endereço, depois de `/folders/`.
3. No passo 2, em vez do comando de cima, cada pessoa usa este, com o código no
   fim:

   ```
   rclone config create upload_despesas_googledrive drive scope=drive root_folder_id=O_CODIGO
   ```

Cada pessoa usa a sua conta Google, e cada mapa fica numa subpasta com o nome da
pessoa, dentro da pasta partilhada.

> **Não partilhes o ficheiro `rclone.conf`.** Dá acesso total à conta Google de
> quem o criou, e não só à pasta das despesas.

### Se não estiver a enviar

O motivo aparece no fim de cada atualização:

- **«O rclone não está instalado»:** falta o passo 1.
- **«…o rclone do sistema não conhece o remote…»:** o passo 2 ficou com outro nome.
  Usa exatamente `upload_despesas_googledrive`, ou escreve o nome que escolheste em
  **⚙ Definições › Remote rclone**.
- **Um erro de autorização do Google:** a ligação expirou. Corre
  `rclone config reconnect upload_despesas_googledrive:` e volta a entrar.

Antes de apagar mais de 20 ficheiros do Drive de uma vez, o programa pede
confirmação.

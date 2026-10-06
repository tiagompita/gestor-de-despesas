# Gestor de Despesas Mensais

Lê as tuas faturas (PDF, fotografias de talões, o CSV do portal e-fatura) e escreve
um mapa anual em Excel. Numa casa partilhada, divide as contas da casa por quem lá
morava em cada mês.

Este repositório tem só as **versões publicadas** do programa. Não precisas de conta
no GitHub para as descarregar.

## Instalar

1. Abre a [última versão](https://github.com/tiagompita/gestor-de-despesas/releases/latest)
   e descarrega o **`Despesas-Mensais.zip`**.
2. Extrai-o para uma pasta à tua escolha.
3. Arranca o programa:
   - **Windows:** duplo clique em `executar_despesas_win.bat`;
   - **Linux e macOS:** `./executar_despesas.sh` num terminal.

Precisas do Python 3.11 ou mais recente ([python.org](https://www.python.org/downloads/)).
No Windows, marca **«Add Python to PATH»** ao instalar. Na primeira vez o programa
prepara o ambiente sozinho (cerca de um minuto) e abre o assistente de configuração.

O `LEIA-ME.txt`, dentro do zip, explica o resto.

## Atualizar

No programa: **⚙ Definições › Atualizações › Instalar a versão nova**. A
configuração, as faturas e o mapa não são tocados.

---

## Enviar o mapa para o Google Drive (opcional)

O programa pode enviar o mapa e as faturas arquivadas para uma pasta do Google
Drive no fim de cada atualização. Só envia o que mudou. **Vem desligado**: sem
isto o programa funciona igual, só não envia nada.

O envio é feito pelo [rclone](https://rclone.org/), um programa à parte. Configura-se
uma vez.

### 1. Instalar o rclone

| Sistema | Como |
|---|---|
| Windows | Descarrega o zip «Windows – Intel/AMD – 64 Bit» de [rclone.org/downloads](https://rclone.org/downloads/). Dentro da pasta do programa, cria uma pasta `ferramentas` e extrai lá o zip: fica `ferramentas\rclone-v1.xx-windows-amd64\rclone.exe`. O programa encontra-o aí sozinho. |
| macOS | `brew install rclone` |
| Linux | `sudo apt install rclone` |

### 2. Ligar o rclone ao teu Google Drive

Num terminal, na pasta do programa (no Windows, usa o caminho do `rclone.exe` do
passo anterior em vez de `rclone`):

```
rclone config create upload_despesas_googledrive drive scope=drive
```

Abre-se o navegador. Entra na tua conta Google e autoriza o acesso. No fim, o
terminal diz que a configuração foi guardada.

O nome `upload_despesas_googledrive` é o que o programa espera. Se escolheres outro,
escreve-o em **⚙ Definições › Remote rclone**.

Para confirmar que ficou a funcionar:

```
rclone lsd upload_despesas_googledrive:
```

Tem de listar as pastas do teu Drive.

### 3. Ligar o envio no programa

Em **⚙ Definições**, liga **«Enviar o mapa e os documentos para o Google Drive»** e
grava. Na atualização seguinte, o mapa vai para a pasta
**`Despesas/<o teu nome>`** do teu Drive. O nome da pasta de cima muda-se em
**⚙ Definições › Pasta destino Cloud**.

### Uma pasta comum para a casa toda

Se quiserem que os mapas de todos fiquem numa só pasta, **não partilhem o ficheiro
de configuração do rclone**: ele dá acesso total à conta Google de quem o criou.
Em vez disso:

1. Quem tem a pasta partilha-a no Google Drive com cada pessoa, como **Editor**.
2. Cada pessoa abre a pasta no navegador e copia o identificador do endereço, a
   parte depois de `/folders/`.
3. Cada pessoa liga o rclone à **sua própria** conta, apontado para essa pasta:

   ```
   rclone config create upload_despesas_googledrive drive scope=drive root_folder_id=O_IDENTIFICADOR
   ```

Os mapas ficam em `<pasta partilhada>/Despesas/<nome de cada um>`, cada um na sua
subpasta.

### Onde fica a configuração do rclone

O `rclone config create` guarda-a no sítio habitual do rclone
(`rclone config file` diz qual), e o programa usa-a daí. Se puseres um ficheiro
`rclone.conf` na pasta do programa, é esse que manda: útil para levar a pasta para
outro computador já configurada. **Nunca o partilhes nem o publiques.**

### Se o envio não funcionar

O programa diz no fim de cada atualização porque é que não enviou:

| Mensagem | O que fazer |
|---|---|
| «O rclone não está instalado» | Volta ao passo 1 |
| «Falta o .rclone.conf (ou rclone.conf) nesta pasta, e o rclone do sistema não conhece o remote» | O nome do remote em ⚙ Definições não é o que criaste no passo 2 |
| Um erro de autorização do Google | A ligação expirou. Corre `rclone config reconnect upload_despesas_googledrive:` |

Antes de apagar mais de 20 ficheiros da pasta do Drive de uma vez, o programa
pergunta numa janela.

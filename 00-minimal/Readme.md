## Exemplo Blink

Esse é um exemplo básico para um projeto com a placa Franzininho C0.

Foram configurados os seguintes periféricos:
- LED1 - Pino PB6 - OUTPUT
- LED2 - Pino PB7 - OUTPUT
- Button - Pino PA8 - INPUT - PULL UP
- Clock do sistema: interno 48 MHz

Aplicação exemplo:
- Inverte o estado dos LEDs 1 e 2 a cada 500 milissegundos.

## Software

STM32CubeIDE Version: 1.16.1

## Estrutura do projeto

```bash
$ tree -d -L 2
.
├── Core
│   ├── Inc
│   ├── Src
│   └── Startup
└── Drivers
    ├── CMSIS
    └── STM32C0xx_HAL_Driver
```

### `Core`

Primariamente arquivos gerados pelo STM32CubeIDE, incluindo configuração da lib HAL para os periféricos ativos, códigos de inicialização e função `main`.

É geralmente o local para implementação da camada de aplicação e bibliotecas do projeto.

### `Drivers`

Bibliotecas STM32 HAL DRIVER e CMSIS fornecidas pelo STM32CubeIDE. Não possui código customizado pelo projeto.

## Programação

A placa Franzininho C0 é gravada pela porta USB, que está conectada ao um conversor USB-Serial CH341.

O software [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html) deve ser utilizado para comunicar com o bootloader interno do microcontrolador e executar a gravação da memória Flash.

1. Abrir o STM32CubeProgrammer
2. Conectar a placa Franzininho C0 na porta USB
3. No canto superior direito, escolher UART como interface de gravação e configurar:
  - Port: selecionar correspondente à Franzininho C0 (utilizar o refresh se necessário)
  - Baudrate: 115200
  - Parity: Even
  - RTS/DTR: Low (indiferente)
4. Segurar botão BOOT0, pressionar e soltar RST, soltar BOOT0 para iniciar execução do bootloader UART.
5. Clicar em CONNECT
6. Verificar logs de execução, deve indicar "Data read successfully". Volte ao passo 2 em caso de falhas.
7. Abrir "Erase & Programming" no menu lateral esquerdo.
8. Clicar em "Browse" e selecionar arquivo "Debug/stm32c0_sample.elf"
9. Marcar "Run after programming"
10. Clicar em "Start Programming"
11. Logs de execução devem indicar "File download complete" e "Start operation achieved successfully".
12. O aviso "Connection to device 0x443 is lost" é esperado, já que o software perde a comunicação com o bootloader após o início da execução aplicação.
12. Verificar que o LED B6 está piscando na placa

A porta serial deve estar sem uso para executar a gravação, feche seções de softwares terminal serial para executar a gravação.

O STM32CubeProgrammer pode ser mantido aberto, retorne ao passo 4 para gravações posteriores.

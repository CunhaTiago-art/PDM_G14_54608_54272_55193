# Sensores, localização, câmara e ficheiros em Android

**Práticas de desenvolvimento para dispositivos móveis** · Grupo 14

| Nº | Elemento |
|----|----------|
| 54608 | André Pires / João Leitão / Tiago Cunha |
| 54272 | André Pires / João Leitão / Tiago Cunha |
| 55193 | André Pires / João Leitão / Tiago Cunha |

> Completar a correspondência entre número e nome antes da entrega.

Este repositório contém a app de demonstração (`Demo/`) e a apresentação (`.pptx`) do trabalho sobre acesso a sensores, localização, câmara, fotografias e ficheiros em Android, com exemplos em Kotlin e Java.

## Conteúdo

```
.
├── Demo/                                   Projeto Android Studio (Kotlin)
│   └── app/src/main/
│       ├── java/com/example/demo/MainActivity.kt
│       ├── res/layout/activity_main.xml
│       └── AndroidManifest.xml
└── Sensores, localização, câmara e ficheiros em Android.pptx   Apresentação
```

## O que a app demonstra

Uma única `MainActivity` com quatro secções, cada uma ligada a um tema da apresentação:

| Secção | API usada | Permissão |
|--------|-----------|-----------|
| **Sensores** | `SensorManager`: acelerómetro (com deteção de abanadelas, módulo > 15 m/s²) e sensor de luz | Nenhuma |
| **Localização** | Fused Location Provider, `getCurrentLocation()` com prioridade alta (precisa) ou equilibrada (aproximada) | `ACCESS_FINE_LOCATION`, `ACCESS_COARSE_LOCATION` |
| **Câmara** | Câmara do sistema (`TakePicture`, guarda em `Pictures/DemoApp` via `MediaStore`) e pré-visualização com **CameraX** | `CAMERA` |
| **Fotografias** | Photo Picker (`PickVisualMedia`) | Nenhuma |
| **Ficheiros** | Storage Access Framework: `OpenDocument` para abrir `.txt` e `CreateDocument` para guardar `nota.txt` | Nenhuma |

Decisões de implementação:

- Os sensores são registados em `onResume()` e desregistados em `onPause()`, para poupar bateria. Se o dispositivo não tiver sensor de luz, a app indica-o no ecrã.
- A localização pede as duas permissões em conjunto e trata os três resultados: precisa, só aproximada e recusada. Na recusa, a app continua a funcionar.
- A permissão de câmara é pedida só quando o utilizador carrega num dos botões de câmara. A ação fica guardada e é executada se a permissão for concedida.
- Se a foto da câmara do sistema for cancelada, a entrada vazia criada no `MediaStore` é apagada.

## Requisitos

- Android Studio recente, com o JDK incluído
- Android 10 ou superior (`minSdk 29`; `compileSdk`/`targetSdk` 37)
- Gradle 9.5.0 e Android Gradle Plugin 9.3.3 (o *wrapper* já está incluído)
- Para a localização: dispositivo ou emulador com Google Play services
- Dependências: `play-services-location` 21.3.0 e CameraX 1.3.4

## Como executar

1. Abrir a pasta `Demo/` no Android Studio e esperar pela sincronização do Gradle.
2. Escolher um dispositivo físico ou um emulador (Android 10+).
3. Executar a configuração `app`.

Em linha de comandos:

```bash
cd Demo
./gradlew assembleDebug
```

## Protocolo experimental

Cada funcionalidade deve ser testada nos três cenários de permissão: **concedida**, **recusada** e **revogada nas Definições**. O comportamento esperado é o seguinte.

| # | Funcionalidade | Passos | Resultado esperado |
|---|----------------|--------|--------------------|
| 1 | Acelerómetro | Abrir a app e mover o dispositivo | Valores x, y, z e módulo atualizam em tempo real |
| 2 | Abanadela | Abanar com força (módulo > 15 m/s²) | O contador "Abanadelas" sobe e aparece um *toast*; no máximo uma deteção por segundo |
| 3 | Luz | Tapar e destapar o sensor | O valor em lux muda; sem sensor, aparece "sensor indisponível" |
| 4 | Pausa dos sensores | Enviar a app para segundo plano e voltar | Os valores deixam de atualizar em segundo plano e retomam ao voltar |
| 5 | Localização precisa | "Obter localização" → conceder localização precisa | Latitude e longitude com a indicação "precisa" |
| 6 | Localização aproximada | Conceder só a aproximada (Android 12+) | Latitude e longitude com a indicação "aproximada" |
| 7 | Localização recusada | Recusar as duas permissões | Mensagem de recusa; a app continua utilizável |
| 8 | Localização desligada | Desligar a localização do telemóvel | "Sem localização (ative a localização do telemóvel)" |
| 9 | Câmara do sistema | Carregar no botão e conceder `CAMERA` | Abre a câmara; a foto aparece na app e fica em `Pictures/DemoApp` |
| 10 | Câmara cancelada | Sair da câmara sem tirar foto | Nenhuma entrada vazia fica na galeria |
| 11 | CameraX | Carregar no botão e conceder `CAMERA` | Pré-visualização da câmara traseira na app |
| 12 | Câmara recusada | Recusar `CAMERA` | *Toast* "Câmara recusada" |
| 13 | Câmara revogada | Revogar `CAMERA` nas Definições e repetir | A permissão é pedida de novo |
| 14 | Photo Picker | Escolher uma imagem | A imagem aparece na app, sem pedir permissões |
| 15 | Abrir ficheiro | Escolher um `.txt` | O conteúdo aparece na caixa de texto |
| 16 | Guardar ficheiro | Escrever texto e guardar `nota.txt` | O ficheiro é criado onde o utilizador escolher e aparece "Ficheiro guardado" |

## Entrega

- Repositório com acesso de leitura ao protocolo experimental e ao código desenvolvido.
- Submissão do link por um único elemento do grupo, com indicação do número do grupo (**14**).
- Disponível na véspera da data de apresentação e mantido até ao final do ano letivo.

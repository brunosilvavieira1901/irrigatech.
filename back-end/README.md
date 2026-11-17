# 🔌 Firmware e Sensores — IrrigaTech 🌱

> Documentação do sistema embarcado responsável pela leitura dos sensores, processamento das informações e controle automático da irrigação.

---

## 🌿 1. Sobre o sistema

O firmware do IrrigaTech foi desenvolvido utilizando MicroPython e executado em um microcontrolador ESP8266.

Sua função é realizar a leitura dos sensores conectados ao sistema, processar os dados coletados e controlar o funcionamento da bomba de água de acordo com a umidade do solo.

O sistema também realiza a comunicação com um servidor Python, enviando as informações coletadas para serem utilizadas pelas demais partes do projeto.

---

## ⚙️ 2. Tecnologias e componentes utilizados

| Tecnologia / Componente | Função |
|---|---|
| ESP8266 | Microcontrolador principal |
| MicroPython | Linguagem utilizada no firmware |
| ADS1115 | Conversor analógico-digital |
| Sensores de umidade | Monitoramento da umidade do solo |
| Sensor DS18B20 | Medição da temperatura |
| Sensor de luminosidade | Monitoramento da iluminação |
| Módulo relé | Controle da bomba de água |
| Bomba de água | Realização da irrigação |
| HTTP / JSON | Comunicação e envio dos dados |
| Servidor Python | Recebimento das informações |

---

## 📡 3. Leitura dos sensores

O sistema utiliza o conversor ADS1115 para realizar a leitura dos valores analógicos dos sensores.

A função `ler_ads1115(canal)` configura o canal de leitura, solicita os dados ao conversor e transforma os valores recebidos em números que podem ser processados pelo programa.

```python
def ler_ads1115(canal):
    mux = 4 + canal
    config = 0x8000 | (mux << 12) | 0x0200 | 0x0100 | 0x0080 | 0x0003
    dados_config = bytes([
        0x01,
        (config >> 8) & 0xFF,
        config & 0xFF
    ])
    i2c.writeto(ADS_ADDR, dados_config)
    time.sleep_ms(10)
    i2c.writeto(ADS_ADDR, b"\x00")
    dados = i2c.readfrom(ADS_ADDR, 2)
    valor = (dados[0] << 8) | dados[1]

    if valor > 32767:
        valor -= 65536

    if valor < 0:
        valor = 0

    return valor
```

---

## 💧 4. Processamento dos dados de umidade

Após realizar as leituras, o sistema utiliza a função `converter_umidade()` para transformar os valores dos sensores em porcentagens.

O cálculo utiliza os valores de referência de solo seco e solo úmido, limitando o resultado entre 0% e 100%.

```python
def converter_umidade(valor, seco, umido):
    if valor == 0:
        return 0

    porcentagem = ((seco - valor) * 100 // (seco - umido))

    if porcentagem < 0:
        porcentagem = 0

    if porcentagem > 100:
        porcentagem = 100

    return porcentagem
```

---

## ☀️ 5. Conversão da luminosidade

A função `converter_luminosidade()` processa o valor recebido e converte a leitura para uma escala de 0 a 100.

Isso facilita a apresentação dos dados no aplicativo.

```python
def converter_luminosidade(valor):
    if valor < 0:
        valor = 0

    if valor > 32767:
        valor = 32767

    return valor * 100 // 32767
```

---

## 🌡️ 6. Monitoramento da temperatura

O sistema utiliza o sensor DS18B20 para realizar a medição da temperatura do ambiente.

A função `ler_temperatura()` verifica se o sensor está disponível, solicita uma conversão de temperatura e retorna o valor registrado.

Caso ocorra algum erro durante a leitura, o programa apresenta uma mensagem no terminal.

```python
def ler_temperatura():
    if not roms:
        return None

    try:
        ds_sensor.convert_temp()
        time.sleep_ms(750)
        return ds_sensor.read_temp(roms[0])

    except Exception as erro:
        print("Erro DS18B20:", erro)
        return None
```

---

## 🚿 7. Controle automático da bomba

O controle da irrigação é realizado pela função `controlar_bomba()`.

O sistema compara a umidade do solo com o limite mínimo configurado na variável `UMIDADE_MINIMA`.

- Se a umidade estiver abaixo do limite, a bomba será ligada.
- Se a umidade estiver igual ou acima do limite, a bomba será desligada.

```python
def controlar_bomba(umidade):
    if umidade < UMIDADE_MINIMA:
        rele.value(RELE_LIGADO)
        print("SOLO SECO - BOMBA LIGADA")

    else:
        rele.value(RELE_DESLIGADO)
        print("SOLO UMIDO - BOMBA DESLIGADA")
```

---

## 🌐 8. Comunicação com o servidor Python

Após realizar o processamento das informações, o ESP8266 organiza os dados em um objeto JSON e envia as informações ao servidor Python utilizando uma requisição HTTP POST.

Os dados enviados incluem:

- Umidade do sensor 1.
- Umidade do sensor 2.
- Umidade do sensor 3.
- Média das leituras.
- Luminosidade.
- Temperatura.
- Estado da bomba.

```python
def enviar_para_python(sensor1, sensor2, sensor3, media,
                       luminosidade, temperatura, bomba):

    dados = {
        "sensor1": sensor1,
        "sensor2": sensor2,
        "sensor3": sensor3,
        "media": media,
        "luminosidade": luminosidade,
        "temperatura": temperatura,
        "bomba": bomba
    }

    payload = ujson.dumps(dados)
    headers = {"Content-Type": "application/json"}

    try:
        gc.collect()

        resposta = urequests.post(
            URL_SERVIDOR_PYTHON,
            headers=headers,
            data=payload
        )

        print("Resposta do Servidor Python:", resposta.text)
        resposta.close()

    except Exception as e:
        print("Erro ao enviar para o Python:", e)
```

---

## 🔄 9. Funcionamento geral do sistema

O funcionamento do firmware acontece por meio das seguintes etapas:

1. **Inicialização:** o ESP8266 prepara os componentes e as conexões.
2. **Leitura:** os sensores coletam informações sobre a umidade e as condições do ambiente.
3. **Processamento:** os valores são convertidos e organizados pelo programa.
4. **Temperatura:** o DS18B20 realiza a medição da temperatura.
5. **Controle:** o sistema verifica a necessidade de irrigação e controla a bomba.
6. **Comunicação:** os dados são enviados ao servidor Python por meio de HTTP.
7. **Monitoramento:** as informações podem ser utilizadas pelo aplicativo IrrigaTech.

---

## 🎯 10. Objetivo do firmware

O firmware foi desenvolvido para automatizar a coleta de informações e o controle da irrigação, reduzindo a necessidade de acompanhamento manual constante.

A integração entre ESP8266, sensores, bomba de água, servidor Python e aplicativo permite construir um sistema inteligente de monitoramento de plantas, contribuindo para o uso consciente da água e para a sustentabilidade.

---

**🌱 IrrigaTech — Tecnologia e sustentabilidade trabalhando juntas.**

*Firmware desenvolvido em MicroPython para ESP8266.*

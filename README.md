# stm32-filtro-cmsis

Filtro FIR passa-baixa completo com ADC, DAC e DMA no STM32F4xx usando CMSIS-DSP.

## Descrição

Implementação de sistema de filtragem digital em tempo real: aquisição do sinal via ADC com DMA, filtragem FIR passa-baixa usando a função otimizada `arm_fir_f32` da CMSIS-DSP, e saída via DAC. Aproveita a FPU e instruções SIMD do Cortex-M4.

## Hardware

- Microcontrolador: STM32F4xx (Cortex-M4 + FPU)
- Entrada: ADC1
- Saída: DAC

## Parâmetros do filtro

| Parâmetro | Valor |
|-----------|-------|
| Tipo | FIR passa-baixa |
| Número de coeficientes | 512 |
| Tamanho do bloco | 1 |
| Tipo de dado | `float32_t` |

## Pipeline de processamento

```
ADC (DMA) → buffer entrada → arm_fir_f32() → buffer saída → DAC
```

## Periféricos

| Periférico | Função |
|------------|--------|
| ADC1 | Aquisição do sinal |
| DMA | Transferência ADC → memória |
| DAC | Saída do sinal filtrado |
| TIM | Trigger de amostragem |

## Dependências

- ARM CMSIS-DSP Library (`arm_math.h`)
- `#define ARM_MATH_CM4` e `__FPU_PRESENT 1`

## IDE

Atollic TrueSTUDIO 9.3 / STM32CubeIDE

## Escola

Centro Tecnológico Liberato — Novo Hamburgo/RS

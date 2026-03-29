# STM32 Filtro CMSIS — FIR com CMSIS-DSP

🇧🇷 **Português** | 🇺🇸 [English](#english)

---

## Português

Filtro FIR em tempo real no STM32F4xx usando a biblioteca CMSIS-DSP (`arm_fir_f32`), pipeline ADC→DMA→FIR→DAC+DMA acionado por TIM2.

### O que faz
- **ADC1 + DMA** captura amostras de `adcValue[]`
- No callback `HAL_ADC_ConvCpltCallback`, aplica **filtro FIR** com `arm_fir_f32()`
- **512 coeficientes** definem a resposta em frequência do filtro
- Saída filtrada enviada para **DAC + DMA** via `adcValue1[]`
- **TIM2** aciona o ADC com trigger periódico

### Pipeline de processamento
```
PA0 → ADC1+DMA → adcValue[] → arm_fir_f32(512 coefs) → adcValue1[] → DAC+DMA → PA4
                                        ↑
                               HAL_ADC_ConvCpltCallback
```

### Código chave
```c
arm_fir_init_f32(&instancia, AMOSTRAS, &coeficientes[0], &firStateF32[0], blocksize);

// No callback:
entrada = (float32_t)adcValue[0];
arm_fir_f32(&instancia, &entrada, &saida, blocksize);
```

### Requisitos
- STM32F4xx com **FPU** (unidade de ponto flutuante)
- Biblioteca **CMSIS-DSP** (inclusa no STM32Cube)
- Atollic TrueSTUDIO

---

## English

Real-time FIR filter on STM32F4xx using the CMSIS-DSP library (`arm_fir_f32`), with an ADC→DMA→FIR→DAC+DMA pipeline triggered by TIM2.

### What it does
- **ADC1 + DMA** captures samples into `adcValue[]`
- In `HAL_ADC_ConvCpltCallback`, applies **FIR filter** with `arm_fir_f32()`
- **512 coefficients** define the filter frequency response
- Filtered output sent to **DAC + DMA** via `adcValue1[]`
- **TIM2** triggers the ADC periodically

### Processing pipeline
```
PA0 → ADC1+DMA → adcValue[] → arm_fir_f32(512 coefs) → adcValue1[] → DAC+DMA → PA4
                                        ↑
                               HAL_ADC_ConvCpltCallback
```

### Key code
```c
arm_fir_init_f32(&instancia, AMOSTRAS, &coeficientes[0], &firStateF32[0], blocksize);

// In callback:
entrada = (float32_t)adcValue[0];
arm_fir_f32(&instancia, &entrada, &saida, blocksize);
```

### Requirements
- STM32F4xx with **FPU** (floating-point unit)
- **CMSIS-DSP** library (included in STM32Cube)
- Atollic TrueSTUDIO

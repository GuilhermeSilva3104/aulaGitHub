# Relatório — Lab05a: Besouro Articulado  

> **Objetivo:** criar um modelo hierárquico e animado de um besouro 3D em Three.js, com controles via GUI.

## Criação do modelo

* **Primitivas Three.js**  
  * `SphereGeometry` — abdômen, tórax e olhos  
  * `ConeGeometry` — cabeça e ponta do chifre  
  * `CylinderGeometry` — coxas, fêmures, tíbias, fíbias e haste do chifre  

* **Estrutura hierárquica**  
  * `grupoBesouro` (raiz)  
    * `grupoCorpo`  
      * três `grupoPar` (frontal, medial, posterior)  
        * pivôs de quadril esquerdo/direito → coxa → fêmur → tíbia → fíbia  
      * `grupoCabeca` → `grupoChifre`  
  * Cada pivô corresponde ao eixo de rotação de uma junta.

## Graus de liberdade implementados

| Componente                       | Eixo | DoF por instância | Instâncias | Total |
|----------------------------------|------|-------------------|------------|-------|
| Rotação global do corpo          | Y    | 1 | 1 | 1 |
| Cabeça (inclinação)              | Z    | 1 | 1 | 1 |
| Chifre (elevação)                | Z    | 1 | 1 | 1 |
| Quadril                          | X    | 1 | 6 | 6 |
| Fêmur                            | X    | 1 | 6 | 6 |
| Tíbia                            | X    | 1 | 6 | 6 |
| Fíbia                            | X    | 1 | 6 | 6 |
| **Total**                        |      |   |   | **27 DoF** |

*Os controles da GUI agrupam as juntas em três pares (frontal, medial, posterior); internamente, as pernas esquerda ↔ direita são espelhadas e mantêm todos os 27 DoF.*

## Movimento simulado

* **Caminhada automática**  
  * Cada par de pernas oscila no eixo X com fase de 120 ° entre pares e 180 ° entre lados, reproduzindo um padrão de passada estável.  
  * A altura do passo segue um perfil de Bézier cúbica para suavizar o contato com o solo.

* **Avanço global**  
  * Com `animacaoAutomatica` ativo, o besouro translada no eixo X a uma velocidade proporcional a `progressoLinearPorCiclo`.

* **Controles via GUI**  
  * Alterna entre animação automática e pose manual.  
  * Ajusta velocidade ω, amplitude do passo, altura do levantamento e direção global.  
  * Permite inclinação da cabeça, elevação do chifre e ângulos de quadril, fêmur, tíbia e fíbia (espelhados entre lados) para cada par de pernas.

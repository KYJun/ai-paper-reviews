---
title: "Attention Is All You Need — Transformer의 탄생"
date: 2026-03-03
categories: [LLM]
tags: [transformer, attention, seq2seq, google]
math: true
---

## 논문 정보

| 항목 | 내용 |
|---|---|
| **제목** | Attention Is All You Need |
| **저자** | Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Łukasz Kaiser, Illia Polosukhin |
| **학회** | NeurIPS 2017 |
| **링크** | [arXiv:1706.03762](https://arxiv.org/abs/1706.03762) |

---

## 핵심 기여

RNN이나 CNN 없이 **Self-Attention만으로** 시퀀스 변환(sequence transduction) 문제를 해결하는 **Transformer** 아키텍처를 제안했습니다. 이 논문은 현대 LLM의 근간이 되는 구조를 처음 소개한 기념비적인 연구입니다.

- 기존 RNN 기반 모델의 **순차 처리 병목**을 제거하여 병렬화 가능
- **Multi-Head Attention** 메커니즘으로 다양한 표현 부분공간에서 정보 추출
- **Positional Encoding**으로 순서 정보를 보존하면서도 재귀 없는 구조 실현

---

## 방법론

### Self-Attention 메커니즘

입력 시퀀스의 각 위치가 다른 모든 위치에 대한 가중치를 계산합니다:

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

- **Q** (Query), **K** (Key), **V** (Value)는 입력의 선형 변환
- $\sqrt{d_k}$로 나누는 **Scaled Dot-Product**로 학습 안정성 확보

### Multi-Head Attention

단일 어텐션 대신, 여러 개의 어텐션 헤드를 병렬로 계산하여 결합합니다:

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \ldots, \text{head}_h)W^O
$$

이를 통해 모델이 서로 다른 위치에서 서로 다른 표현을 동시에 학습할 수 있습니다.

### Encoder-Decoder 구조

- **Encoder**: 6개 동일 레이어 (Self-Attention + Feed-Forward)
- **Decoder**: 6개 동일 레이어 (Masked Self-Attention + Cross-Attention + Feed-Forward)
- 각 서브레이어에 **잔차 연결(Residual Connection)**과 **Layer Normalization** 적용

---

## 실험 결과

### 기계 번역 (WMT 2014)

| 모델 | EN-DE BLEU | EN-FR BLEU |
|---|---|---|
| 기존 SOTA (Ensemble) | 26.36 | 41.29 |
| **Transformer (Big)** | **28.4** | **41.8** |

- 영어→독일어: 기존 최고 성능 대비 **+2.0 BLEU** 향상
- 영어→프랑스어: 단일 모델로 앙상블 모델을 능가
- 학습 비용: 기존 모델 대비 **훈련 시간 대폭 감소** (8 P100 GPU, 3.5일)

### Ablation Study

- 어텐션 헤드 수: $h=8$이 최적
- Key 차원: $d_k$를 줄이면 성능 하락
- 모델 크기를 키울수록 성능 향상 (Big 모델이 Base 모델보다 일관적으로 우수)

---

## 한줄평

> RNN 시대를 끝내고 Transformer 시대를 연 논문. GPT, BERT, 그리고 현재의 모든 LLM은 이 논문의 Self-Attention 메커니즘 위에 세워져 있습니다. 논문 제목 그대로 — "Attention Is All You Need."

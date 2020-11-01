# 논문 요약: Convolution Neural Networks for Sentence Classification

[논문 링크](https://arxiv.org/abs/1408.5882) 

## 서론

본 연구에서는 하나의 레이어를 가진 간단한 CNN 모델을 훈련시켜서 여러가지 sentence level의 분류classification 관련 NLP task에 적용하였다.

## 모델 구조

1. Nxk 차원의 문장 representation을 인풋으로 함
2. Convolution layer를 적용함 (여러 개의 필터)
3. Max-over-time pooling
4. Dropout과 softmax를 적용한 fully connected layer

## 데이터셋 실험 셋팅

영화리뷰/문장/질문/구매평 등에 대한 분류를 학습할 수 있게 하는 데이터셋

- MR: 영화 리뷰+postive/negative
- SST-1: MR의 확정인데, train/dev/test 쪼개져있음 + vpos/pos/neu/neg/vneg
- SST-2: neutral이 제거된 SST-1
- Subj: 문장 + 주관적/객관적
- TREC: 질문 + 6가지유형(사람/장소/숫자…)
- CR: 상품에 대한 구매리뷰 + pos/neg
- MPQA: 의견의 극단성?

## Model variation

- Cnn-rand: 베이스라인 모델. 모든 단어가 랜덤 초기화되며 학습할때 변화됨.
- Cnn-static: pre-trained word2vec을 사용한 모델. 모든 단어들은 static하게 고정되며 다른 parameterㄴ 수정됨
- Cnn-non-static: 위와 유사하나 pre-trained vector가 fine-tune된다.
- Cnn-multichannel: 2가지 셋의 word vector를 가진 모델이다. 각각의 셋은 channel로 취급되며 각각의 필터는 두 채널에 ㅗ두 적용됨. 두 채널 모두 word2vec을 통해 초기화됨

## 결과

- Baseline 모델: 성능 별로
- Cnn-static: 성능이 확 뛴다.
- Cnn-non-static: fine-tuning은 성능을 개선해준다.
- Multichannel vs single channel 멀티채널이 overfitting을 막아주길 기대했음. (오리지널 값으로부터 너무 벗어나지 않게함) 그러나 실제 결과는 mixed였다.
- Static과 non-static
- 싱글 채널 non-static, 멀티 모델은 non-static 채널을 fine-tune할 수 있다.
- 예를 들어, good은 word2vec에서는 bad와 가장 비슷했다 왜나면 syntactic한 면에서 대등하기 떄문이다
- 그러나 non-static channel에서의 벡터에서 sst-2로 finetuning했더니 nice와 더 비슷해졌다.
- Finetuning은 더 의미있는 representation을 학습하게 한다.
- Pretrain안돼서 랜덤초기화된 토큰들은 finetuning을 통해 의미있는 뜻을 학습하였다.

## 결론

- 간단한 CNN 모델에서 가볍게 hyperparameter 튜닝만 하면 여러가지 benchmark에 대해 탁월한 성능을 보였다.
- (7개의 과제중 4가 sota 성능을 능가하였다: sentiment analysis, question classification)

# 논문 요약: **Character-Aware Neural Language Models**

[논문링크](https://arxiv.org/abs/1508.06615)

## 서론

본 연구: CNN 모델의 아웃풋을 RNNLM모델의 인풋으로 사용되게 할 것이다.

즉, Highway network over characters를 사용하는데, 이는 CNN을 돌린 아웃풋을 LSTM 모델로 전달하는 것이다.

다음은 contribution이다.

1. 영어에서, Penn Treebank에서 60% 적은 parameter를 가지고 sota를 달성했다.
2. 아랍어,체코,불어,독어,서어,노어 등 morphologically rich(형태 다양)한 언어에서도 우리의 모델은 베이스라인을 능가하였으며 parameter를 적게 사용함.

## 모델 구조

전통적인 NLM은 워드 임베딩을 인풋으로 받는 데 반해

우리 모델은 싱글레이어 캐릭터레벨 CNN의 아웃풋을 받는다.

1. Character embeddings
2. Character-level CNN (multiple filters)
3. Max-over-time pooling layer
4. Highway network
5. LSTM network
6. Softmax를 거쳐 다음 단어를 예측함

실험 셋팅: English Peen Treebank(PTB)를 학습함.

## 결과

1. 영어에서, Penn Treebank에서 60% 적은 parameter를 가지고 sota를 달성했다.
2. 아랍어,체코,불어,독어,서어,노어 등 morphologically rich(형태 다양)한 언어에서도 우리의 모델은 베이스라인을 능가하였으며 parameter를 적게 사용함.

## 결론

- 본 연구에서는 오직 character-level의 인푸슬 사용한 neural language model를 소개했다.
- 예측은 word-level에서 이루어졌다.
- Parameter를 적게 가졌음에도 불구하고, word/morpheme(형태소) 임베딩을 이용한 다른 베이스라인 모델보다 성능이 좋았다.
- 문자 단위의 인코딩이 시맨틱한 특성을 인코딩할 수 있다.
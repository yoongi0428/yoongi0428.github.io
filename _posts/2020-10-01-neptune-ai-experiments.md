---
title: neptune.ai를 활용하여 실험 관리하기
author: yoonkij
date: 2020-10-01 09:00:00 +0900
categories: [Dev, Experiments]
tags: [neptune-ai, experiment]
render_with_liquid: true
---

### Introduction

딥러닝 모델을 만들고 실험하여 성능 평가를 할 때, 공통적으로 하게 되는 흐름이 있다.

가장 Naive하게는 매 실험 결과를 Stdio로 출력하고 이를 Excel에 일일이 정리하는 것이다. 그러다 실험이 Stdio를 File 출력으로 전환하여 항상 앞에 앉아 있어야 하는 자유를 얻었다. 하지만 여전히 Excel '파일'에 정리해야 하기에 다른 서버, 다른 작업 공간에서 실험 결과를 확인하는 데에 어려움이 있다. 다른 작업 공간은 Google Spreadsheet로 해결이 가능하지만, 여러 서버에서 실험을 돌릴 때,

1. **실험이 오류없이 잘 돌아가고 있는지**
2. **실험 진행 상황이 어떻게, 얼마나 되고 있는지**
3. **여러 실험 결과를 한번에 모아 비교할 수는 없는지**

등은 나를 오랫동안 골치아프게 했다.

이런 고민은 역시 나 혼자만의 고민이 아니었나보다. 얼마전 한 블로그 (아래)를 통해 많은 실험 관리 서비스가 존재한다는 것을 깨닫고, 이 중 **Neptune**을 적용해보기로 하였고, 그 결과를 아래와 같이 공유하고자 한다.

### Neptune.AI

> The most lightweight experiment management tool that fits any workflow

[Neptune.ai](https://neptune.ai/home) 홈페이지를 들어가면 위 문구를 볼 수 있다. 
별 다른 복잡한 구조와 코드 수정 없이 실험을 한번에 정리 할 수 있다.

구체적인 특징으로는,

1. **Python, R, Jupyter notebook + more 에 적용 가능하며**
2. **사용 패키지에 무관하게 적용 가능하고**
3. **Tensorboard, MLflow 등의 Tool과도 연동이 가능하다.**

자세한 내용은 공식 문서를 참고 부탁드린다. 또한, 자체 Blog에도 유용한 내용이 꽤 있으니 심심할 때 읽어보는 것도 좋다. 개인 연구자, 학생 등은 무료로 제한적 이용이 가능하다. 제한적 이용이라고는 하지만 부족함을 느끼지는 못했다.

공식 문서: [https://docs.neptune.ai](https://docs.neptune.ai/)
블로그: [https://neptune.ai/blog](https://neptune.ai/blog)

### Neptune 설치하기

Neptune은 pip를 통해 설치할 수 있다. 주의해야할 점은 그냥 neptune이 아닌 **neptune-client**를 설치해야 한다는 점이다. 추가적으로 하드웨어 사용 추적을 위해서 psutil 라이브러리를 설치할 수도 있다.

```python
**pip install neptune-client psutil**
```

### Neptune 적용하기

PyTorch, Torchvision을 활용한 간단한 MNIST 분류로 Neptune을 사용해보겠습니다. **모든 코드는 [여기](https://github.com/yoongi0428/neptune_logger)**에서 보실 수 있으며 **MNIST 분류 코드는 [이 블로그](https://nextjournal.com/gkoehler/pytorch-mnist)**를 참고하였습니다.  MNIST 불러오기, 모델 정의 등 Neptune과 관련 없는 내용은 이 글의 주제에서 벗어나기에 생략하겠습니다.

구현한 **"NeptuneLogger"**를 사용하기 위해서는 몇가지 argument를 넣어주어야 합니다.

![](https://velog.velcdn.com/images/yoongi0428/post/c16dc5ac-843c-4da9-9bb5-4bbf2b248dc9/image.png)


이 중, API Token은 개인별 할당된 코드로 타인에게 공개될 경우 Neptune에 접근할 수 있으니 주의가 필요합니다. **API Token은 로그인 후 우 상단 "Getting Started"**를 누르면 아래와 같이 확인할 수 있으며, 사용시에는 **코드가 아닌 환경변수로 넣는 것을 추천**합니다.

![](https://velog.velcdn.com/images/yoongi0428/post/3bf53a0a-f2d9-4bf4-90be-e0adc4c4be37/image.png)


### Neptune 활용하여 Logging하기

간단한 예제로 Neptune을 활용하여 Hyper-parameter, Metric, Artifact (trained weights, files ...), Image를 Logging 해보겠습니다.

**Hyper-parameter Logging**

![](https://velog.velcdn.com/images/yoongi0428/post/fb7df84d-3099-43e5-b04a-a35549bb68c1/image.png)


실험 hyper-parameter는 보통 dictionary로 관리하게 됩니다. 이 dictionary를 그대로 **log_hparams**에 넘겨주면 NeptuneLogger가 기록합니다. 예제에서는 흔히 사용되는 ArgumentParser를 활용하였습니다.

**Metric Logging**

![](https://velog.velcdn.com/images/yoongi0428/post/d549a27b-7f81-4016-87cd-bd21ab6a7e15/image.png)


실험을 하다보면 Loss와 Accuracy등 Metric을 Epoch에 따라 기록하여 학습 상황을 확인합니다. **log_metric**이라는 함수에 ('이름', 값, EPOCH)을 넣어 기록할 수 있습니다. 만약 EPOCH이 없이 최종 값을 기록하고 싶다면 EPOCH에 아무 값도 넣지 않을 수 있습니다.

**Artifact (학습된 가중치, 로그 파일 등) Logging**

![](https://velog.velcdn.com/images/yoongi0428/post/c86a22fa-12e7-4d83-bf34-71849eecb5ef/image.png)


실험으로 얻은 최종 모델의 가중치, 실험 로그를 기록한 파일 등 상황에 따라 파일을 통째로 기록하고 싶을 수 있습니다. **log_artifact**함수를 통해 기록이 가능합니다.

**Image Logging**

![](https://velog.velcdn.com/images/yoongi0428/post/f51bf38c-65f0-4bf4-8e69-b037b33e4948/image.png)


실험이 끝난 후, Epoch에 따른 Loss등 matplotlib을 통해 Plot을 그리거나 모델이 생성한 이미지 등을 기록할 수 있습니다. log_image에 matplotlib의 figure 객체를 그대로 넣어주거나, 이미지 파일명을 명시하여 가능합니다.

### Neptune으로 실험 확인하기

Neptune을 활용할 때, 주로 사용한 기능은 주로 아래와 같습니다.

1. 실험이 진행 상황 수시로 확인 (잘 돌아가는 지, 얼마나 걸렸는 지, 끝났는 지 등)
2. 실험 최종 성능 확인 및 비교
3. Tag를 통한 실험 필터링
4. Plot, Chart 그려서 확인

![](https://velog.velcdn.com/images/yoongi0428/post/8ffe2a84-6452-4a59-8ec9-ffab53c48286/image.png)


실험을 실행하고 Neptune에 접속하여 Project를 클릭하면 위와 같은 화면을 보게됩니다. 이미 끝난 NEP-4 실험과 현재 진행중인 NEP-5 실험이 보입니다.이 테이블의 View는 커스터마이징이 가능하며 저 같은 경우는 위와 같이 설정합니다. 태그를 클릭하면 특정 태그에 해당되는 실험이 필터링되어 화면에 보여집니다. (아주 유용!)

![](https://velog.velcdn.com/images/yoongi0428/post/610f1106-b06a-48eb-8cc1-fe1172aaa245/image.png)
![](https://velog.velcdn.com/images/yoongi0428/post/43fbea7f-0779-475e-9403-3ff62a60687c/image.png)


NEP-5를 클릭하여 왼쪽을 보면 Log, Charts 등 메뉴가 있습니다.Log를 클릭 할 경우 위와 같이 log_metric을 통해 기록한 Train loss, Test Acc, 등을 볼 수 있고 Chart를 클릭하면 아래와 같이 Train loss가 그려집니다. 원하는 모양대로 Chart를 만들 수도 있지만 지금은 생략하겠습니다.

![](https://velog.velcdn.com/images/yoongi0428/post/c8f3ce46-afda-48b6-8d47-54af4acdedbb/image.png)
![](https://velog.velcdn.com/images/yoongi0428/post/c7dc0a87-ef9a-4363-979d-cca724f39481/image.png)


Log 란에서 'Train Loss Plot'은 코드를 통해 기록한 Matplotlib Image입니다. 왼쪽과 같이 나오며 클릭시 확대가 됩니다.Artifact를 누르면 오른쪽과 같이 학습한 모델 가중치가 올라와있으며 다운로드도 가능합니다.

### 마치며...

이번 글에서는 Neptune을 (아주) 간단히 소개하고 기능을 설명하며 실험 코드에 바로 적용할 수 있는 구현체를 제공하였습니다. 더 자세하고 많은, 커스토마이징이 가능한 기능들이 있으니 공식 문서와 여러 적용 사례 등을 살펴보시면 좋습니다!

도움이 되었길 바라며, 모두들 편-안한 실험 관리 하세요!
---
# the default layout is 'page'
icon: fas fa-info-circle
order: 1
---

> 마지막 업데이트: 2023-10-17

**`Email`**: yoongi0428@gmail.com   **`Phone`**: 010-3761-3368

**`Github`**: [https://github.com/yoongi0428](https://github.com/yoongi0428)

**`Linkedin`**: [https://www.linkedin.com/in/yoon-ki-jeong-8aa920158/](https://www.linkedin.com/in/yoon-ki-jeong-8aa920158/)

**`Blog`**: [https://yoongi0428.github.io/](https://yoongi0428.github.io/)

> 🔥 **결과를 내고싶은 3년차 개발자, 정윤기입니다.**
> 
> 3년차 개발자로 네이버에서 웹 검색 모델 개선을 위한 업무를 해왔습니다. 웹 검색 랭킹팀으로 입사하여 학습 데이터 생성, 랭킹 모델 피쳐 관리부터 이 후 품질 평가, 저품질 탐지 업무, 대규모 검색로그 정제 등을 경험하며 검색 서비스에 대한 폭 넓은 경험을 하였습니다.
> 
> 결과물을 만들어 내는 것을 중요시합니다. 학부연구생 시절 동료들과 Tensorflow 기반으로 문제에 맞는 모델 아키텍쳐와 오랜 시간의 튜닝으로 RecSys 2018 2위에 입상하였습니다. 대학원 재학 시절에는 연구의 크기와 관계없이 내용을 정리하여 논문의 형태로 만들었습니다. 또한, 네이버 입사 이후에는 익숙한 Python은 물론 필요하다면 Django, Scala, Spark를 모두 활용하여 주어진 업무를 마무리 하는데에 집중했습니다.
{: .prompt-tip }

---
## Work Experience
### **Research Engineer @ 네이버** (2021.04 ~)
  
#### ***Quality Prediction Team***
- **공식정보 적합성 마이닝**
	- 링크 기반 키워드 마이닝: 주요 도메인의 HTML 태그를 분석하여 주요 링크 그룹 패턴을 발굴하고 *Mapreduce*와 *Spark*를 활용하여 공신력있는 정답형 질의-문서 쌍을 대량 생성
	- [관련 네이버 공식 블로그](https://blog.naver.com/naver_search/222916797141)
- **검색 품질 대량 자동 평가 지표**
	- 평가 대상 질의를 다수의 내부 데이터와 BERT-based 모델을 조합하여 품질 평가 지표 생성
	- 여러 단계의 Spark job을 airflow로 묶어 자동화
- **저품질 탐지:** 대량 평가 질의 대상으로 부착된 다수 지표로부터 개선 필요한 질의를 패턴 마이닝

#### ***Web Search Ranking Team***
- **랭킹 모델 학습 데이터 생성**: 스팸 혹은 저품질 결과 랭킹 모델 학습 데이터 생성
- **피쳐 관리 툴 개발 및 유지보수**
	- Django로 구현된 피쳐관리 도구의 신규 기능 개발
	- 사내 계정 인증 시스템 연동과 mysql DB를 활용한 접근성 제어
	- 랭킹 모델 업데이트에 따른 피쳐 중요도 변화 비교를 통한 모델 이해도 향상

---

## Education
#### **성균관대학교 전자전기컴퓨터공학과 석사** (2019.03 - 2021.02)
- **Data Intelligence And Learning Lab.** (지도교수: 이종욱 교수님)
- **연구분야**: 추천시스템, 정보검색
- **학위논문**: *Performance Comparison and Analysis between 
Neural and Non-neural Autoencoder-based Recommender System*

#### **성균관대학교 글로벌경제학과 / 컴퓨터공학과 학사** (2012.03 - 2019.02)
- 전공학점: 4.28/4.5

---

## Publication
1. Jaewan Moon, **Yoonki Jeong**, Dong-kyu Chae, Jaeho Choi, Hyunjung Shim and Jongwuk Lee. “CoMix: Collaborative filtering with mixup for implicit datasets”, *Information Sciences*, 2023.
2. Minjin Choi, **Yoonki Jeong**, Joonseok Lee and Jongwuk Lee. “Local Collaborative Autoencoders”, *14th ACM International Conference on Web Search and Data Mining (WSDM)*, 2021.
3. **Yoonki Jeong** and Jongwuk Lee. “Performance Comparison and Analysis between Neural and Non-neural Autoencoderbased Recommender System”, *Journal of Korean Institute of Information Scientists and Engineers (JOK)*, 2020.
4. **Yoonki Jeong**, Dongmin Kim, Jongwuk Lee, A CNN-based Column Prediction Model for Generating SQL Queries using Natural Language, *Journal of Korean Institute of Information Scientists and Engineers (JOK)*, 2019.
5. Dongmin Kim, **Yoonki Jeong**, Jongwuk Lee, A Column Prediction Model for Generating SQL Queries using Natural Language, *Korea Software Congress (KSC)*, 2018.
6. Hojin Yang, **Yoonki Jeong**, Minjin Choi, Jongwuk Lee, MMCF: Multimodal Collaborative Filtering for Automatic Playlist Continuation, *ACM RecSys Challenge 2018*, 2018.
7. **Yoonki Jeong**, Minjin Choi, Hojin Yang, Jongwuk Lee, Frequency-Based Syllable Embedding for Korean Natural Language Processing, *Korea Software Congress (KSC)*, 2018.

---

## Honors and Rewards
### 2nd Place in RecSys Challenge 2018 (2018.10)
*ACM RecSys 2018, Vancouver, 2018*
* Spotify에서 주최한 *RecSys Challenge 2018*에서 2등을 수상
* **대회 주제**: **Automatic Playlist Continuation(APC)** - 주어진 플레이리스트와 그 트랙들의 메타데이터를 기반으로 이어질 트랙들을 추천
* **솔루션**: Autoencoder 기반 Collaborative Filtering 모델과 CNN 기반 자연어 모델을 결합한 멀티모달 추천 모델로 플레이리스트 생성에서 고려해야 할 다양한 문제를 해결
* **담당**: Character-level CNN을 기반으로 플레이리스트 제목 - 트랙 사이의 패턴을 학습하는 추천 모델 개발 (Tensorflow v1.5)
* 관련 링크: [Paper](https://hojinyang.github.io/papers/MMCF18.pdf) / [Github](https://github.com/hojinYang/spotify_recSys_challenge_2018) / [Challenge](http://www.recsyschallenge.com/2018/)

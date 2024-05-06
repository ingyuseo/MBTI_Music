# MBTI_MUSIC
MBTI 기반 노래 추천 시스템

## Introduction

Problem. 기존 음악 추천 시스템의 한계
- 음악 자체를 분석하기 보다는, 장르나, 템포 등의 데이터를 이용. 비슷한 유형의 노래만 추천하는 경향.
- 개인의 특성을 반영하는 요소가 부족

Solution. 음악 자체 분석 + MBTI로 개인 특성 적극반영

- 음악의 파형에서 추출한 MFCC 계수를 학습에 이용 -> 음악 Raw Data에서 다양한 특성 활용 가능
- Spotify의 MBTI 관련 플레이리스트에서, 선호 음악 데이터 추출 -> 성격, 기질 특성 반영
- Transformer로 MFCC에서 feature 추출 후, MBTI별(16개) Decoder를 활용해 각각의 추천 점수 반환.


## 학습 방법

데이터 : Spotify에서 MBTI 데이터 크롤링
ex) INTP라고 치면 -> 한 플레이리스트에서 랜덤으로 5곡 추출 -> 한 MBTI당 100곡씩 총 1600곡

노래와 MBTI 형태로 데이터 설계
ex) 
아무 노래.flac | INFP 
이런 노래.flac | INTP
...

해당 음악 파일이 입력으로 들어왔을 때, 해당 MBTI의 값에 대한 추천 확률이 높도록 학습
정답 레이블 : 해당 MBTI에는 1.0 , 나머지는 0.4로 설정 (좋아할수도 안할수도 있음)

Loss Function : Weighted BCELoss
- 해당 정답 레이블에 대해서 틀릴 경우 가중치를 더 부여함.
- 왜냐하면 15개의 0.4 레이블 때문에 전체적으로 0.4를 반환하려는 경향이 있기 때문.


## 시스템 구성

![image](https://github.com/ingyuseo/MBTI_Music/assets/38852476/133e9528-9d6f-486e-91ac-f0a7ed624aaa)

## 서비스 

![image](https://github.com/ingyuseo/MBTI_Music/assets/38852476/b2ba92cb-e502-481e-9cb1-3915a8f43d87)


- fast api와 react로 구성
- 음악 검색 시 추천 점수 반환 + 저장된 데이터로 추천 플레이리스트 반환

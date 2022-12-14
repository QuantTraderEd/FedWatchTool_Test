![OneDollar](/img/onedollar.png)
# CME FedWatchTool 분석
FedWatch Tool 방법론에 따라 실제 데이터로 계산 및 검증  

# **CME그룹 FedWatch Tool 과 Fed Funds Futures (미연방금리선물) 확률트리 계산기 이해하기**
[영문 원본](https://www.cmegroup.com/education/demos-and-tutorials/fed-funds-futures-probability-tree-calculator.html)  
[한글 원본](https://www.cmegroup.com/ko/education/fed-funds-futures-probability-tree-calculator.html)  
## **FedWatch Tool의 가정과 해석:**

- 미 연방기금금리 (FED Fund Rate) 상승 확률은 현재 타겟금리 위에 있는 모든 타겟 금리의 확률을 합산하여 계산
- 미 연방기금금리 별 확률은 Fed Fund 선물 계약의 가격에 근거하여 계산되며 상승폭은 0.25% 단위로 가정하며  
  미 연방기금유효금리 (Fed Funds Effective Rate, FFER) 은 같은 폭으로 반응하게 될것이다.   
  ※ Fed Funds 선물 Spec 요약  
    30 일 Fed Fund 선물은 해당월 마지막 거래일은 해당월 마지막 영업일, 정산은 다음달 첫 거래일 현금 정산,   
    해당월 평균 Fed Fund Overnight Rate 으로 정산,  
    1bp 의 1/10 에 가장 가까운 값으로 반올림,  
    일별 Fed Fund Overnight Rate은 뉴욕연방은행에서 발표
     
- FOMC 회의 확률은 이에 대응되는 CME 그룹 Fed  Fund (FF) 선물 계약에 의해 결정된다.
- FOMC 회의는 분기당 한달은 회의 일정에 포함되지 않고 대부분 일정한 분포로 게시된 일정에 따라 진행.   
  (실제로 1년에 8번, 6주 간격으로 진행)
- 각 기간 별 예상 FFER 의 마지막 금리는 그 다음 기간의 FFER 시작 금리와 동일
- 위의 가정을 기반으로 계산된 확률이 추정 되었으며 어떤 원칙이 손상된 경우 변동될 수 있음

## 방법론:

FedWatch Tool 은 FOMC 회의 결과의 비조건부 확률을 계산하여 이진 확률 트리를 생성합니다. 

CME 그룹은 30일 Fed Fund (FF) 선물이 상장되어 있고 이 선물가격에는 선물계약의 일평균 FFER 수준에 대한 시장 기대치가 반영되어 있습니다. 

(예를들어 FFU5 시장가격은 2015년 9월 한달동안 평균 FFER 레벨의 시장 컨센서스 기대값을 반영한다.) 

**FFER 은 뉴욕연방준비은행에서 매일 발표하고 있으며 예금기관 간 거래 되는 무담보 1일물 대출(loan) 시장에서 주요 브로커에 의해 취급된 전일자 거래 금리의 거래량 가중 평균값으로 계산**되고 있다.

([뉴욕연준 Fed Funds Market Rate](https://www.newyorkfed.org/markets/reference-rates/effr))

(일부 금융사(니커보커) 파산 사건(1907년), Fed Reserve 설립으로 1914년 증권 담보비율을 높여서 Call 시장에서 우량 금융기관만 참여가능한  Fed Funds Market 으로 전환)

FedWatch Tool 의 확률분석은 금리 변동의 크기는 25bp이고  FOMC 회의 월의 이전 이후 FF 선물 가격은 회의 결과와 독립적 혹은 전적으로 회의 결과에 의존적인 정보를 포함하고 있다고 가정합니다. 

또한 FedWatch Tool 은 FFER 금리는 0 이하의 값은 제한되어 있다고 가정합니다.  

개별 **FF 선물 계약은 계약월의 일별 평균 FFER 의 기대값을 의미**하기 때문에, 만약 이전 달에는 FOMC 회의가 없었고 이번 달에 있는 경우, 
이전달의 FF 선물의 가격은 현재 월 FOMC 회의 와는 독립적으로 정보를  포함한다. 마찬가지로 현재 월에는 FOMC 회의가 있고 다음달에는 계획된 FOMC 회의가 없다면  
다음 FF 선물 가격은 현재 월의 FOMC 회의 결과의 정보만 반영한다.  

현재 월에 FOMC 회의가 한번 있고 일별 FFER 타겟 금리가 올릴지 현상태를 유지 할지 결정한다고 하면 금리상승  확률과 보합 및 하락 확률은 아래와 같다.

P(상승) = [ FFER(월말) – FFER(월초) ] / 25 basis points

P(보합) = 1 – P(상승)

FOMC 가 일별 FFER 타겟을 레밸로 설정하든 범위로 설정하든 FF 선물가격이나 FOMC 회의 결과의 내재 확률을 계산하는것에 대해 영향을 미치지 않을 것이다. 왜냐하면 월초 FFER 과 FFER 의 비교를 기반으로 계산되기 때문이다. 

FOMC 타겟 레밸의 변화가 25b로 주어진다면 (주어진 타겟 레밸이 변하든 범위가 변하) 금리 변동확률은 월말 타겟 기대치와 월초 타겟 기대치의 차이와 연계될 것이다.

현재 월 FOMC 회의에서 타겟 변경에 대한 비조건부 확률을 구하기 위해서 우선적으로 고려사항은  FOMC 회의가 이전월에 있었는가 이다. 만약 이전달에 회의가 없었다면 “Type2 회의”로 분류하고 그렇지 않으면 “Type1 회의”로 분류한다. (영문 테이블이 예시와 맞지 않아 한글 테이블 준용)
  
변수|Type 1 (이전달에 회의가 있는 경우)|Type 2 (이전달에 회의가 없는 경우)|
|:---|:---:|:---:|
|N|Days in Meeting Month (회의 월의 일수)|Days in Meeting Month (회의 월의 일수)|
|M|Day(MeetingDate) – 1 (회의 일까지 일수)|Day(MeetingDate) – 1  (회의 일까지 일수)|
|FFER.Start|(N/M) * [Implied Rate – FFER.End*((N-M)/N)]|(100 – FF.MonthBefore)|
|Implied Rate|100 – FF.MeetingMonth|100 – FF.MeetingMonth|
|FFER.End|100 – FF.MonthAfter|(N/(N-M)) * [Implied Rate – (M/N)*FFER.Start]|
  
FFQ5 = 99.8675 (FF 선물 15년 8월 물, FOMC 회의가 없는 월)  
FFU5 = 99.805   (FF 선물 15년 9월 물, FOMC 회의가 있는 월)  
N = 30  (9월달은 30일)  
M = 14  (8월 31일 기준)  
  
FFER.Start = 0.1325 (100-99.8675)  
ImpliedRate = 0.195 (100-99.805)  
FFER.End = 30/14* [0.195 – (16/30)*0.1325]  
                = 0.26643   
  
P(Hike) = (0.26643 – 0.1325) / 0.25 = 53.6%  
P(NoHike) = 46.4%  
  
FedWatch tool 은 각 회의 일자 마다 비조건부 확률을 계산한 후 이 정책결정 트리를 계산한다.  
이진 트리의 첫번째 노드는 두가지 결과에 대한 확률을 가지고 있다.  
(1) 현재 타겟금리 유지, (2) 타겟금리 변경 (25bp 상승 혹은 25bp 하락).   
현재의 예시와 다음예시는 두가지 결과만 나타낼것이다. 예를들어 상승 혹은 보합, 하락 혹은 보합 ([노트1](#노트1))  
두번째 노드에서는 두번째 회의에서 타겟 금리를 올리거나 올리지 않거나에 대한 기대를 가정하고   
두번째 회의에서 타겟금리 하락의 확률, 타겟금리가 변동하지 않을 확률, 타겟금리가 상승할 확률을 계산하게 된다.  
  
산식은 아래와 같다. ([노트2](#노트2)) 

P(FFER 하락) = Probability(FFER 이전하락) * (1-금리변화확률)  
P(FFER 보합) = Probability(FFTR 이전 상승) * (1-금리변화확률)  
+ (Probability of a FFTR 이전하락) * (금리변화확률)  
P(FFER 상승) = (Probability FFTR 이전상승)* (금리변화확률)  
  
#### 노트1
FOMC 회의는 매번 25bp 만큼 움직일수 있다는 가정을 다시 상기 해보자.   
기대 FFR 금리 (선물가격에 내재되어 있는) 현재 유효 금리 보다 25bp 이상이 될수 있다.   
이러한 시장상황에서는 25bp 이상 올라갈 확률이 내재되어 있다.  
  
이러한 경우는 확률이 100%를 초과하게 될것이다. 해석의 단순화를 위해서 계산되어지는 확률은   
Single Hike 와 Two Hike 사이에서 재 배정된다.   
- 100%가 초과 되는 확률은 50bp 상승 확률로 정의가 되고 다음 상승확률은 P(Hike/50bp)가 된다.  
- Single Hike 확률 P(Hike/25bp) 는 1-P(Hike/50bp)로 정의한다.   
  
예시: 선물계약의 높은 내재금리로 인해 25bp 인상확률이 104% 로 계산된다면  
위의 사례를 적용하여 확률을 P(NoHike)=0%, P(25bpHike) = 96%, P(50bpHike) = 4% 로 재조정  
  
#### 노트2
시장 해석과 가정에 기초하여 FFER 은 zero 이하는 제한되어 있다.   
두번째 노드의 시나리오는 다음과 같다.두번째 회의 FFER 보합확률,   
FFER 인상확률 (혹은 첫번째 노드에서 금리 인상 두번째 노드에서 금리 인하 확률),   
첫번째 두번째 회의 모두 인상할 확률.  
  
- P(FFER 보합) = Probability(FFER NoHike | 이전 회의) * (1-Probability of 금리변화 확률)  
- P(FFER 이번회의가 첫번째 인상 or 첫 회의 상승 두번째 회의 하락) = Probability(FFTR hike | 이전회의) * (1-금리변화확률) + (Probability of a FFTR NoHike | 이전회의) * (금리변화확률)  
- P(FFER 연속두번상승확률) = (Probability FFTR Hike | 이전회의)* (금리변화확률)                                                         

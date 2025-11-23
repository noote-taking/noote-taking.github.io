---
layout: single
title: "MDE는 어떻게 설계해야 되는가? (Ver.1)"
categories: 통계학
tags: [ab-test]
toc: true
author_profile: false
search: true
use_math: true
typora-root-url: ../
---



> **요약** <br>
>
> - 1안: 업계 표준 기준을 사용한다. (상한선 5%)
> - 2안: 과거의 경험으로 비즈니스 적으로 유의미한 개선에 대한 수준을 정한다.
> - 3안: 검정력 공식을 역으로 사용해서 검정 가능한 MDE를 확인한다. 



실험 설계 전에 최소 샘플 사이즈를 계산하기 위해서는 MDE를 설계해야한다. 그런데 어떻게 설계를 하면 될까? 이 부분이 실무자 입장에서는 상당히 어려운 부분인데, 가장 큰 이유는 대개의 경우가 베이스라인을 설정할 수 있을 만큼 회사 내에 실험 데이터가 없다는 점이다. 

비즈니스적으로 유의미한 수준의 기준을 잡아야 한다는 말도 다소 추상적인데, 그 이이유는 간단하다. 회사에서는 20%, 50% 이상의 높은 개선일수록 좋겠지만 이는 현실적이지 않으며, 적당히 10% 정도가 좋을까? 라고 생각해도 통계적으로 유의하게 10% 정도가 올랐던 실험 사례가 많지 않다면 그걸 베이스라인으로 정하기에는 확신이 없을 수 있다.

나 또한 그 기준을 어떻게 잡아야 하는가? 에 대해서는 여전히 정답을 찾지는 못했지만, 지난 2년여간 약 100개의 실험을 진행하면서 조금은 구체화 된 내용이 있어서 그 기록을 남겨본다. (언젠가는 더 명확한 기준을 설계할 수 있기를 기원하며 포스팅 제목을 Ver.1으로 작성..)

## 1안. 업계 표준 기준을 사용한다

가장 심플하지만 가장 어려운 방법이다. 왜냐하면 업계 표준에 대해서는 알려진 자료가 별로 없기 때문이다. 특히 본인이 담당하는 서비스가 틈새 시장에 속하는 경우라면 더욱더 참고할 만한 기준을 찾기 어렵다.

내 경우에는 Ron 박사님의 [Why 5% should be the upper bound of your MDE in A/B tests](https://www.linkedin.com/pulse/why-5-should-upper-bound-your-mde-ab-tests-ron-kohavi-rvu2c/) 게시물을 참고했는데, 여기에는 왜 MDE의 상한을 5%로 잡아야 하는지에 대한 내용들이 작성되어 있다. 

우선 박사님이 게시물을 통해 직접 공유한 사례 두 가지는 아래와 같다.

1. Airbnb allowed me to state that in 1.5 years of leading search relevance, we improved booking conversion by 6% by running 250 experiments, of which 20 were successful. The average successful experiment improved conversion by 0.3%. <br>➤ **1.5년동안 250개의 실험을 진행하며 예약 전환율 6%를 개선했지만, 성공한 실험 하나당 평균 개선율은 0.3% 수준**
2. At Bing, hundreds of people worked to improve the cumulative OEC of Bing’s relevance by 2% every year. In https://eduardomazevedo.github.io/papers/azevedo-et-al-ab.pdf, the success-rate metric (a component of the OEC) varied from -0.22% to 0.28% (Table 1). <br>➤ **1,450개의 실험중 가장 낮은 실험의 효과는 -0.22%, 가장 높은 실험의 효과는 0.28%, 평균 효과는 -0.001%** 

<br>즉, Airbnb에서 성공한 실험의 평균 개선율은 0.3% 수준이고, Bing과 같이 성숙한 서비스에서는 관찰되는 실험의 효과가 매우 적다는 것이다. 물론 최적화가 덜 된 제품으로 작업한다면 이보다 더 큰 효과를 목표로 해야겠지만, 그걸 고려하더라도 제품 개선 실험의 경우 5%의 MDE가 합리적인 상한선으로 보인다는 의견이다.

물론 이 내용은 서비스 도메인의 특성 외에도, 앱이 아닌 인터넷이 서비스의 주력 무대였던 시절의 데이터임을 고려할 필요는 있어보인다.



## 2안. 과거의 경험으로 비즈니스 적으로 유의미한 개선에 대한 수준을 정한다

개인적으로는 2안이 가장 좋은 방법이라고 생각한다. 회사마다 서비스의 형태와 실험의 종류가 한정될 수 있으므로 각 도메인별, 기능별, 가설별로 실험 데이터가 많이 있다면, 이번에 진행하는 실험에서는 목표치를 어느정도로 잡아야할지 감을 잡기 수월하기 때문이다.

하지만 대부분의 초기 스타트업은 1년에 잘 설계된 실험을 20번 하기도 힘든게 현실이라 생각한다. 결국 지금 회사 내부에 데이터가 없다면 실험 환경을 구축하고, 더 많은 가설을 실험을 통해 검증하길 권장한다.

개인적으로는 지금까지 진행한 약 100여 개의 실험들 중에서 최근 3분기에 진행한 실험들에서 유의미한 결과들이 제법 나왔는데, 그중에서도 최소 샘플사이즈를 만족하면서 p-value < 0.05인 실험은 하나가 있었고, 약 7.x% 수준의 지표 개선이 있었다. 

승률 1%.. 실험 노하우 내재화를 위해서는 더 부지런히 움직여야 한다.



## 3안. 검정력 공식을 역으로 사용해서 검정 가능한 MDE를 확인한다. 

이 방법은 기존의 검정력 공식인 $n=16\sigma^2/\delta^2$ 을 $\delta = 4\sigma/\sqrt{n}$ 로 바꿔서, 우리 서비스에서 일정 기간내에 검증 가능한 효과의 크기를 역산하는 방법이다.

간단한 방법이지만 우리 서비스의 한계치를 직관적으로 살펴볼수 있는 쉬운 방법이라고 생각한다. 특히 실험에 대한 투자가 미흡한 조직의 경우, 2주 안에 실험이 종료되기를 바라는 경우가 있는데, 이럴 경우에 왜 좀 더 기다려야 하는지에 대한 설명을 하기에도 용이하다.

생각해 보면 실험을 하기 위해서는 내부적으로 생각보다 많은 준비가 필요하다. 랜덤할당 로직부터 평가 기준 수립, 데이터 정합성 고도화, 가설 검증을 위한 적절한 방식의 실험 설계까지. 만약여기 까지 왔다면, 1~2주 더 실험을 진행하는게 그렇게 큰 비용일까? 오히려 실험을 조기 종료하고 검정력이 낮은 상태에서 잘못된 의사 결정을 하는 것보다는 훨씬 얻는 게 많은 기다림 일 거라 생각한다. 



## References

[1] Kohavi, R. (2023). Why 5% should be the upper bound of your MDE in A/B tests. LinkedIn. https://www.linkedin.com/pulse/why-5-should-upper-bound-your-mde-ab-tests-ron-kohavi-rvu2c/

[2] Azevedo, E. M., Deng, A., Montiel Olea, J. L., Rao, J., & Weyl, E. G. (2019). A/B testing with fat tails. arXiv preprint arXiv:1505.03940v4.

[3] Kohavi, R. (2023). Using the statistical power formula in reverse. LinkedIn. https://www.linkedin.com/posts/ronnyk_using-the-statistical-power-formula-in-reverse-activity-7027144092459438080-2FTy/

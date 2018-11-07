mini-batch SGD

- 계산이 효율적 (parallel 시스템 이용)
- training set의 gradient와 비슷하게 학습



- but, lr을 낮게 측정, hyper parameter의 초기값을 조심 스럽게 츶정



- Internal covariate shift : 전 레이어의 파라미터에 따라, deep layer의 인풋의 distributon이 크게 달라지게 된다.
- input distribution의 변화는 큰 문제가 된다. 왜냐하면 각 레이어의 파라미터는 새로운 분포에 적응해야 된다. 
  - 고정된 input 분포는 다음 레이어에 긍정적인 영향이 있음. sigmoid activation 사용시, input이 큰 값일 경우, gradient vanish 현상이 발생



- 효과 : 학습 속도를 높혀주고, internal covariate shift를 줄인다.
- input의 고정된 mean과 variance를 사용
- regularize 효과



- Image net 실험시 7%의 train step을 이용해 성능을 더 끌어 올림.



- network와 parameter를 직접 수정할 경우, gradient 효과과 줄어들 수 있음

### Towards Reducing Internal Covariate Shift

- Internal Covariate Shift : network 내부 분포의 변화



- 기존 방법 : whiten (zero mean과 unit varianve, 이고 decorrelation을 만드는 작업)
  - 장점: layer마다 동일한 분포를 가짐
  - 단점: whiten을 이용해 학습시, gradient 효과를 줄일 수 있음
    - 예 u + b에서 b를 학습할 때, b가 변화해도 loss에 대한 변화가 없음.
    - scale이 있을 때, 더 심해진다.
    - 계산량이 많음 : cov matrix를 구하기가 힘듬.
  - 이유: gradient descent가 normalization이 일어났다는 것을 고려하지 않음. -> gradient decent에 normalization가 고려되어야 한다.

### Normalization via mini-batch statics

whiten : cost가 비싸고, differentiable의 불가능

- 스칼라 값으로 normalize
- identity transform을 위해 gamma와 beta가 저장 됨

